---
title: "Install Media"
teaching: 0
exercises: 0
questions:
- "How do I write an SD card?"
objectives:
- "Installing Raspbian"
keypoints:
- "Learn command-line utilities to write SD cards."
---
Raspbian provides [good documentation for writing an SD card](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

In the BCRC, we'll need to use the MacOS directions, but you can write your SD using another computer if you prefer.

Go to [https://www.raspberrypi.org/downloads/raspbian/](download page) and download the "lite" version of Raspbian

Insert the SD card in the slot or connect the SD card reader with the SD card inside.
~~~
$ diskutil list
~~~
{: .language-bash}

Identify the device number of the SD card.

~~~
$ sudo dd bs=4m if=path_of_your_image.img of=/dev/rdiskN conv=sync
~~~
{: .language-bash}

Replace N with the number that you noted before. Note that you're refering to the "raw device"

This will take 5 or 10 minutes, depending on the image file size. Check the progress by pressing Ctrl+T.

If the command reports dd: bs: illegal numeric value, change the block size bs=1m to bs=1M.

If the command reports dd: /dev/rdiskN: Operation not permitted you need to disable SIP before continuing.

If the command reports the error dd: /dev/rdisk3: Permission denied, the partition table of the SD card is being protected against being overwritten by Mac OS. Erase the SD card's partition table using this command:

----
$ sudo diskutil partitionDisk /dev/diskN 1 MBR "Free Space" "%noformat%" 100%
----
{: .language-bash}

That command will also set the permissions on the device to allow writing. Now issue the dd command again.

After the dd command finishes, eject the card:

----
$ sudo diskutil eject /dev/rdiskN
----
{: .language-bash}

You should be able to remove the microSD card and use it to boot your Raspberry Pi.

{% include links.md %}
