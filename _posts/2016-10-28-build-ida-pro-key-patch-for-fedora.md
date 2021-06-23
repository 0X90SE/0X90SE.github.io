---
date: 2016-10-28 14:07:00+02:00
title: Build IDA Pro Key Patch for Fedora
header:
  image: /assets/images/posts/computer-cropped.jpg
teaser_image_path: /assets/images/posts/computer-cropped.jpg
toc: true
toc_label: "Content"
toc_sticky: true
tags:
- Security
- Reverse Engineering
- Tools
---

KeyPatch by Keystone is a rather powerful tool when it comes to reverse engineering and patching binaries in particular. It lets you write the mnemonics/assembly directly instead of the opcodes. I know it makes my life way easier when patching!

It transparently supports many different architectures such as:

* X86 (16/32/64bit)
* ARM (32/64bit)
* MIPS
* SPARC
* PowerPC

As I for many reasons switched from a debian based package system to Fedora, an RPM based package system a while ago I also left the mainstream community. Many guides are .deb focused which in most cases is not very compatible with .rpm based distros when it comes to package names, quirks, solutions etc.

This small guide focuses on building the Keystone library used by KeyPatch from source on Fedora gathering some of the scattered information I found on the subject.

## Prerequisites

First make sure that you have all essential development tools such as gcc, gcc-c++, make, cmake, git etc. Some of which may for example be installed using the command:

~ gt; sudo dnf install @development-tools gcc-c++ cmake git

Also, as IDA Pro is using 32bit executables and libraries we need to build our library for 32bit. We thus need a few 32bit libraries installed as most modern distros are 64bit nowadays:

`sudo dnf install libstdc++.i686 glibc.i686 glibc-devel.i686`

## Building the Keystone Library

First we grab a fresh copy of the Keystone source and start building the shared library used by the Python binding:

```
git clone https://github.com/keystone-engine/keystone.git ~
cd keystone keystone
mkdir build keystone
cd build build
../make-share.sh lib32 lib_only
```

If all went well, the library is ready to be installed. You can also check the results using command file in order to see the it became a 32bit shared library:

```
build
file llvm/lib/libkeystone.so libkeystone.so: ELF 32-bit LSB shared object, Intel 80386, version 1 (GNU/Linux), dynamically linked, BuildID[sha1]=945c9d57d608e43daf532c247d6a3628ae5d90e4, not stripped
```

Now leave the build directory:

```
build
gt; cd ..
```

## Installing the Keystone Library

Copy the shared library to your IDA Pro installation directory. As for me, I have it in my home directory for different reasons. As such, you will have to modify the below paths according to your own IDA Pro installation:

```
keystone
cp -r bindings/python/keystone /home/user/system/ida69/python/ keystone
cp build/llvm/lib/libkeystone.so* /home/user/system/ida69/python/keystone/
```

The first command copies all necessary Python bindings while the other copies the actual library.


## Installing the KeyPatch Plugin

Now that the library is in place, we need the actual plugin. Get a fresh copy of the KeyPatch plugin from github:

```
git clone https://github.com/keystone-engine/keypatch.git
```

Finally, copy the python script to your IDA Pro plugins directory:


```
cp keypatch/keypatch.py /home/user/system/ida69/plugins
```

## Give it a Try

Now try starting IDA Pro. The following log is expected:

KeyPatch can then be invoked using hotkeys CTRL+ALT+K but probably not if your are using KDE/Plasma as this hot key combination is predefined for changing the keyboard layout. You can also invoke keypatch by right-clicking the instruction you want to edit/patch and chose menu option "keypatch".

Don't forget to write your changes to the binary when done ;)

To get a quick tutorial, please check the official tutorial:

http://www.keystone-engine.org/keypatch/tutorial/