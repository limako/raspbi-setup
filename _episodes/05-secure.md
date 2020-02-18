---
title: "Secure your Pi"
teaching: 0
exercises: 0
questions:
- "How can you secure your Pi?"
objectives:
- "Keeping your Raspberry Pi safe from compromise."
keypoints:
- "Limit access and keep up-to-date."
---
Your Raspberry Pi is a computer on the internet. There are people constantly scanning computers on the internet looking for vulnerable devices to compromise. If your computer gets compromised, it could make all of your data suspect but, more importantly, it could be used to spy on you and to attack other computers. This is especially important for a computer that's likely to become a piece of infrastructure: that runs quietly in the background without being paid close attention on a day-to-day basis.

At the same time, there is a tension between usability and security: Gene Spafford, a famous computer security expert, once said, "only truly secure system is one that is powered off, cast in a block of concrete and sealed in a lead-lined room with armed guards - and even then I have my doubts." A really secure computer is unusable.

While developing a prototype instrument, it's reasonable to simplify some of the security setup. But it's worth being aware of what steps you should take to harden your pi before leaving it running as infrastructure.

## Use secure password(s)

In the initial configuration, we set a secure password for the pi account. Make sure to use secure passwords in all circumstances.

## Use ssh keys

You can generate public/private key pairs that allow you to connect via ssh without typing a password. Run ssh-keygen on both sides — on the pi and on the computer you want to connect from.

~~~
ssh-keygen
~~~
{: .language-bash}

This will create a .ssh directory in your home directories. If you copy the contents of .ssh/id_rsa.pub from the computer you want to connect from and put it in the .ssh/authorized_keys file of your raspberry pi, you'll discover that went you connect, you'll be logged in without a password.

## Only install what you need

Every software package you install potentially includes vulnerabilities that could be exploited. This is why we begin with the "lite" distribution of raspbian. Pay attention to what you install, only install what you need, and remove additional packages you're not actively using.

## keep up to date

It's important to keep the software up-to-date. Periodically run these two commands

~~~
sudo apt-get update
sudo apt-get upgrade
~~~
{: .language-bash}

The first command receives the list of updated packages available and the second, checks to see which installed packages have been updated, and invites you to update them.

Note that updates have the potential to modify or break existing services and code. Make sure to install updates at a time when you're not actively collecting data — and when you have time available to pursue problems if an occur. It's a bad idea to install updates on a Friday afternoon. 

## install a firewall

## create a separate account

## modify sudoers

## use backups

## manage permissions correctly

## document everything

{% include links.md %}
