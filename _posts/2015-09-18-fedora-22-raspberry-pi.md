---
layout:     post
title:      Changing the partition type using sfdisk
date:       2015-09-18 11:21:29
summary:    
categories: raspberrypi
---

Parted is all very well, but I found that needing to know the exact position of the partition was a bit of a pain.

Whilst scanning my feeds, I saw a reference to sfdisk (scriptable fdisk). As all I need to do is change the partition to vfat, then make a new filesystem, I might be able to do the following:


{% highlight bash %}
losetup -f test.img 
sudo sfdisk --part-type /dev/sdb 1 0b
mkfs.vfat /dev/sdb1 -F32 
{% endhighlight %}

