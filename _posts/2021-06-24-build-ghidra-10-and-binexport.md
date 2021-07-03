---
date: 2021-06-24 16:09:00+02:00
title: Build Ghidra (10) from source
excerpt:
author: Circle
header:
  image: /assets/images/posts/ghidra2.png
teaser_image_path: /assets/images/posts/ghidra.png
toc: true
toc_label: "Content"
toc_sticky: true
tags:
- Reverse Engineering
- Ghidra
- BinDiff
- Tools
---

Ghidra 10 has now been released. I've been reloading the Ghidra Github almost every day now in order to get my hands on the release. However, since a few weeks I've been playing around with the source and the Beta 10, building Ghidra from scratch gaining insight into the build process and thought I'd share them with you guys as I know there are a lot of beginners out there that really want to level-up their skills.


## Prerequisites

Do note that this guide is Fedora centric. However these packages are usually available in Debian/Ubuntu using the same or similar package names. Use `apt search` to find the packages needed. Prepping and building Ghidra for Eclipse development is also out of scope of this guide.

I also know that you may have some, perhaps most or even all packages below installed. Even if so, they are needed in the various steps in this guide.

```
sudo dnf install java-11-openjdk git wget unzip 
```


### Setting active Java version

Ghidra does not agree entirely with the latest Java release, so we need to ensure that we use Java 11.

```
java -version
openjdk version "11.0.11" 2021-04-20
OpenJDK Runtime Environment 18.9 (build 11.0.11+9)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.11+9, mixed mode, sharing)
```

Should your version differ, switch it by using command:

```
sudo alternatives --config java
```

Output similar to the below is expected:

```
[sudo] password for user: 

There are 2 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
*+ 1           java-11-openjdk.x86_64 (/usr/lib/jvm/java-11-openjdk-11.0.11.0.9-4.fc34.x86_64/bin/java)
   2           java-1.8.0-openjdk.x86_64 (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.292.b10-4.fc34.x86_64/jre/bin/java)

Enter to keep the current selection[+], or type selection number:
```

Make your choice `java-11-openjdk.x86_64` and re-run `java -version` to verify your settings. This also goes for `javac`. Should it differ, repeat the above steps but for command `javac` and verify:

```
sudo alternatives --config javac
[sudo] password for user: 

There are 2 programs which provide 'javac'.

  Selection    Command
-----------------------------------------------
*+ 1           java-11-openjdk.x86_64 (/usr/lib/jvm/java-11-openjdk-11.0.11.0.9-4.fc34.x86_64/bin/javac)
   2           java-1.8.0-openjdk.x86_64 (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.292.b10-4.fc34.x86_64/bin/javac)

Enter to keep the current selection[+], or type selection number:
```


### Install Gradle

Since the Early 10.0 code base, Ghidra has been supporting Gradle v7.x, which we will focus on using in this case.

```
wget https://services.gradle.org/distributions/gradle-7.1-bin.zip
unzip gradle-7.1-bin.zip
```

copy the whole directory to a suitable path and add Gradle to your path:

```
sudo cp -R gradle-7.1 /usr/local/gradle
sudo echo "export PATH=/usr/local/gradle/bin:$PATH" > /etc/profile.d/gradle.sh
```

This time only, run (By next reboot/login, Gradle will be added to your path:

```
source /etc/profile.d/gradle.sh
```

Now try running Gradle by getting its version. Something similar to the below output is expected.

```
gradle -version

------------------------------------------------------------
Gradle 7.1
------------------------------------------------------------

Build time:   2021-06-14 14:47:26 UTC
Revision:     989ccc9952b140ee6ab88870e8a12f1b2998369e

Kotlin:       1.4.31
Groovy:       3.0.7
Ant:          Apache Ant(TM) version 1.10.9 compiled on September 27 2020
JVM:          11.0.11 (Red Hat, Inc. 11.0.11+9)
OS:           Linux 5.12.12-300.fc34.x86_64 amd64

```


## Build Ghidra

Building Ghidra is very straight forward and I have not encountered any snags along the way this far, having built a number of releases.

First, get Ghidra from Github:

```
git clone https://github.com/NationalSecurityAgency/ghidra.git
cd ghidra
```

From here you can list all releases and checkout the one you want:

```
git tag

Ghidra_10.0_build
Ghidra_9.0.1_build
Ghidra_9.0.2_build
Ghidra_9.0.3_build
Ghidra_9.0.4_build
Ghidra_9.1.1_build
Ghidra_9.1.2_build
Ghidra_9.1_build
Ghidra_9.2.1_build
Ghidra_9.2.2_build
Ghidra_9.2.3_build
Ghidra_9.2.4_build
Ghidra_9.2_build
```

Lets checkout the latest one `Ghidra_10.0_build` as we want the Ghidra Debugger and build it.

```
git checkout Ghidra_10.0_build
Note: switching to 'Ghidra_10.0_build'.
...
HEAD is now at 51254872b Updated ChangeHistory and WhatsNew for 10.0
```

Now lets prepare Ghidra for building. The gradle script `fetchDependencies.gradle` does all the work and satisfies all other dependencies for us.

```
gradle -I gradle/support/fetchDependencies.gradle init
```
Output similar to the below is expected:
```
Starting a Gradle Daemon (subsequent builds will be faster)
Processing dex-tools-2.0.zip
Copying AXMLPrinter2.jar to /home/someuser/Downloads/git-repos/ghidra/dependencies/flatRepo
Copying yajsw-stable-12.12.zip to /home/someuser/Downloads/git-repos/ghidra/dependencies/GhidraServer
Copying PyDev 6.3.1.zip to /home/someuser/Downloads/git-repos/ghidra/dependencies/GhidraDev
Copying cdt-8.6.0.zip to /home/someuser/Downloads/git-repos/ghidra/dependencies/GhidraDev
Copying vs2012_x64.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vs2012_x86.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vs2015_x64.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vs2015_x86.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vs2017_x64.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vs2017_x86.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vs2019_x64.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vs2019_x86.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vsOlder_x64.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb
Copying vsOlder_x86.fidb to /home/someuser/Downloads/git-repos/ghidra/dependencies/fidb

> Task :init SKIPPED
The build file 'build.gradle' already exists. Skipping build initialization.

Deprecated Gradle features were used in this build, making it incompatible with Gradle 8.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

See https://docs.gradle.org/7.1/userguide/command_line_interface.html#sec:command_line_warnings

BUILD SUCCESSFUL in 51s
```


Now we can build Ghidra
```
gradle buildGhidra
```

Most of the above and more can be found in [Ghidra Developers Guide](https://github.com/NationalSecurityAgency/ghidra/blob/master/DevGuide.md).



## Concluding Remarks

As seen, its quite easy and fast to build Ghidra which is nice when you want to try that new announced feature not yet released but present in the code base. No need to scrap your old version of Ghidra either since they ar self contained and can co-exist on your Linux system.

**Important:** Ghidra version 10 is backward compatible, but not the other way around. Old databases will be converted when opened in Ghidra version 10. Make sure to make a backup of your old projects if you need to be able to open them in older versions of Ghidra.

