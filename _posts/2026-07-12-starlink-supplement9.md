---
date: 2026-07-12 08:01:00
layout: post
title: "Starlink Analysis – Supplement(9):Spectral Efficiency and Satellite Capacity"
thread: 2026071211
categories: starlink
tags: OFDM Starlink modem 64QAM feeder-link gateway satellite spectrum-efficiency capacity bandwidth
---

I continued reading the Starlink patent US12003350 and summarized several system parameters. The patent was filed in early 2021, so readers should consider its timeliness accordingly.

According to FIG.21, Starlink uses two types of modem chips/modules based on capability of demodulation bandwidth and data rate.

Modem for the Ku-band User Link

This modem is used for:

* Terminal reception of a 250 MHz Ku-band downlink channel.
* Satellite reception of four 62.5 MHz Ku-band uplink channels (4 × 62.5 MHz = 250 MHz).

It provides a peak data rate of 1.25 Gbps over 250MHz bandwidth.

Modem for the Ka-band Feeder Link (satellite — gateway)

This modem is used for:

* Gateway reception of two 250 MHz Ka-band downlink channels (2 × 250 MHz = 500 MHz).
* Satellite reception of a 500 MHz Ka-band uplink channel.

It provides a peak data rate of 2.5 Gbps over 500MHz bandwidth. 

From these informations, the Starlink modem achieves a spectral efficiency of 5 bit/s/Hz.

The highest modulation supported is 64QAM, whose ideal uncoded spectral efficiency is 6 bit/s/Hz. After accounting for channel coding, packet headers, and other protocol overhead, the resulting 5 bit/s/Hz is both efficient and a reasonable engineering result.

In deployment:

* A user terminal contains a single modem.
* Satellites and gateway stations contain multiple modems, forming a modem pool onboard.
* The modems are connected in a daisy-chain architecture rather than a star topology to aggregate capacity.

Each Starlink satellite user link provides:

* 32 Ku-band downlink RF chains
* 32 Ku-band uplink RF chains

Each RF chain supports eight parallel frequency channels:

* Ku uplink: 8 × 62.5 MHz = 500 MHz, with a total data rate of 2.5 Gbps
* Ku downlink: 8 × 250 MHz = 2 GHz, with a total data rate of 10 Gbps

Each RF chain supports approximately 200 simultaneously active terminals. Each satellite supports approximately 6,400  (32 chains times 200 UT/chain) simultaneously active terminals.

Assuming equal resource allocation:

* Average uplink per terminal: 12.5 Mbps (2.5Gbps/200)
* Average downlink per terminal: 50 Mbps (10Gbps/200)

In practice, depending on service tiers and network oversubscription, a satellite can serve significantly more than 6,400 subscribed users.


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

