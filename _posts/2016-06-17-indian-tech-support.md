---
2016-06-17
---

Perhaps you've heard about them? The Indian Microsoft technical support that has detected that your computer is infected with viruses and infested by hackers. And there they are like knights in shining armour promising to help you...and of course at a small fee...

Background

On occasion, I'm at home during workdays and got this weird phone call on a poor line. A woman speaking English with an Indian accent bursting through like a ton of bricks. I could hardly understand a word she said. I soon understood what it all was about. I've heard of them many times. Its been in Swedish media; news papers, TV and radio: The Indian Microsoft Technical Support. Just to throw them off at that time, I asked them how on earth they could get indications on that my computer was infected by viruses, because I run Linux. Short silence on the other end of the line, followed by whispering with some co-scammers. And all of the sudden they just hung up on me!

Next time I want to set a trap...

In mid-February 2014 they call me again, and this time I'm a bit more prepared. I fired up my virtual machine, which in fact was a bit ill prepared, as well as my email address used for these purposes. They were all almost empty. However, I thought I'd give it a shot.

Modus Operandi

After just a few minutes of searching the web I find that all victims and all those who have played along tell the same tale, almost word by word. This indicate that we are dealing with the same scammers in most cases. This may also indicate that the ones talking to their victims are merely the puppets, while the pupeteers are the ones that are cashing in on these scams.

They start off by trying to gain your confidence by telling you that they are an authorized support organization contracted by Microsoft. Once some trust is gained, they tell you that they can fix this for you. However, they need to remote control your computer. For this, they direct you to TeamViewer and tell you to establish a waiting connection to which they will connect.

Once connected, they in fact guide you on the phone, letting you, the user, perform most of the operations as a part of them gaining your trust even more. Now they enter the phase where they try to plant as much fear and uncertainty in you as possible. By telling the victim to enter some commands they give you a small tour in showing how badly infected you are by viruses and how many hackers that have access to your computer. And for some reason, it is very important that you count all entries and tell them about it.

As seen in the image to the left, there are 6 established connections - 2 active to TeamViewer, as they are connected to my machine, and 4 local connections. According to the "Indian Tech Support" these are active hackers logged in onto my computer. In a sense they are right, sort of, but I wouldn't say hackers. Two of them are the Indian scammers. Next they want to show me all infected files on my computer and ask me to run the standard command "tree" which lists all directories, sub-directories and their contents in a tree like structcture. Be sure to hold your breath now - All of those standard Windows files we see listed here are all viruses planted by the hackers!

Shortly after stating this, they ask me what I see on the screen. As text is being written in the command window, they want to give me the impression of that Windows is telling me that the computer is corrupted and that I need a new license key. I think someone at Microsoft needs to brush up his/her English a bit. The timing of the text being persented tells me that someone at their end is typing it as we speak over the phone. Hilarious!

Their Tools

When they have given you this small fearful guide, they of course tell you that they can fix all this for you. And of course there is a small fee involved in this. Trying to be professional, they offer you; a two year, a three year, a five year and life time support plan. This is pretty amazing in itself since Windows XP will reach end of support in April this year (April 2014)! By directing you to a poorly designed payment web page they ask you to enter your credit card details.

I on my part gave them fake details, which of course was bound to fail...and it did. Tired of a couple of failed attempts, they direct me to another site where the payment magically was accepted!

I start to notice that they were getting a bit frustrated as they couldn't get the payment though properly, all despite the above success. This is where they start to act without my consent and open one of my decoy files placed on my desktop; passwords.txt.

It contains the decoy email address and a fake non-existing PayPal account. And of course they want to use this account.

They "convince" me that they will try to solve this in one way or another and that I need to open my email account. When done, I must not touch keyboard nor mouse while they work on my computer. Also they tell me that they need to hang up and call me back later. Like a obedient user I fully comply. For a while I watch them doing practically nothing. After ten minutes or so, the phone starts ringing. However, I do not pick up. I want to see if they do something else to my computer if they think I've left the computer unattended. After nearly thirty minutes, they start Notepad as a mean of communications, asking me if I'm there.

Still I don't reply. Using my email account, they send themselves a couple of emails saying that I'm very happy with the service they've provided. After another 10 minutes or so, I use the same Notepad window they started and tell them that it all was very entertaining and that all is recorded. Suddenly, they drop the connection.

Below is a short summary of the sites they are using:

Both the one above and the one below are located in Korea:

Below is a Host/IP address summary of systems involved in the scam:

Hostname	IP Address
www.etechpro.net	203.124.116.232
www.planetonlineservice.org	118.67.248.167
aispccare.com	182.50.158.65
www.isaackorea.net	61.74.61.253
pg.ksnet.co.kr	210.181.28.137

Whether these site are all scams or misused in the Indian support scams is unknown to me as I have not digged any further into this. I do not condone any illegal activities against these sites.

Conclusion

First off, I waited for the moment where they would plant some malware in my system in order to gain some persistence. However, to my great disapointment my wait was all in vein. They are nothing more than a bunch of simple credit card scammers using some cheap theatrical moves built on FUD in order to steal your credit card details. The overall MO/Manuscript is somewhat well rehearsed but at the same time very poorly orchestrated. We may also laugh at "the stupid user" for buying such a cheap trick. However, laughing at the "stupid user" won't change a thing. We should instead ask ourselves why some people buy these cheap tricks, because its certainly not stupidity. Perhaps we are dealing with carelessness as a result of great ignorance? No, I think we should instead ask ourselves how we can prevent this. Perhaps with the clever use of technology and education of the general population?