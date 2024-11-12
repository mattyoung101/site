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

<!-- mtoc-end -->

## Background
Recently, I decided I would like to migrate my backups from an external HDD to ✨ *the cloud* ✨. This idea
sort of came up while I was trialling Google Cloud, which I was doing in order to run a BOOM RISC-V tape-out
using [OpenROAD](https://openroad.readthedocs.io/) that needed more RAM than my 64GB workstation could
provide. Google Cloud Storage has an interesting Archive tier option, which I was planning to use to sync my
[Borg](https://borgbackup.readthedocs.io/en/stable/) backups. Unfortunately, I noticed that my WD external USB
HDD had died (!) after only a few years of non-intensive use (!), which meant this was no longer a viable
option.

![Ruh roh](/blog_nextcloud/disk_failure.png)

*Ruh roh. Not btrfs this time, but actual physical disk corruption (notice the critical medium error)*

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
