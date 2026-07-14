---
date: 2026-07-14 08:01:00
layout: post
title: "Starlink Analysis – Supplement(10):Discussion of Modems, RF Links, Beams, and Capacity"
thread: 2026071411
categories: starlink
tags: OFDM Starlink modem RF-Link beam phased-array DBF Digital-Beamforming satellite spectrum-efficiency capacity bandwidth
---

I continued reading the Starlink patent US12003350. Some aspects of the satellite capacity calculation remain ambiguous, and I welcome further discussion. The patent was filed in early 2021, so readers should consider its timeliness accordingly.

The patent defines links and RF links as follows.

* FIG.4 defines a link from the perspective of the user terminal (UT). A link is an L3 connection to a UT, including the associated MAC, PHY transmitter, PHY receiver, and one or more beams.
* An RF link is defined as a paired downlink (DL) and uplink (UL) channel starting from the MAC layer. The patent explicitly states that an RF link is a logical concept, consisting of one MAC, one PHY transmitter, and one PHY receiver. It also states that an RF link may use multiple beams. One possible guess is that when a UT is located near the boundary between two beams, a single RF link may serve that terminal through both beams.

The patent states that a satellite can have 32 RF link pairs, and each RF link may include multiple frequency bands:

“In an embodiment, the SAT 104 can include 32 RF link pairs (e.g., 32 UL RF links and 32 DL RF links), and each of the DL and UL links can include a plurality of frequency bands (e.g., a DL can have 8 frequency bands, a UL can have 8 frequency bands).”

We already know that the Ku-band user link consists of:

* Eight 250 MHz downlink channels (2 GHz total)
* Eight 62.5 MHz uplink channels (500 MHz total)

A natural interpretation is that a single RF link can span the entire bandwidth by including all eight channels.

However, the wording is ambiguous.

Does the patent mean that each link can have eight frequency bands, or that each RF link can have eight frequency bands?

Does “can have” mean that eight bands are always active simultaneously, or simply that up to eight bands are supported?

Must these eight bands be eight non-overlapping frequency channels, or could they instead represent reuse of the same channel across eight geographically separated beams?

In my previous analysis, I assumed that each RF link simultaneously carried eight 8*250 MHz downlink channels. This resulted in a total downlink capacity of 320 Gbps for 32 RF links.

However, 320 Gbps is much higher than the publicly reported capacities of current Starlink satellites. Public sources typically report approximately 20 Gbps for first-generation satellites and around 80 Gbps for V2 Mini satellites. Even the not-yet-launched V3 satellites are reported to provide about 1 Tbps of downlink capacity —— not super higher than 320Gbps.

Therefore, either my previous blog interpretation was incorrect, or the 2021 patent describes a configuration that was considerably more ambitious than the satellites eventually deployed so far.

Additional information in the patent provides further insight.

Regarding the relationship between the modem and the RF link, FIG.5 suggests that a modem contains one PHY transmitter and one PHY receiver, implying a one-to-one correspondence between a modem and an RF link.

However, later figures suggest a more complex implementation. A modem may contain two transmitter/receiver pairs, supporting two independent 250 MHz Ku-band downlink channels. In that case, one modem would contain two RF links.

FIG.20 illustrates an example satellite configuration with four modems for Ku band downlink. Each modem supports two 250 MHz channels, and each channel corresponds to one beam, resulting in a total of eight 250 MHz downlink channels. According to the earlier RF link definition, this is an example of one modem (or RF link) supporting two frequency bands, rather than eight.

These eight 250 MHz channels provide a total downlink capacity of 10 Gbps, noticing 5 bit/s/Hz in my previous blog.

The patent also notes that the four-modem configuration in FIG.20 is only an example and could instead use 16 modems.

With 16 modems, the satellite would support 32 independent 250 MHz downlink channels, providing a total downlink capacity of 40 Gbps. This example also aligns naturally with the patent’s reference to 32 RF links.

Based on this interpretation, the Ku-band downlink capacity explicitly described in the patent falls in the range of 10–40 Gbps, which is much closer to the reported capacities of current Starlink satellites than the 320 Gbps estimated in my previous blog analysis.

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

