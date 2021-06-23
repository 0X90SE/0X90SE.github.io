---
date: 2016-10-30 14:07:00+02:00
title: Debugging MIPS Programs in IDA Pro
header:
  image: /assets/images/posts/computer-cropped.jpg
teaser_image_path: /assets/images/posts/computer-cropped.jpg
toc: true
toc_label: "Content"
toc_sticky: true
tags:
- Security
- Reverse Engineering
- MIPS
- QEMU
---

Many embedded devices use the MIPS CPU architecture in their implementations to run for example Linux, which is one of the most popular implementations. MIPS - Microprocessor without Interlocked Pipeline Stages is a cheap and effective RISC architecture that comes in 32bit and 64bit versions. It is also bi-endian.

As such, the need to be able to debug applications for this architecture quickly arises, not only from a design point of view but also from a security point of view.

One method is to use for example JTAG but that requires a functional JTAG port and an interface.

Prerequisites

There are a few tools that need to be in place before we begin.

Preparing the QEMU virtual machine

Start the QEMU VM using the following command:

The "-user" argument sets up a user space network connection to the VM. The

Try SSH into your machine

ssh root@localhost

Perhaps one day I will write a guide on how to build the distro from scratch. But for now, we will settle using this method

Starting Your Sample

scp <FILE> user@localhost;/target/path/in/guest

Hooking up IDA Pro to the QEMU Guest

Last but not least, start IDA Pro and load the program you are debugging in the QEMU guest.