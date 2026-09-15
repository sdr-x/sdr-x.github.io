---
date: 2026-09-14 02:05:00
layout: post
title: "Tracking In-Flight Starlink Routers Worldwide from the Ground Using IPv6"
thread: 2026091425
categories: starlink
tags:  Starlink Terminal In-Flight Airline Satellite Phased-Array Aviation LNB SDR AD9361 FPGA Ku-band IPv6 MPLS PoP GeoIP Mobile-Network
---

By identifying some static IPv6 addresses from publicly known Starlink address pools that remain online for a period of time while their access point (PoP) and MPLS information changes over time, 
and cross-referencing these observations with flight data (ADS-B), it is possible to identify Starlink routers installed on aircraft and subsequently measure in-flight Starlink performance from ground!

Well-known Starlink researcher Professor Jianping Pan from the University of Victoria, Canada, recently presented a paper titled Measuring Starlink Aviation Around the World at the IMC'26 conference in Germany. 
The first page of the paper has been made public, although the full paper is not yet available.

Based on the publicly available first page of the paper, the basic principle appears to be roughly as follows (confidence: 90%):

 1 . First, build a list of candidate addresses

Starlink publicly provides IPv6/GeoIP information:

https://www.ietf.org/slides/slides-ipgeows-paper-position-paper-starlink-00.pdf

This information can be used to obtain a large number of Starlink IPv6 prefixes. Based on Starlink's address allocation patterns, researchers can then generate possible customer-router addresses, such as ::1 within a /56 prefix.

 2 . Send packets to these addresses from a computer or ordinary ground-based server

There is no need to be on a Starlink-equipped aircraft and connected to its in-flight Wi-Fi. Researchers can probe candidate addresses from a home IPv6 network, a university network, cloud servers, or other Internet-connected locations using tools such as ping6 and traceroute6 to determine which addresses are actually online.

 3 . Look for IPv6 addresses that "move"

A typical Starlink residential user is associated with a relatively stable network region. But an aircraft-based Starlink user has a distinctive property: the IPv6 address of the aircraft's Starlink router remains unchanged, while the Starlink PoP it connects through changes as the aircraft flies.

For example:
```
IPv6: 2605:xxxx:xxxx::1
        ↓
Doha PoP (Starlink Network to Internet interface in Doha, Qatar)
        ↓ aircraft in flight
        ↓
New York PoP (Starlink Network to Internet interface in New York)
```
The IPv6 address stays the same, but the network access point changes.

This PoP information can be observed from information returned by tools such as traceroute.

 4 . Continuously record path changes

Run traceroute measurements every few minutes from multiple locations and record:

IPv6 address + time + RTT + PoP + MPLS information

If a particular IPv6 address is found to:

- remain online for a long period, comparable to the duration of a flight;
- remain unchanged throughout that period; and
- continuously switch between Starlink PoPs in different geographic regions;

then it is very likely to belong to a Starlink router installed on an aircraft.

 5 . Finally, match the observations with flight data

The activity period, PoP changes, and latency variations of the IPv6 address can then be compared with ADS-B data and flight trajectories to further determine:
```
IPv6 address
      ↓
a specific aircraft
      ↓
an airline
      ↓
a specific flight
```
The paper calls this Starlink aviation measurement technique "outside-in": instead of measuring from inside the aircraft toward the Internet, measurements are performed from Internet-connected ground nodes toward the aircraft.

Most previous research has used an "inside-out" approach, in which researchers or users must actually board a Starlink-equipped aircraft and measure and record Starlink performance through the in-flight Wi-Fi network. 
There are also several crowdsourced datasets containing Starlink in-flight performance measurements reported by passengers via this "inside-out" way.

The paper also cross-validates the "outside-in" approach against established and reliable traditional "inside-out" measurements, demonstrating the effectiveness and accuracy of the outside-in method.

There is little doubt that the publication of this technique could spark considerable interest among researchers worldwide—and perhaps among hackers as well. It may lead to a chain of follow-up developments. How long this "outside-in" research window will remain open remains to be seen.

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
