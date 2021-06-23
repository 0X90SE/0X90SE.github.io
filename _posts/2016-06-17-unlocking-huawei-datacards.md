---
2016-06-17
---

In Sweden the Huawei datacard is probably one of the most widely used datacards around. They are offered by operators to attract consumers to their services by offering them very cheap at low rates and such. However this comes at a price. Sometimes the consumers are locked to a single operator for up to 24 months. Even when this time has passed, the data card remains locked, which can be very unfortunate for the consumer. Let's go about and see how they can be unlocked...

One of these lucky days when looting the electronics waste bin, I found 15 of them datacards of different models, all Huawei, all branded by the same operator. So I figured they were all locked. Trying them out, all were operational and of course locked. I guess they were thrown away because of this, which of course is a terrible waste of resources as well. But do they really have to be?

Unlockers

Googling "huawei datacard unlocker" will give you overwhelming bunch of hits ranging from shamelessly priced programs to so called "free" unlockers. The latter proved to usually be stuffed with malware. They might just work as intended, but as a small bonus, your computer gets 0wned too! Always have the habit of checking unknown programs with Virus Total for example. It seems that DC Unlocker is the most popular of the commercial variants as it offer support for a wider range of products and a set of "advanced" features, such as activation of the voice functionality. Yes, most datacards are in fact fully functional cell phones. They only lack the physical mic and ear piece.

In sweden at least where unlocking is legal, there is a great number of small mobile shops that happily will unlock you cell phone or datacard for SEK 100-300 ($15-46). The price vary greatly. So we end up again paying a lot of money for something that is pretty simple. But how simple?

Unlocking

The actual unlocking part is no secret, nor is it advanced. The advanced part has already been dealt with by a guy who calls himself "sergeymkl", who actually discovered how to unlock the cards. When he analyzed the Huawei firmware he found that the actual calculation taking place is based upon a MD5 sum of the IMEI number along with a static salt value hardcoded into the firmware. Based on this MD5 sum, another calculation takes place involving simple XOR, which in the end results in the final unlock code.

First off, as the checksum needed in order to calculate the actual unlock code is based on the IMEI number, we can fetch it using the following AT command. You may use any terminal program of your liking for this purpose. Personally I use PuTTY.

AT+CGSN

And now for the "magic" part on which most commercial unlockers thrive upon. The process in calculating the unlock code can be done using the following piece of simple Python script:

import hashlib

def getCode(imei, salt):
        digest = hashlib.md5((imei+salt).lower()).digest()
        code = 0
        for i in range(0,4):
                code += (ord(digest[i])^ord(digest[4+i])^ord(digest[8+i])^ord(digest[12+i])) < (3-i)*8
        code &= 0x1ffffff
        code |= 0x2000000
        return code
 
# Insert your IMEI number here:
imei = "123456789012347"
 
print ("Unlock code is:\t" + str(getCode(imei, "5e8dd316726b0335")))
print ("Flash code is:\t" + str(getCode(imei, "97B7BC6BE525AB44")))

As mentioned earlier, the salt in this case is the constant hardcoded into the firmware of the data card.

If this feels uncomfortable for you, there are actually a few free code generators out there which just takes the IMEI number and calculates the unlock code for you without risk(?). However I cannot swear that you won't become a victim of a drive-by-download though ;) Use them at your own risk as well. The Python code above is the only thing completely without risk.

When done, the actual unlocking is done using one simple AT command:

AT^CARDLOCK=<unlock-code>

And in order to check the status of your data card lock status, the following AT command may be used:

AT^CARDLOCK?

This command will result in a string like the one below:

^CARDLOCK: 1, 10, xxxxx

The first value can range from 1 to 3, which have the following meaning:

1 = Unlock code need to be provided = Locked
2 = Unlock code does not need to be provided = Unlocked
3 = Locked forever...Sorry, nothing more can be done

The second value is a counter telling you how many attempts you have left to enter a valid code. If the counter reaches 0, the datacard will be in a locked state permanently! So if you find yourself in possession of a card having a first value of 1 and a second value of 0, you're out of luck. The third value is a 5 digit ID unique to the operator to whom the card is locked.
Activation of Other Features

This is perhaps the most shameless part of it all. Activating extra features on the data card such as voice support commonly comes at yet an additional cost. I've seen prices up to $15 for this! However the truth is that this feature only lies two AT commands away:

AT^CVOICE?

If it returns 0 you are all set, you are in PC Voice mode. However if it returns 1 you are in Earphone mode. You then need to set it to PC Voice mode:

AT^DDSETEX=4
Conclusion

If you find yourself in posession of a locked Huawei datacard, don't throw it away and don't go buy an unlocker from one of those profiteering gluttons making money on something so open and simple. Avoid downloading those closed source unlockers as well. One individuals desperate need usually goes hand in hand with the increased risk of that the solution to that specific need carrys some type of malware.

I'm currently writing a more user friendly Python script that will be more interactive to make it easier for the beginner.