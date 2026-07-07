---
date: 2026-07-07 00:01:00
layout: post
title: "Starlink Analysis – Supplement (3):UW Selection, Sampling Rate, Bandwidth, and Configurable Parameters"
thread: 2026070745
categories: starlink
tags:  OFDM Starlink modem feeder-link CFR DPD bandwidth gateway
---

I continued reading the Starlink patent US12003350 to kill the time.

The Unique Word (UW) at the beginning of each packet is indeed likely different for different users. However, it is associated with the channel ID, rather than the terminal’s MAC address. FIG.12 confirms this.

The meaning of CFR is also confirmed. It indeed stands for Crest Factor Reduction, not Channel Frequency Response. The patent states:

“Transmitter components including at least a crest factor reduction module and a digital pre-distortion module.”

The patent further confirms the following:

* Single-channel terminal uplink bandwidth: 62.5 MHz
* Dual-channel terminal uplink bandwidth: 2 × 62.5 MHz
* Satellite-to-terminal downlink bandwidth per channel: 250 MHz
* Satellite-to-gateway downlink bandwidth per channel: 250 MHz
* Gateway-to-satellite uplink bandwidth per channel: 500 MHz

Why is the gateway-to-satellite channel the widest?

Most likely because it carries the aggregated Internet DL traffic for a large number of user terminals, including web browsing and video streaming.

Channel bandwidth is changed by adjusting the sampling rate, while keeping the FFT size unchanged. This matches my earlier guess based on measured uplink signals.

The modem’s configurable parameters include:

- a bandwidth, 
- a number of pilot sym-bols, 
- a pilot band ollset, 
- a pilot averaging configuration, 
- a resource block size, 
- a user allocation in one or more bursts of a radio frame, 
- a time-domain cyclic guard band configu-ration, 
- a number of channel estimation symbols, 
- a length of' a cyclic prefix and postfix value, 
- a characteristic of a DC null, 
- a burst length, 
- a modulation and coding scheme, 
- an antenna delay adjustment 
- and a carrier frequency.


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
