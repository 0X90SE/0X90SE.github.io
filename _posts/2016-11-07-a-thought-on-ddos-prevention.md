---
2016-11-07
---

OK, here's a thought I came up with when having my wee dram in front of the fireplace the other day. We've come to learn to live with the day to day threat of DDoS attacks. Each of which seem to grow exponentially in magnitude, launched by literally anyone having some basic programming skills, time and sometimes some resources. The collateral damage is also usually unintended and huge. But what if we could stop many these things earlier?

As we all know, it is always a matter of resources (read botnet zombies etc.) in order to fill the bandwidth of the intended target(s), either by plugging the architectural bottlenecks, hitting the Achilles heel (read DNS infrastructure and similar) or by hitting the target directly.

Problem

No matter bandwidth there will always be a way to consume it. We also know that things start to choke the closer to the target you get.

Of course there are a number of DDoS protection mechanisms and services that may be deployed, some of which are quite powerful. But most of them only work up to a certain level and time and will choke at the end of the day anyway. That said, we also know that the attack is commonly quite weak at the sources.

What if we could stop the attack earlier, way earlier at the point where the packets leave the source networks and throttle the traffic before they even become a problem?

Perhaps One of Many Possible Solutions

I'm thinking an out of band infrastructure, more resilient towards attacks, dedicated to protect the Internet infrastructure on a global scale. There are plentiful techniques available as well as basic communications channels already in place which does not depend on the Internet infrastructure.

Upon detection the target network infrastructure sends a messages to the anti-DDoS infrastructure about the ongoing attack using the OOB channel. The source networks throttle the traffic to levels which the target network is able to handle.

The above image is very simplified, but shows the main principle.

This is of course is also a delicate matter and has a lot of questions and problems that need to be answered and solved, both technical and non-technical. As with any added complexity, there are new security concerns etc.

For example, what if a bogus DDoS detection signal is sent to a source network? This means that a  country could potentially shut down another country's Internet access. Well, this is already possible today by other means. But still, we need to figure this out. Perhaps this could be based on an automated "voting" and scoring system. If it from that can be concluded that a number of packets destined for the same target with several country sources are of the hostile sort, perhaps the link speed for the offending sources can be throttled via the OOB channel? The system could even be voluntary to abide by.

We also need to ensure that a DDoS attack cannot be launched with the single motive to lock out the source networks, thus mounting a reverse DDoS attack 180 degree  style.

There are of course numerous of other obstacles to overcome other than the technical ones too, such as politics, policies, economics, proprietary ones and the like. But the economic incentive should be huge though. No one can afford to have their country offline today! Being offline for one day may result in millions of dollars in lost revenue, just for one big company.

Perhaps the idea is stillborn...But I think we all can agree on the fact that we need to start thinking outside that (in)famous box.

At least, its a thought, even if not entirely thought through.
The whiskey was good though...