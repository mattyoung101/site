+++
title = "Using rsync.net for Borg backups"
date = "2024-11-12T04:41:14+10:00"

# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["infra"]
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
infrastructure is basically FreeBSD + ZFS in a raidz3 configuration, using colocated servers in some pretty
nice datacentres (HE.net stands out as a good example, given they partially power the internet backbone). As
they say on [this page](https://www.rsync.net/cloudstorage.html), the combination of FreeBSD and ZFS in a good
RAID configuration gives me confidence that the data will remain intact. Also, rsync.net have been in the game
since 2001, so I have pretty decent confidence that they know what they're doing, and importantly, that
they'll still be around in ten years time.

In terms of pricing, no matter what way you put it, it's on the expensive side. It's not as bad as AWS/GCP hot
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
TBA

In saying all this, one of the things I really do like about BorgBase is the fact that they support the Borg
and Borgmatic projects directly. This is something I feel like rsync.net should do. Their web UI is also
really nice too, but it wasn't enough of a reason to switch over when I automate all my backups and am
perfectly comfortable using the CLI.
