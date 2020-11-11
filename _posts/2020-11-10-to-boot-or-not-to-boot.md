---
layout: post
title: "The surprising difficulty of creating a Windows bootable device on Linux"
date: 2020-11-10 10:00:00
categories: software
featured_image: /images/cover.jpg
---

This is kind of an unusual post, written out of frustration when trying to install Windows or to be more precise when trying to make a simple stupid SD card bootable.  
Most of what you find on that subject on the internet is either outdated, wrong, or some stale copypastas.
One would expect Microsoft, a company making a  profit out of you buying and using Windows, to  make it easy for you, an OS traitor, to defect.

Well not really...

👇👇👇👇👇👇👇👇👇👇👇   
👉`tl;dr` Use [WoeUSB](https://github.com/WoeUSB/WoeUSB).👈  
☝️☝️☝️☝️☝️☝️☝️☝️☝️☝️☝️

## Why ?
UEFI, Filesystems limitations, and bootloader shenanigans.  

The Windows 10 ISO ships with an installer file greater than 4GB so FAT32 is out of
the questions, so tools like unetbootin don't seem to work.  
exFat (only supported since Linux 5.4.0) and NTFS need some [special attention](https://github.com/pbatard/uefi-ntfs) to make a partition bootable.  

`WoeUSB` is the kind of awesome tool that format, partitions, copy to files and apply the above "fix" just by pointing to the device and the ISO. And it's also
cute[^0]:   

![Woe is cute](/images/2020-11-10-to-boot-or-not-to-boot/woe_cute.png)

I can claim no credit here: this incredible [answer](https://superuser.com/a/1527373) from a Rufus dev covers it all and probably
more than you want to know about the hows and the whys.

----

[^0]: [1]: Tools should be more friendly in general. Or at least have [some personality](https://www.valgrind.org/docs/manual/manual-core.html#manual-core.warnings)
