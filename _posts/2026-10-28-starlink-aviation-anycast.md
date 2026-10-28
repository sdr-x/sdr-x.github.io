---
date: 2026-10-28 05:01:00
layout: post
title: "Unpacking the Evolution of Starlink Aviation Mobile IP Solutions Gen 1–3: How Can Ground-Based Internet Servers Find an Aircraft Anywhere in the World?"
thread: 2026102844
categories: starlink
tags:  Starlink Aviation Router Anycast IPv6 IPv4 BGP MPLS Internet-Traffic-Engineering Gateway PoP Autonomous-System AS
---

When you watch Netflix video in-flight, have you ever thought that how does the Netflix video server knows which Starlink gateway/pop on the ground it shall send the data packets to? 

The answer is counter-intuitive: The server doesn't need to know. It just send the packet to a nearest/arbitrary Starlink gateway/PoP.

Another counter-intuitive thing is that: Starlink Aviation heavily relies on the fiber on the ground.

The well-known paper "Measuring Starlink Aviation Around the World" was recently made publicly available in its entirety. Out of curiosity, I looked into the anycast mechanism mentioned in the paper and wrote this article about it (99.2% confidence). 
I haven’t worked much with network protocols myself, so this has also been a learning experience for me.

At the end, I’ll also introduce the first-generation Starlink aviation solution for mobile IP, which resembles the HLR/VLR architecture used in early era of mobile communications, as well as the second-generation solution based on 
dynamic aircraft IP addresses (GeoIP prefixes). I’ll discuss the advantages and disadvantages of each. After that, you’ll have a much better appreciation of the elegance of the current, third-generation solution of Starlink aviation.

Let’s get started.

1 . First, Think About Your Home Wi-Fi Network

We all know that a home Wi-Fi network has something called a gateway—for example, the device supplied by your internet service provider (ISP).

This gateway is actually a router. It has its own IP address, which is generally a fixed address assigned by your ISP. Because it remains connected to a fixed port on an upstream router at the ISP’s facility, 
there is usually no need to change its IP address. After all, your house doesn’t grow legs and move around!

The Wi-Fi router routes packets between your home network and the external internet, allowing the various devices in your home to communicate with other devices and servers online.

For example, suppose an internet server needs to send a packet to a device on your home Wi-Fi network, such as the phone you’re using to binge-watch a TV series. 
The packet leaves the server, passes through multiple routers, and eventually reaches your home Wi-Fi router. The final hop is from your home Wi-Fi router to your device.

How do routers across the internet know which router to forward a packet to next?

Because routers exchange routing information using various protocols and maintain routing tables—a kind of map of reachable network destinations. 
When a packet arrives, a router examines its destination IP address and consults its routing table to determine the next hop.

When your phone accesses the internet over your home Wi-Fi, it has an IP address within your home network, and the internet knows to send packets destined for your connection toward your home Wi-Fi router. 
When your phone accesses the internet at work, packets destined for it are instead routed toward your company’s Wi-Fi router.

2 . An Aircraft Equipped with Starlink Is Like a Flying Home

An aircraft equipped with Starlink is similar to a home Wi-Fi network. Imagine the aircraft as a house in flight. The cabin Wi-Fi is equivalent to the Wi-Fi network in your home, and passengers use the Starlink router to access the internet.

But unlike a stationary house, an aircraft-mounted Starlink router continually connects via satellite to different Starlink ground gateway sites, or Points of Presence (PoPs), depending on the aircraft’s location and flight path. 
These PoPs provide the aircraft with access to the internet.

Now, let’s think more carefully about that Starlink router on the aircraft. This is where things get interesting.

As mentioned earlier, a home gateway router has its own IP address, which is generally fixed because it has a stable connection to an upstream router at the ISP’s facility.

But an aircraft-mounted Starlink router might be in Asia one moment, Europe the next, and the Americas later on. Although this router can also have a fixed IP address—and the current, third-generation solution does indeed work this way—the aircraft 
is constantly moving between different network access points.

Suppose a passenger is streaming a TV series on the aircraft. Netflix’s servers know that packets destined for this passenger’s connection must be sent to the aircraft’s Starlink router IP address, 
after which the router forwards them to the passenger’s device.

But Netflix’s servers are not located inside the Starlink gateway/PoP site to which the aircraft is currently connected via satellite. Netflix simply sends packets destined for the aircraft’s IP address into the internet.

How does the vast internet know where to send those packets? The aircraft is constantly moving and changing PoP! With so many Starlink PoPs around the world, which one should the internet routes the packets to so that they can reach the aircraft?

To explain this, we first need to clear up a misconception about the internet itself. In a sense, the internet doesn’t actually exist as a single, unified network!

3 . The Internet Is Really a Network of Networks

There is no single company, ISP, or organization that builds, operates, and manages the entire internet as one unified network.

What we call the internet is actually a collection of countless interconnected Autonomous Systems (ASes).

You can think of an AS as a network built and operated by a company or network operator to serve its own customers. Every AS is assigned an Autonomous System Number (ASN).

Google’s network is AS15169, while Starlink’s network is AS14593. AT&T also has AS number(s).

Within its own AS, an operator can provide connectivity between its customers. For example, one AT&T customer can communicate with another AT&T customer.

But what if customers within one AS could communicate only with other customers in the same AS? That would be rather inconvenient.

Suppose a AT&T customer wanted to communicate with a customer of european operator. Would they have to register as a customer of the european operator and establish a dedicated link across the Atlantic Ocean just to communicate with that person?

Of course not.

As a customer of AT&T’s AS, you can communicate with users on other ASes without becoming a customer of those networks. The solution is to interconnect the ASes.

To enable its customers to communicate with users on other networks, each AS establishes connections from one or more of its border routers to border routers belonging to other ASes.

If the routers belonging to two ASes happen to be in the same facility/data-center, connecting them may be as simple as plugging in a fiber-ethernet cable. But what if they are far apart—for example, on opposite sides of the Atlantic Ocean?

No problem. Specialized companies provide connectivity services between networks. Of course, you have to pay for it.

So, the internet is essentially a vast collection of independently operated networks interconnected in a variety of ways.

4 . How BGP Connects These Networks

Traffic between ASes flows through their border routers. These routers use a protocol called BGP (Border Gateway Protocol) to exchange routing information.

In simplified terms, each AS announces which globally unique public IP address prefixes it has in use. Other ASes can then use this information to determine where to forward packets destined for those prefixes.

In effect, an AS tells other networks:

“These IP address ranges are reachable through my network. If you have packets destined for them, forward those packets to my network.”

For Starlink, the border routers connecting its network to other ASes are distributed across its global network infrastructure, including its ground gateway sites and PoPs.

Imagine that Starlink has obtained a large pool of public IPv6 addresses. When an airline becomes a customer, Starlink installs one or more Starlink routers on the aircraft and assigns each router one or more fixed public IP addresses.

Starlink can then announce the relevant IP prefixes to other networks through multiple PoPs around the world.

The message to the rest of the internet is essentially:

“These IPv6 addresses are reachable through the Starlink network. If you have packets destined for them, you can forward those packets to an Starlink PoP.”

5 . Anycast: Send the Packet to Any Starlink PoP

Now we can return to our original question.

Regardless of which AS hosts the ground server an aircraft passenger is accessing, when that server needs to send a packet to the aircraft’s Starlink router, it can forward the packet toward a Starlink PoP selected by the internet’s routing system.

Starlink makes the aircraft’s IP address reachable through multiple PoPs. Each of those PoPs can act as an entry point into the Starlink network.

In that sense, the internet can send the packet to any Starlink PoP, because Starlink’s routing announcements make the relevant destination reachable through multiple entry points.

This is the idea behind anycast:

To reach a particular IP address, send the packet to any one of the multiple network entry points announcing reachability to that address.

The ground internet does not need to update its routing tables every time the aircraft changes location. The externally visible routing information can remain unchanged even while the aircraft moves between satellite links and ground gateway sites.

Once the packet reaches a Starlink PoP, the remaining task becomes much easier.

The packet has entered Starlink’s own AS. Starlink’s internal network and network control systems can determine which PoP is currently serving the aircraft and forward the packet accordingly.

The packet can then travel along a dedicated path within the Starlink network—for example, using an MPLS tunnel over dedicated fiber network—to the PoP currently connected to the aircraft. That PoP sends the packet to the aircraft via satellite.

Starlink operates dedicated long-distance fiber-optic links within its network, potentially supplemented by inter-satellite links. These connections can allow packets to move rapidly between Starlink PoPs without 
relying entirely on the external internet for the long-distance portion of the journey.

An Example

Imagine a server on the US West Coast needs to send a packet to a Starlink router aboard an aircraft flying over Asia. The aircraft is currently connected via satellite to a Starlink ground PoP in Asia.

The server only needs to forward the packet to a Starlink PoP near the US West Coast.

That PoP then forwards the packet across Starlink’s private, high-speed trans-Pacific network connection to the Asian Starlink PoP currently serving the aircraft.

Finally, the Asian PoP transmits the packet via satellite to the aircraft.

This is the essence of Starlink’s current approach: an aircraft-mounted router can retain a fixed IP address while flying around the world, without sacrificing efficient network paths.

This is the third-generation solution. Let’s look at the first two generations and understand why the third is so elegant.

6 . First Generation: Always Route Through the Home PoP

In the first-generation solution, an aircraft would always use its designated home PoP, regardless of where it was flying. This might be a Starlink ground PoP near the airline’s headquarters or primary operational base.

The concept is similar to the Home Location Register (HLR) and Visitor Location Register (VLR) used in early mobile communication networks.

The routing tables of the public internet would not need to change as the aircraft moved. They would direct packets destined for the aircraft ALWAYS to its home PoP, which would then forward them to the aircraft, either directly via 
satellite or through Starlink’s internal network or a satellite backbone.

The disadvantage is obvious.

Suppose a server on the US West Coast needs to send a packet to a Singapore Airlines aircraft also flying nearby over the US West Coast, but the aircraft’s home PoP is in Singapore.

The packet first travels across the public internet, crossing the Pacific to reach the Singapore-based home PoP. That PoP then uses Starlink’s internal network or a tunnel—over fiber, satellite, or both—to send the packet back across the 
Pacific to the US West Coast PoP, because it knows that the aircraft is currently connected to that PoP.

The packet has effectively traveled halfway around the world and back.

In reality, the packet could have been delivered directly to the US West Coast PoP and sent to the aircraft via satellite. Instead, it takes a substantial detour, resulting in unnecessary latency and inefficient traffic distribution.

7 . Second Generation: Change the Aircraft’s IP Address as It Moves

In the second-generation solution, the aircraft changes its IP address as it moves between Starlink PoPs.

The aircraft’s IP address reflects its approximate geographical location through a GeoIP prefix—a globally coordinated arrangement in which public databases map IP address ranges to approximate geographic locations.

When a ground server needs to send a packet to the aircraft, the internet can use the aircraft’s current IP prefix and the associated routing information to direct the packet toward a Starlink PoP serving the relevant geographic area. 
That PoP then forwards the packet to the aircraft via satellite.

The main advantage is that packets no longer need to make the long detours associated with the first-generation approach.

However, the second-generation solution has two disadvantages:

The aircraft’s IP address changes during flight. Some higher-layer network applications and protocols, including TCP connections, may be interrupted and require reconnection when the IP address changes.

The external internet independently determines the route to the PoP. Because traffic travels through networks outside Starlink’s control, Starlink has less control over the resulting latency and quality of service.

8. Third Generation: Fixed IP, Efficient Routing

The third-generation solution retains the second generation’s main advantage—avoiding unnecessary detours—while addressing both of its major drawbacks.

First, the aircraft’s IP address remains unchanged. Higher-layer applications and protocols, including TCP, therefore do not need to reconnect merely because the aircraft moves between PoPs.

Second, the internet only needs to deliver the packet to any (could be the closest one to the traffic source) Starlink PoP. Starlink then handles the long-distance forwarding entirely within its own network, 
using an efficient internal path—for example, an MPLS-based private fiber path—rather than relying on the external internet to carry the packet all the way to the aircraft’s current location.

This gives Starlink greater control over routing, latency, and quality of service.

The key idea is to separate the aircraft’s stable network identity from its constantly changing physical location.

The internet gets the packet into Starlink’s network via any PoP. Starlink takes care of getting it to the aircraft.

If you’ve made it this far and found the article helpful or learned something new, please consider liking, supporting, and sharing it.

And if you spot anything inaccurate or incorrect in the article, I’d welcome your feedback!


<div id="disqus_thread"></div>
<script type="text/javascript">
    /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
    var disqus_shortname = 'jiaoxianjun'; // required: replace example with your forum shortname

    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {
        var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
        dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>


<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-01GGQ8JZW7"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-01GGQ8JZW7');
</script>

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-1542618827905251"
     crossorigin="anonymous"></script>
