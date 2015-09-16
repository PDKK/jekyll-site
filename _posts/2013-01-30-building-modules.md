---
layout:     post
title:      Building Raspberry pi modules without rebuilding the kernel
date:       2014-06-08 11:21:29
summary:    
categories: raspberrypi
---

Every now and then I get back to my Raspberry Pi, and realise that I have completely rebuilt my image. That makes it pretty hard to find my module building instructions, so here they are:

{% highlight bash %}
wget https://github.com/raspberrypi/linux/archive/rpi-3.6.y.tar.gz
tar xvfz rpi-3.6.y.tar.gz
su
mv linux-rpi-3.6.y /usr/src
ln -s /usr/src/linux-rpi-3.6.y /lib/modules/3.6.11+/build
cd /lib/modules/3.6.11+/build
make mrproper
gzip -dc /proc/config.gz > .config
make modules_prepare
wget https://github.com/raspberrypi/firmware/raw/master/extra/Module.symvers
exit
{% endhighlight %}


Then my makefile for the actual module is

{% highlight makefile %}


obj-m := kcrDriver.o

all:

$(MAKE) -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:

$(MAKE) -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
{% endhighlight %}

What is particularly useful about this is that I don’t need to compile the entire kernel just to build my one module.