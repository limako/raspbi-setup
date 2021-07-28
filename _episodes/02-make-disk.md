---
title: "Install Media"
teaching: 10
exercises: 10
questions:
- "How do I write an SD card?"
objectives:
- "Installing Raspbian"
keypoints:
- "Learn command-line utilities to write SD cards."
---
> ## Important
> Before over-writing your SD card, confirm that you were not provided 
> with a pre-configured card as part of your course or training. These
> directions are provided for completeness, but they might not be necessary
> in your circumstances.
{: .callout}

Raspbian provides [good documentation for writing an SD card](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

Go to [Raspberry Pi OS Download Page](https://www.raspberrypi.org/downloads/raspbian/) and download the "lite" version of Raspbian.

> ## Why Lite?
> The Desktop version of Raspbian comes with a lot of software and services 
you won't need for most instrumentation purposes. These take up space, 
consume RAM, steal computing cycles, and create more attack surfaces for 
your computer to get compromised. Start with a minimal image and then only 
add the resources you really need for a project.
{: .callout}

The Raspberry Pi foundation recommends using [Balena-Etcher](https://www.balena.io/etcher/) to write SD cards, which makes it very simple and works the same across multiple platforms.

I've provided the Linux command-line directions below to offer commentary and demystify what happens behind the scenes.

In Linux, you can use "lsblk" to see what disks the system can currently see. The "-p" flag will show the full path to the device.

~~~
$ sudo lsblk -p
~~~
{: .language-bash}

Then insert the SD card in the slot or connect the SD card reader with the SD card inside, then run lsblk again. You should easily be able to readily identify the device and number of the SD card. It might be "/dev/mmcblk0" or "/dev/sda" depending on how you've connected the card.

If this is a new card, the computer may have automounted one or more partitions which show up underneath the entry for the device with a partition designation, e.g. "/dev/mmcblk0p1" or "/dev/sda1"

You'll need to unmount (spelled "umount") all of the mounted partitions before writing to the device:
~~~
$ sudo umount /dev/mmcblk0p1
$ sudo umount /dev/mmcblk0p2

~~~
{: .language-bash}

You may need to umount more than one partition on linux. It's safe to try to umount

> ## Caution!
> Before taking the next steps, double check your typing and be absolutely certain you are referring to the correct device. Writing to the wrong device as root can damage the filesystem, data, and/org operating system of your computer.
{: .callout}

~~~
$ sudo dd bs=4m if=[path_of_your_image].img of=/dev/mmcblk0
~~~
{: .language-bash}

Before you eject the card, it's a good idea to run "sync" which makes sure all disk operations are completed.

~~~
$ sudo sync
~~~
{: .language-bash}

If you remove the card and put the card back into your computer, it should mount one or both partitions and you can make some configuration changes before booting your Pi for the first time.

Once you write the Raspbian image to your SD card will have two partitions: boot and rootfs.  The "boot" partition contains the linux kernel and firmware used to boot the computer plus two configuration files: config.txt and cmdline.txt. This partition is formatted using FAT32 and can be written from Macs, Windows, or Linux computers. (The "rootfs" partition is formatted using EXT4 and is generally only writable with linux).

You should be able to remove the microSD card and use it to boot your Raspberry Pi.

{% include links.md %}
