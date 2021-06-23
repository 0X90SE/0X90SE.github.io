---
date: 2016-12-01 14:07:00+02:00
title: Install Kali Linux on the USB Armory Mark I
header:
  image: /assets/images/posts/computer-cropped.jpg
teaser_image_path: /assets/images/posts/computer-cropped.jpg
toc: true
toc_label: "Content"
toc_sticky: true
tags:
- Security
- Kali Linux
- Hacking
- Electronics
---

A while ago, the Kali team release Kali Linux for the USB Armory, a small SoC computer by Inverse Path.

The instructions for installing Kali on the Armory are not very clear....OK, I'm using very diplomatic and friendly words now. To be frank, the instruction fails badly, not taking into account the type of device being used at all! This guide will hopefully help filling out some of the blanks.

This guide is focuses mostly on using Fedora Linux as the platform for configuration and communication with the USB Armory. Most of it will most certainly be possible to apply on other distributions as well with some tweaking.

Installing Kali on the SD Card

First make sure you are using a fast category 10 SD card at the very least. We want to eliminate as many bottlenecks as possible.

Download the Kali image for the USB Armory from:
https://www.offensive-security.com/kali-linux-arm-images/
The image is slightly over 700Mb in size. When done first make sure that you write the image to the right device:

~
gt; sudo fdisk -l Device         Boot    Start      End  Sectors  Size Id Type /dev/mmcblk0p1         10240 30896127 30885888 14.7G 83 Linux /dev/mmcblk0p2      30896128 62521343 31625216 15.1G  c W95 FAT32 (LBA)

Personally I use a 32Gb SD card. To write the image to the SD card, run the following command:

~
gt; sudo xzcat kali-$version-usbarmory.img.xz | sudo dd of=/dev/sdb bs=512k 0+764079 records in 0+764079 records out 7340032000 bytes (7.3 GB, 6.8 GiB) copied, 305.613 s, 24.0 MB/s

This will decompress and write the image to the SD card at once.

When done, make sure all buffers are flushed and all data is written to SD card using command:

~
gt; sync

This may take a while depending on the speed of SD card used.

Configuring Kali

First off! It is boring to stick with the default disk size, so we need to grow the partition a bit. This is easily done using for example GParted. The image below shows the disk layout directly after the image has been written to the SD card.

When done, we want it to look something like this. I chose to make a FAT32 partition in order to be able to use it in Windows too without any fuzz where I can land the files used in Kali such as loot, logs etc.

In the end, the resulting disk layout looks something like this:

Connecting to Kali

Most popular Linux distributions comes with a network manager that automatically configures and keeps track of your network interfaces. You will soon discover that this messes with your USB Armory as network manager wants to take control over it. However there are a couple of alternatives dealing with this.

First off, plug in your Armory and wait until it has booted, which is indicated by the white pulsing LED, which is the kernel heartbeat. This means that all is well.

Alternative 1 - Permanent Exclusion Using Config File

Run the following command in order to list the interfaces managed by the network manager:

~
gt; nmcli dev status DEVICE      TYPE      STATE        CONNECTION virbr0      bridge    connected    virbr0      wlo1        wifi      connected    circle      enp0s25     ethernet  unavailable  --          lo          loopback  unmanaged    --          virbr0-nic  tun       unmanaged    --

Edit the Network Manager configuration file:

~$> vim /etc/NetworkManager/NetworkManager.conf

Alternative 2 - Permanent Exclusion Using KEYFILE Plugin


In the Network Manager configuration file, add the following entries:

[main]
plugins=keyfile

[ifupdown]
unmanaged-devices=mac:<mac1>;mac:<mac2>

Alternative 3 - Temporarily Disable Network Manager


~
gt; service NetworkManager stop

Do your thing... Then start Network Manager again:

~
gt; service NetworkManager start




Enabling Network Sharing on Fedora

As you perhaps already know, Fedora is running a completely different firewall solution than for example Ubuntu where we have UFW - Uncomplicated Firewall, while Fedora is using firewalld a more complicated and versatile solution. Under the hood both are based on iptables though.

We then need to set up network sharing. First we need to bring up the network interface of the USB Armory.

~
gt; ip link set enp0s20u1 up ~
gt; ip addr add 10.0.0.2/24 dev enp0s20u1

In order to share the access with our main interface:

iptables -t nat -A POSTROUTING -s 10.0.0.1/32 -o <IF> -j MASQUERADE




echo 1 > /proc/sys/net/ipv4/ip_forward







Move physical NICs to external zone (which allows MASQUERADE by default)

move ethernet interface from public zone to external zone (in my case it is enp0s25 but could be eth0)
move wireless interface from public zone to external zone (in my case wl4s0, but could be wlan0)

Configure internal zone. Note that we are not adding the usb0 NIC to internal zone. This is in case other non-USB Armory USB ethernet adapters are used later. They should still go to the default zone (in my case, that is public)

While searching the net on the subject I found that there seem to be some confusion about what the source network is on Kali.

add source 10.42.0.0/24 to internal zone (for Kali image)
add source 10.0.0.0/24 to internal zone (for USB Armory official images)
Allow internal zone access to DNS service
add "dns" service to internal zone (on services tab)

If local/private network access required (e.g. access servers by hostname rather than IP)

edit /etc/resolv.conf

on USB Armory, edit /etc/resolv.conf
edit nameserver entry with the IP address of the local DNS resolver (should also be a forwarder to resolve internet hostnames). e.g. (assuming local DNS is on 192.168.0.1):
#nameserver 8.8.8.8
nameserver 192.168.0.1

SSH to Kali

Finally, we may SSH into our new Kali installation

Error Handling

The kernel heartbeat LED may pulse even if Kali has not booted properly. On one occasion this happened to me as a result from a typo in the fstab file which in turn prevented the SSH service from starting. No SSH, no login. So if a similar thing happens to you, go through all steps again and check for bad configurations, typos etc.