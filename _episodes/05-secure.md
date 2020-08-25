---
title: "Secure your Pi"
teaching: 20
exercises: 5
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

The Raspberry Pi Foundation provides a [guide for securing your computer](https://www.raspberrypi.org/documentation/configuration/security.md) which may have some additional information about how to approach some of the items below.

## Use secure password(s)

In the initial configuration, we set a secure password for the pi account. Make sure to use secure passwords in all circumstances.

## Use ssh keys

You can generate public/private key pairs that allow you to connect via ssh without typing a password. Run ssh-keygen on both sides — on the pi and on the computer you want to connect from.

~~~
ssh-keygen
~~~
{: .language-bash}

This will create a .ssh directory in your home directories. If you copy the contents of .ssh/id_rsa.pub from the computer you want to connect from and put it in the .ssh/authorized_keys file of your raspberry pi, you'll discover that went you connect, you'll be logged in without a password.

You might also want to set up a relationship between your pi and a server to automate backups.

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

## Install a firewall

A firewall only allows computers from defined addresses being able to connect. During development, your computer will probably move from place to place and have different addresses. When you set up your pi permanently, you should install a firewall to restrict who can connect.

## Create a separate account

Since the pi account has a well-known name, it's a good idea to create your own account and use that for your work.

To add a new user, enter:

~~~
sudo adduser [username]
~~~
{: .language-bash}

You will be prompted to create a password for the new user.

A home directory for the new user will be created at /home/[username]/.

To add them to the sudo group to give them sudo permissions as well as all of the other necessary permissions:

~~~
sudo usermod -a -G adm,dialout,cdrom,sudo,audio,video,plugdev,games,users,input,netdev,gpio,i2c,spi [username]
~~~
{: .language-bash}

You can check your permissions are in place (i.e. you can use sudo) by trying the following:

~~~
sudo su - [username]
~~~
{: .language-bash}

If it runs successfully, then you can be sure that the new account is in the sudo group.

Make sudo require a password

Placing sudo in front of a command runs it as a superuser, and by default, that does not need a password. In general, this is not a problem. However, if your Pi is exposed to the internet and somehow becomes exploited (perhaps via a webpage exploit for example), the attacker will be able to change things that require superuser credential, unless you have set sudo to require a password.

To force sudo to require a password, enter:

sudo nano /etc/sudoers.d/010_pi-nopasswd

and change the pi entry (or whichever usernames have superuser rights) to:

alice ALL=(ALL) PASSWD: ALL

Now save the file.

## modify sudoers

The sudo command allows you to take action as the superuser and do things that your permissions would not normally allow. During setup and development, its a convenience to not have to enter a password, but if your account becomes compromised, requiring the sudo command to require a password is a good practice. When you complete development, you can edit a file to to require a password to be entered for running a command with sudo.

~~~
sudo vi /etc/sudoers.d/010_pi-nopasswd
~~~
{: .language-bash}

## Use version control and backups

Frequent backups are a good idea during development. By doing your work in a git repository and pushing it frequently to a server, you also protect yourself against data loss. But maintaining a backup of your home directory is a good idea too.

~~~
/usr/bin/rsync -artuz /home/[username]/ [username]@[server]:/home/[username]/[pi name]
~~~
{: .language-bash}

## Manage permissions correctly

A detailed presentation of Unix permissions is beyond the scope of this document, but don't make file permissions more permissive than necessary. Unix files have three sets of permissions: for the user, the group, and the world (everything on the system). Beginners sometimes make everything world writable so they don't have to think about permissions. Instead, use sudo to modify configuration files without changing their permissions, and do your own work as yourself so system processes can't trivially modify your files.

## Document everything

Backups and version control are useful for the code you write, but won't capture the configuration changes you made to set up your project. The best solution is to document the changes you make. Professional sysadmins generally write all of their changes in a permanent notebook, so they can refer to changes they made. Short of that, you can use copy & paste to put commands into a text file. If your card becomes corrupted, good documentation can save yourself hours of effort trying to figure out how to set everything up again.

{% include links.md %}
