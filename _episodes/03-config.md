---
title: "Configure your Pi"
teaching: 20
exercises: 0
questions:
- "Why are things spelled funny?"
- "Why does my keyboard not work right?"
objectives:
- "Getting ready to do work."
keypoints:
- "Be productive quickly."
---

When you start your Raspberry Pi for the first time, there are some things we should configure before you start working.  There is a handy-dandy script called raspi-config that can configure a lot of basic stuff. In addition, in the guide below, I'll share some of the actual commands behind the scenes that are being run. But rasp-config is a shell script (written in bash) and so you can actually look at the script to see how it works.

You'll need to start raspi-config with sudo because its needs root privileges to change most configuration options.

~~~
sudo raspi-config
~~~
{: .language-bash}

The script uses a system called "whiptail" that provides a sort of graphical user environment in a terminal window. You can use arrow keys to make a selection in a list, the "tab" to move to different buttons, "space" to pick or toggle a selection, and "enter" to select the option currently indicated.

## Set a Secure Password

Initially, the pi comes with a single user account ("pi") and the password is set to "raspberry". You should reset this password before you connect to the network. Even a few minutes on the network with the default password set could result in your computer being compromised.

There are many guidelines for choosing a secure "memorized secret". The National Institutes of Standards and Technologies (NIST) has an appendix (A) at the end of their [Digital Identity Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html) that provides some guidance for choosing a good password.

## Localisation options

By default, your Raspberry Pi starts up configured for use in England. We should set some things things up so it will work correctly in your environment.

### Change Locale

Select ""

Edit /etc/locale.gen and uncomment the line with en_US.UTF-8

Alternatively, you can run this single command without editing anything manually (Thanks to Andre for this.)

~~~
perl -pi -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
~~~
{: .language-bash}

Run locale-gen en_US.UTF-8
Run update-locale en_US.UTF-8

## Set the timezone



## Set the hostname

edit /etc/hostname
edit /etc/hosts



{% include links.md %}
