---
layout:     post
title:      Building a Fedora 22 Raspberry Pi image
date:       2015-09-17 11:21:29
summary:    
categories: raspberrypi
---

Some old instructions for doing this are available at http://jonarcher.info/2015/02/getting-fedora-21-raspberry-pi-2/. I wanted to run these against 
an F22 image, getting ready for F23 coming out this month.

The idea is to take the F22 image, replace the boot partition with the Raspberry Pi version, update the modules and fstab, and ship it. In order to 
make it a bit more efficient, I decided to construct the image an the host machine, rather than re-wrtiting the SD card lots of times.

So first, we get a Fedora image, and uncompress it:

xz -d Fedora-xxxx.xz

Then loopback mount the image:

{% highlight bash %}
losetup -f test.img 
parted /dev/loop0 print
parted /dev/loop0 rm 1
parted /dev/loop0 mkpart primary fat32 1049k 513M
parted /dev/loop0 print
mkfs.vfat /dev/loop0p1 -F32 
losetup -d /dev/loop0
{% endhighlight %}

