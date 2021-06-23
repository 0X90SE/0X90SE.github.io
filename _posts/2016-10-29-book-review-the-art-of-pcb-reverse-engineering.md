---
date: 2016-10-29 14:07:00+02:00
title: Book Review - The Art of PCB Reverse Engineering
header:
  image: /assets/images/posts/computer-cropped.jpg
teaser_image_path: /assets/images/posts/computer-cropped.jpg
toc: true
toc_label: "Content"
toc_sticky: true
tags:
- Reverse Engineering
- Electronics
- Tools
---

There are many books on SRE - Software Reverse Egineering, however, there are less books on hardware reverse engineering, especially in the area of electronics. However there is an interesting book concerning PCBs (Printed Circuit Boards) and how to reverse engineer them - The Art of PCB Reverse Engineering by Ng Keng Tiong.

My View of the Book

This book does not primarily focus on reverse engineering printed circuit boards from a security perspective, which I admit I thought at first. Its more of a personal story where the author tells you about the journey reviving ancient hardware that has reached end of life some time ago or where the blue prints simply are no longer available. A journey along with his experiences he then shares through this book.

The book is divided in to the following main chapters

Before You Begin
Basic Preparation Work
Laying Out the PCB Artwork
Wiring Up The Schematics
Advanced Topics
Going From Here

The book guides you throughout the whole process ranging from all necessary preparations, documentation, tools needed to the final document visualizing the PCB.

One could argue that it is more a book on Visio than a book on reversing PCBs, it is! But this may perhaps also be one of its strengths as it describes the importance of documentation of the process. I personally find it a good feature being a friend of thorough and easy to read documentation and all. I was thus quite positively surprised to find the extensive documentation used throughout the reversing processing. Many underestimate the importance of documentation, which ironically is one of the reason why this book was written in the first place.

Throughout the book I could however not get rid of the feeling that I wanted more on the PCB reversing techniques and less on creating templates in Visio. This perhaps has to do with that the PCB reversing process is quite simple in the end. When stripped of all components it is "only" a matter of following, mapping out and documenting a number of more or less hidden wires. That's why I for example like the "Gold finger" technique explained in the book.

Perhaps what I would like to see more of in the midst of explaining component features and how to draw them in Visio is also the different characteristics and features a PCB can hold. Perhaps explaining why traces are drawn in a certain way, which may tell you a lot of the PCB as well as PCB layer design and characteristics such as grounding, shielding, vias and all the rules it has to adhere to etc.

Despite of the above, the book also has a number of valuable and sometimes hard to get experience and how to avoid common pitfalls regarding both Visio and the process of reverse engineering PCBs.

I also like that the author also pay some attention to the components encountered on the PCB and explain them a bit even though there are books more dedicated to the topic such as The Art of Electronics and Practical Electronics for Inventors, which I also would like to recommend.

Areas of Improvement

On the downside, the much talked about Visio templates used throughout the book are not yet available for sale almost two years after the release of the book. Only the templates for discrete components are available for free download, which is quite sad:

http://www.visio-for-engineers.com/store.html


It would also be nice to see more dedicated electronics tools involved too like KiCAD, Eagle, DipTrace or similar, which is more suited for reconstructing a PCB than Visio as you then are able to actually engineer the PCB out of the exported Gerber files for example.

https://en.wikipedia.org/wiki/Gerber_format


As the author is trying to revive/repair old or undocumented hardware, his process is quite non-destructive. I would like to see some other tricks discussed like for example delayering a PCB. A subject that Joe Grand, a dedicated hardware hacker at Grand Idea Studio writes about in a few great presentations and papers:

http://www.grandideastudio.com/pcbdt/


It would also be nice to see more advanced topics like chip-off techniques as this in many cases is a very necessary process in order to be able to both document and interact with parts of the PCB or its components. This is especially true when it comes to BGA - Ball Grid Array components. There is a bunch of cheap and easy to use tools available for this purpose.

Perhaps way out of scope and a bit too advanced as it may be out of reach for many, it would be nice to mention methods like computer tomography 3D scanning or fluoroscopic techniques available which may be available at university campuses for example.

Conclusion

It is a nice book even if it is quite heavy on the Visio focus. If you are a beginner or intermediate user of Visio from an engineering perspective and only have a basic understanding of electronics and the components used on a PCB, this is an excellent book.

However, if you are an advanced Visio-PCB-ninja-engineer looking for something with a focus on things from a security perspective you may find this book a bit too easy or off topic and you may easily be distracted by other things while reading it... You are then perhaps better off reading the many excellent blogs out there.

The Art of PCB Reverse Engineering ISBN: 1499323441
Price range: $70-$80
Format: Paperback