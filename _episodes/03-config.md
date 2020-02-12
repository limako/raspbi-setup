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

By default, your Raspberry Pi starts up configured to use in England. We should set some things things up


set the hostnamename

edit /etc/hostname
edit /etc/hosts

set the locale

    Edit /etc/locale.gen and uncomment the line with en_US.UTF-8

Alternatively, you can run this single command without editing anything manually (Thanks to Andre for this.)

perl -pi -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen

    Run locale-gen en_US.UTF-8
    Run update-locale en_US.UTF-8


{% include links.md %}
