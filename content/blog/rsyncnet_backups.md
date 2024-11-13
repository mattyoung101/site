+++
title = "Borg and rsync.net: a really nice backup setup"
date = "2024-11-12T04:41:14+10:00"

description = "This blog post details setting up Borg and rsync.net for backups, and why they might be the best option."

tags = ["infra", "storage"]
+++

**Table of contents**

<!-- mtoc-start -->

* [Background](#background)
* [Evaluating the other options](#evaluating-the-other-options)
  * [Kopia](#kopia)
  * [Rustic](#rustic)
* [Cloud storage providers](#cloud-storage-providers)
  * [rsync.net](#rsyncnet)
  * [BorgBase](#borgbase)
  * [Backblaze B2/Wasabi/IDrive e2](#backblaze-b2wasabiidrive-e2)
  * [The elephant in the room: Australia](#the-elephant-in-the-room-australia)
* [Setting up Borg](#setting-up-borg)

<!-- mtoc-end -->

## Background
Recently, I decided I would like to migrate my backups from an external HDD to ✨ *the cloud* ✨. This idea
sort of came up while I was trialling Google Cloud, which I was doing in order to run a BOOM RISC-V tape-out
using [OpenROAD](https://openroad.readthedocs.io/) that needed more RAM than my 64GB workstation could
provide. Google Cloud Storage has an interesting Archive tier option, which I was planning to use to sync my
[Borg](https://borgbackup.readthedocs.io/en/stable/) backups. Unfortunately, I noticed that my WD external USB
HDD had died (!) after only a few years of non-intensive use (!), which meant this was no longer a viable
option.

![Ruh roh. Not btrfs this time, but actual physical disk corruption (notice the critical medium error)](/blog_nextcloud/disk_failure.png)

Borg cannot sync directly to the S3-like object stores, it needs a server on the other end which needs SSH.
So, with my backup drive dead, I started looking at other cloud based options.

## Evaluating the other options
The goal for any backup is to ensure long-term reliability and reduce cost. Google Cloud Storage Archive is
really nice for that, costing only US$0.0023 per GB per month to store data, which is instantly accessible too
(unlike Amazon Glacier). This isn't usable with Borg: as mentioned above, by design, Borg requires a server.
Lacking a usable HDD of sufficient capacity as well, there was no easy way to replicate Borg archives to the
cloud. There are other options, however.

But before that: there's a bigger problem. Is this Archive storage tier even economical? There's a [very
rigorous analysis](https://kmh.prasil.info/posts/rustic-cold-storage-glacier-economics/) I read which was
really good. By my own calculations, it would cost a whopping US$414  (AU$621 !!) to retrieve my 1.8 TB backup
in a disaster scenario:

- Operations cost: ~$0.005 (class B operation to queue download of ~1,000 backup files)
	- https://www.googlecloudcommunity.com/gc/Infrastructure-Compute-Storage/Cloud-Storage-Class-A-and-B-Operations/m-p/686099
- Retrieval cost: \$0.05 per GB x 1800 GB = $90
- Data transfer to Australia: \$0.18 x 1800 GB = $324
- Total cost to restore = $414

As a primary backup media, this would be really bad. The worst part is, the retrieval cost itself is only
US\$90 - it's the US$324 in bandwidth that's the killer. This would apply to any tier above Archive as well,
and the costs are similar with other large providers like AWS.

So, pulling the entire backup set isn't ideal from a cost POV. Luckily, folks have written some backup
programs with S3-compatible storage in mind. The options I'm going to be taking a look at are Kopia and
Rustic.

### Kopia

### Rustic
[Rustic](blah) is a Rust rewrite of [Restic](blah).

## Cloud storage providers
### rsync.net
[rsync.net](https://rsync.net) is a cloud storage provider primarily designed for backups. Their
infrastructure is basically FreeBSD + ZFS in a raidz3 configuration, using co-located servers in some pretty
nice datacentres (HE.net stands out as a good example, given they partially power the internet backbone). As
they say on [this page](https://www.rsync.net/cloudstorage.html), the combination of FreeBSD and ZFS in a good
RAID configuration gives me confidence that the data will remain intact. Also, rsync.net have been in the game
since 2001, so I have pretty decent confidence that they know what they're doing, and importantly, that
they'll still be around in ten years time.

In terms of pricing, no matter what way you put it, it's on the expensive side. I certainly don't want to come
across like an rsync.net shill: the pricing is a serious sticking point. It's not as bad as AWS/GCP hot
storage, but it's still pricier than alternative S3 providers like Backblaze B2. They have a [secret offer
page](https://www.rsync.net/products/borg.html) for Borg users that bills US$0.008/GB/month, so US$8/TB/month.
This is basically the same as their regular accounts, minus Borg-specific support and ZFS snapshots, which I
don't need. However, for reference, compare this to Backblaze B2 which is US$6/TB/month, and IDrive e2 which
is US$5/TB/month. To be fair, the Borg pricing is _much_ better than their standard pricing, but yes - it
still adds up.

However, by far the best thing about rsync.net is there is zero bandwidth fees, forever. As in, you can
download and upload as much as you want, 100% free of charge! As we found out above, and [as even Cloudflare
have noted](https://blog.cloudflare.com/aws-egregious-egress/), the HyPeRsCaLeRs have
exorbitant bandwidth fees. Even Backblaze B2 _do_ eventually charge for bandwidth when you push the bandwidth
high enough (over 3x your average monthly storage amount). I actually have no idea how they can manage this,
but props to them, it works.

### BorgBase
[BorgBase](https://www.borgbase.com/) is a service that hosts Borg backups. The pricing is a very reasonable
US$5/TB/month.

One of the things I really like about BorgBase is the fact that they support the Borg and Borgmatic projects
directly. This is something I feel like rsync.net should do. Their web UI is also really nice too, but it
wasn't enough of a reason to switch over when I automate all my backups and am perfectly comfortable using the
CLI.

There are some issues I have with BorgBase. They have only been in business since 2018, and seem to be staffed
by one person, so I was concerned about the long-term health of the service, and if they will still be running
in say 10 years time. The other issue I discovered was [a long period of historical
downtime](https://news.ycombinator.com/item?id=37115540), which is theoretically OK; but as the HN commenters
point out, the description of the issue seems worryingly like the administrators have trouble maintaining a
RAID configuration. This is concerning when BorgBase [specifically asks you not to run `borg
check`](https://docs.borgbase.com/faq/#how-often-should-i-run-borg-check) operations to check the integrity of
your backups, as they say it's protected by their RAID configuration.

### Backblaze B2/Wasabi/IDrive e2
These are all S3-compatible object stores. As we detailed above, they have very good pricing (especially when
compared with Amazon S3!) and are all hot storage, meaning they can be accessed instantly. The only thing
preventing their use with Borg is that Borg needs a server running SSH on the receiving end, which of course
these S3-compatible stores don't have. The only other option would be recording Borg backups to an external
HDD, and then syncing the archive files using rclone to this data store, but given my terrible track record
with disks, I decided against this.

Writing this up now, I think one of Kopia/Rustic and Backblaze B2/IDrive e2 would actually be a decently good
setup as well, and `rclone mount` would still allow me to use this as the backend for Nextcloud. With Borg,
the main advantage I think you get is the stability: neither Kopia nor Rustic have reached their 1.0.0 release
yet. Normally, this wouldn't bother me... but for a backup, I do feel it's
important.

In the future, if the costs get too high with rsync.net, I may move to this setup, with preference to
Backblaze B2 due to its long-term presence.

### The elephant in the room: Australia
Anyway, we haven't addressed the elephant in the room: all these storage providers have US/EU servers, and I'm
Australian. This is a real pain in the ass, because no matter how you spin it, speeds are going to be slow. As
I type this up, I am _still_ syncing my backup to rsync.net at measly 18 MiB/s, and it may take several weeks
to complete. We will just have to hope that Borg's deduplication makes the following backups faster!

## Setting up Borg
Alright, let's get into the details. After you pay rsync.net, it will take some time (up to 24 hours) for them
to manually provision your account - which I found a little strange in 2024, but not to worry. When you do so,
you'll get an email containing your login, which is something like `<dc><number>@<dc><number>.rsync.net`. For
me, `dc` is `fm` for "Fremont", as that's the datacentre I picked.

You will be assigned a temporary password, so the first thing I did was transfer my SSH key and disable
password authentication in the rsync.net Account Manager. Always a good idea, especially with the automation
we'll setup :)
