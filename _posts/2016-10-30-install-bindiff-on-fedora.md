---
2016-10-30
---

In March 2011 Google acquired the company Zynamics, the developer of the tools BinDiff and BinNavi. These two tools were thus assimilated by Google, perhaps never to be seen again. However almost on the day 5 years later, BinDiff was release for free to the public! Soon to be followed by BinNavi which was release both for free and open source. Great news!

BinDiff (v4.20 when writing this post) is released both for Windows and Linux. The packages are released as .deb packages both for 32bit and 64bit systems, but no .rpm. The support pages said that one is to contact zynamics-support@google.com when in need for other packages. They've now changed the page(!) telling you that the may not answer and only decide on case by case whether to answer or not. I never got an answer.  But hey! Its no problem...

What is BinDiff?

It is a tool that interprets the saved database produced by IDA Pro after loading a binary into IDA. BinDiff takes two of these databases and compares them, visualizing the difference in disassembly style very much like IDA Pro's Graph View.

Its an excellent tool for analyzing binaries that may have been silently patched for vulnerabilities. Using BinDiff you will quickly see that potential vulnerability.

Prerequisites

You will need to have the tools "alien" and "rpmrebuild" installed on your system.

~
gt; sudo dnf install alien rpmrebuild

Both tools are part of RPM Fusion, so you will need to have that enabled too on your system. This is however outside the scope of this post.

Converting the Debian package to RPM

Download a fresh copy of BinDiff from the Zynamics web site

https://www.zynamics.com/software.html


Then convert the package using the tool alien

~
gt; alien -v -k --to-rpm bindiff420-debian8-amd64.deb

An RPM file is now produced which then may be installed. However you will most likely soon run into a small problem. It won't install! The following error may be produced:

~
gt; sudo dnf install bindiff-4.2.0-1.x86_64.rpm [sudo] password for user: Last metadata expiration check: 0:45:20 ago on Thu Oct 27 18:50:55 2016. Dependencies resolved. ============================================================ Package Arch Version Repository Size ============================================================ Installing: bindiff x86_64 4.2.0-1 @commandline 17 M Transaction Summary ============================================================ Install 1 Package Total size: 17 M Installed size: 32 M Is this ok [y/N]: y Downloading Packages: Running transaction check Transaction check succeeded. Running transaction test Error: Transaction check error: file / from install of bindiff-4.2.0-1.x86_64 conflicts with file from package filesystem-3.2-37.fc24.x86_64

This is a common and known problem which is easily fixed. The offending part is "file / from...". We need to remove that part from the RPM package using rpmrebuild.

~
gt; rpmrebuild -pe bindiff-4.2.0-1.x86_64.rpm

The tool will now open the manifest for the RPM file in an editor. You are looking for the section: %files There you will find a number of entries similar to the following:

%dir %attr(0755, root, root) "/"
%dir %attr(0755, root, root) "/usr"
%dir %attr(0755, root, root) "/usr/bin"
...

You then remove the offending entries based on the errors produced by dnf when trying to install the package. In this case, the first line.

%dir %attr(0755, root, root) "/"

When done, save the file and exit the editor. Rpmrebuild will now ask whether to rebuild or not. Answer Y.

If you are uncomfortable with the systems choice of editor, you can define your own by exporting a new EDITOR environment variable before running rpmrebuild:

export EDITOR=/path/to/your/editor/of/choice

When rpmrebuild is done, a copy of the modified package is now available in the directory:

/home/user/rpmbuild/RPMS/

You may now install it as usual.

~
gt; sudo dnf install bindiff-4.2.0-1.x86_64.rpm

Happy diff'ing!