+++
title = "Setting up storage.horse: FreeBSD, Nextcloud, rclone and rsync.net"
date = "2024-11-13T03:16:12+10:00"
tags = ["infra", "storage"]
+++


<!-- mtoc-start -->

* [Introduction](#introduction)

<!-- mtoc-end -->

## Introduction
In my [previous post](/blog/rsyncnet_backups), I explained how I moved my backups off site and online using
Borg and rsync.net. Continuing on with my temporary obsession with data storage, I had the great realisation
that by using the absolutely amazing [rclone](https://rclone.org/) tool, we can actually use it as a
[Nextcloud](https://nextcloud.com/) backend as well! That is, rather than pay for expensive block storage on
Google Cloud, we can simply store it all on Nextcloud, transparently, for a much better price.

The end result of this is [storage.horse](https://storage.horse): a Nextcloud instance for my friends and
family. It's also documented on the [projects page](/projects).

So, strap in! This is a long and surprisingly complicated ride, although it _did_ eventually end up working,
so that's nice.
