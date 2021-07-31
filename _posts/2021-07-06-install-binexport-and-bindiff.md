---
date: 2021-07-31 13:00:00:00+02:00
last_modified_at: 2021-07-31 13:00:00:00+02:00
title: Install BinExport and BinDiff
excerpt:
author: Circle
author_profile: false
header:
  image: /assets/images/posts/2021-07-06-install-binexport-and-bindiff/bindiff_logo.png
teaser_image_path: /assets/images/posts/2021-07-06-install-binexport-and-bindiff/bindiff_logo.png
gallery_binexport_install:
  - url: /assets/images/posts/2021-07-06-install-binexport-and-bindiff/install-binexport-browse-extension.png
    image_path: /assets/images/posts/2021-07-06-install-binexport-and-bindiff/install-binexport-browse-extension.png
    alt: "Install Ghidra Extension"
    title: "Install Ghidra Extension"
  - url: /assets/images/posts/2021-07-06-install-binexport-and-bindiff/install-binexport-marked-extension.png
    image_path: /assets/images/posts/2021-07-06-install-binexport-and-bindiff/install-binexport-marked-extension.png
    alt: "Install Ghidra Extension"
    title: "Install Ghidra Extension"
toc: true
toc_label: "Content"
toc_sticky: true
categories:
- Reverse Engineering
tags:
- Ghidra
- BinDiff
- Tools
---

BinDiff v7.1 has quite resently been released! So I thought I'd write an updated guide on how to install BinDiff on Fedora Linux. Since my last post on how to install BinDiff on Fedora I've switched from IDA Pro to Ghidra. IDA Pro is very good, but not $6000+ good (Average price for IDA Pro and a couple of decompilers) compared to a free and a very competent Ghidra...but thats my personal opinion and a completely different discussion. Thus this guide will also include steps on how to install [BinExport](https://github.com/google/binexport) needed in order to use BinDiff with Ghidra on Fedora Linux.


## Prerequisites

As usual this guide is quite Fedora Linux centric, however it may easily be adapted to other RPM based Linux distributions. This guide also assumes that you've got a working instance of Gradle working on your system. It also assumes that you've got Ghidra 10.x installed and working. If this is not the case, check my guide [Build Ghidra from Source](https://www.0x90.se/build-ghidra-10-and-binexport/) in order to install Gradle and Ghidra.

Perhaps these needed packages are already present on your system, but still, we need to make sure they are present:

```
sudo dnf install alien rpmrebuild
```


## Build the BinExport Extension

In order to be able to use BinDiff with Ghidra, we need to implement Google's extension [BinExport](https://github.com/google/binexport) into Ghidra. This extension exports a Ghidra project into a format that BinDiff understands.

Download BinExport:

```
git clone https://github.com/google/binexport.git
cd binexport/java
```

**NOTE:** The Gradle build script expects that the `GHIDRA_INSTALL_DIR` environment variable is set with the absolute path to your Ghidra installation or given as an command line argument to gradle.
{: .notice--warning}

Either use the command line parameter (Adjust the path according to your Ghidra installation):

```
gradle -PGHIDRA_INSTALL_DIR=/home/someuser/bin/ghidra
```

Or export the path as an environment variable (Adjust the path according to your Ghidra installation):

```
export GHIDRA_INSTALL_DIR=/home/someuser/bin/ghidra
gradle
```

Output similar to the below is expected:

```
gradle -PGHIDRA_INSTALL_DIR=/home/somuser/bin/ghidra

Deprecated Gradle features were used in this build, making it incompatible with Gradle 8.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

See https://docs.gradle.org/7.1/userguide/command_line_interface.html#sec:command_line_warnings

BUILD SUCCESSFUL in 15s
8 actionable tasks: 8 executed
```

The Ghidra extension is now available in `../binexport/java/dist`.


## Install the BinExport Extension

Start Ghidra and install the new extension by clicking "File" and then "Install extensions..."

{% include gallery id="gallery_binexport_install" caption="*Installing the BinExport Ghidra Extension.*" %}

Information about the extension may be seen by marking the installed extension. Click "OK" to close this dialog and the restart Ghidra in order to make the extension active.


## Install BinDiff

I wrote [this guide](https://www.0x90.se/install-bindiff-on-fedora/) a few years ago on how to install BinDiff 4.2 on Fedora Linux, but when my old server stopped working the guide was lost and with time equally inaccurate. I found out that it was lost when doing a search in bindiff and Fedora. Thomas at [https://0xc0decafe.com/install-bindiff-on-fedora/](https://0xc0decafe.com/install-bindiff-on-fedora/) had tried to read my post and failed. Instead it resulted in a nice and detailed post of his own.

Download BinDiff 7.0:

```
wget https://storage.googleapis.com/bindiff-releases/updated-20210607/bindiff_7_amd64.deb
```

We then need to convert it from `.deb` to `.rpm` in order to install it on our Fedora system. Also in order to ensure and maintain original ownerships within the package, we run alien as root.

```
sudo alien -vk --to-rpm bindiff_7_amd64.deb
```

When trying to install the package with (Command assumes that you are in the same directory as the package):

```
sudo dnf install ./bindiff-7-1.x86_64.rpm
```

It will result in the following error:

```
Error: Transaction test error:
  file /usr/bin from install of bindiff-7-1.x86_64 conflicts with file from package filesystem-3.14-5.fc34.x86_64
  file /usr/lib from install of bindiff-7-1.x86_64 conflicts with file from package filesystem-3.14-5.fc34.x86_64
```

We need to fix the manifest and remove the offending lines in it by rebuilding the package

```
rpmrebuild -pe bindiff-6-1.x86_64.rpm
```


The following rows (116, 243 and 246 in my case) need to be removed:
```
...
%dir %attr(0755, root, root) "/"
...
%dir %attr(0755, root, root) "/usr/bin"
...
%dir %attr(0755, root, root) "/usr/lib"
```

**NOTE:** Since BinDiff version 7.1 it seems that the row `%dir %attr(0755, root, root) "/usr/lib"` also needs to be removed since it also generates an error.
{: .notice--warning}

The resulting file will be exported to:

```
result: /home/someuser/rpmbuild/RPMS/x86_64/bindiff-7-1.x86_64.rpm
```

### Configuring BinDiff

Unfortunately we are not done yet. Trying to run BinDiff you will most certainly get an error similar to this:

```
bindiff -ui
BinDiff 7 (@377901646, Jun  7 2021), (c)2004-2011 zynamics GmbH, (c)2011-2021 Google LLC.
Error: Missing jar file in prefix:
```

Doing an `strace bindiff -ui` gives us the following:

```
...
stat("/etc/opt/bindiff", {st_mode=S_IFDIR|0755, st_size=62, ...}) = 0
openat(AT_FDCWD, "/etc/opt/bindiff/bindiff.json", O_RDONLY) = 3
lseek(3, 0, SEEK_END)                   = 9823
lseek(3, 0, SEEK_CUR)                   = 9823
lseek(3, 0, SEEK_SET)                   = 0
read(3, "{\n \"version\": 7,\n \"directory\": \""..., 9823) = 9823
close(3)                                = 0
mkdir("/home", 0775)                    = -1 EEXIST (File exists)
mkdir("/home/user", 0775)               = -1 EEXIST (File exists)
mkdir("/home/user/.bindiff", 0775)      = -1 EEXIST (File exists)
openat(AT_FDCWD, "/home/someuser/.bindiff/bindiff.json", O_RDONLY) = -1 ENOENT (No such file or directory)
sysinfo({uptime=79447, loads=[145952, 156448, 140384], totalram=67128475648, freeram=16485195776, sharedram=2208235520, bufferram=32260096, totalswap=8589930496, freeswap=8589930496, procs=1405, totalhigh=0, freehigh=0, mem_unit=1}) = 0
stat("bin/bindiff.jar", 0x7ffeb50a21b0) = -1 ENOENT (No such file or directory)
stat("bindiff.jar", 0x7ffeb50a21b0)     = -1 ENOENT (No such file or directory)
write(2, "Error: Missing jar file in prefi"..., 35Error: Missing jar file in prefix: ) = 35
write(2, "\n", 1
)                       = 1
exit_group(1)                           = ?
+++ exited with 1 +++
```

Adding the path to the configuration file only generates another error like the one below:

```
bindiff -ui     
BinDiff 7 (@377901646, Jun  7 2021), (c)2004-2011 zynamics GmbH, (c)2011-2021 Google LLC.
Error: Error executing 'bindiff.jar': Permission denied
```

This simply means that BinDiff has not been correctly configured yet. This is due to the fact that some information in the post configuration script within the `.deb` package has been lost in translation to the `.rpm` package, which does not run on post install. Therefore add `/opt/bindiff` to the `directory` parameter as well as adding the correct jre path `/opt/bindiff/jre/bin/java` to the `java_binary`parameter (You may not be able to run BinDiff correctly using your system java version) in the beginning of the configuration file `/etc/opt/bindiff/bindiff.json`:

```
{
 "version": 7,
 "directory": "/opt/bindiff",
 "ui": {
  "java_binary": "/opt/bindiff/jre/bin/java",
  "java_vm_option": [],
  "max_heap_size_mb": 0,
  "server": "127.0.0.1",
  "port": 2000,
  "retries": 20
 },
```

Now try running BinDiff by issuing the following on the command line

```
bindiff -ui
```

## Testing the Whole Implementation

in order to test the installation, take any Ghidra project of yours where you have two versions of a binary (Not important for export tests only really).


## Concluding Remarks

As you can see, the procedure, albeit a bit more lengthy, it is still quite simple.

**NOTE*** In order to get good BinDiff results you need to do a good and for each binary to be compared, an equally executed analysis. The post "[Patch Diffing with Ghidra](https://ihack4falafel.github.io/Patch-Diffing-with-Ghidra/)" by Hashim gives you quite a good overview of the process of diffing two binaries.
{: .notice--info}


## References

Some references and recommended links:

[www.zynamics.com/bindiff.html](https://www.zynamics.com/bindiff.html)

[https://www.zynamics.com/bindiff/manual/index.html](https://www.zynamics.com/bindiff/manual/index.html)

[https://github.com/google/binexport](https://github.com/google/binexport)

[https://ihack4falafel.github.io/Patch-Diffing-with-Ghidra/](https://ihack4falafel.github.io/Patch-Diffing-with-Ghidra/)

[0x0decafe.com](https://0xc0decafe.com/)
