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

Select "Change Locale", scroll to "en_US.UTF-8", hit the space bar to select it, use tab to move to <Ok> and hit enter.

You may notice that en_GB.UTF8 is already selected. You should leave it selected.

The next screen asks you to set the "default locale". Choose "en_US.UTF-8", use tab to move to <Ok> and hit enter.

You can perform the same actions by editing /etc/locale.gen, uncommenting the line with en_US.UTF-8, and running these two commands

~~~
$ sudo locale-gen en_US.UTF-8
$ sudo update-locale en_US.UTF-8
~~~
{: .language-bash}


### Set the timezone

In raspi-config, select "Localisation options" then "Change Timezone". For UMass, select "US" then "Eastern."

Or you can edit /etc/timezone and replace the contents with "US/Eastern".

## Set the hostname

Use raspi-config or replaced "raspberry pi" in both /etc/hostname and /etc/hosts.

{% include links.md %}
