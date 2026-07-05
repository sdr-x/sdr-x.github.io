---
date: 2026-07-05 15:05:00
layout: post
title: "Starlink Analysis – Supplement (1): A Unique Modem Design Philosophy"
thread: 2026070545
categories: starlink
tags:  OFDM Starlink modem feeder-link DPD CFR
---

During the holiday, besides spending time with my family, I passed some spare time by reading the well-known Starlink patent US12003350.

Today I only read the first small section, but I already found two interesting points.

1. A radical departure from the traditional satellite modem design philosophy

In conventional satellite communication systems, the modem design differs significantly between the user terminal, the satellite payload, and the feeder link (the satellite-to-gateway link). This is because these links operate under substantially different conditions, including frequency band, bandwidth, data rate, operating environment (mobile vs. fixed, multipath characteristics, antenna capability, etc.). As a result, each modem is typically optimized specifically for its own role.

In the Starlink system, however, the user terminal, the satellite, and the gateway station all use a common OFDM modem architecture. The same modem is adapted to different links and hardware characteristics primarily through what the patent describes as a “configurable” design.

This is yet another major break from traditional satellite communications. In terms of how disruptive it is, I would say it rivals Starlink’s earlier decision to deploy OFDM on satellites at large scale.

What surprised me even more is that the feeder link (satellite to gateway) also uses essentially the same OFDM modem as the user link (satellite to terminal).

Traditionally, feeder links operate in higher frequency bands. Gateway stations are equipped with high-gain antennas and high G/T, and there are many mature high-speed single-carrier modems available for such links. Compared with wideband OFDM, high-speed single-carrier waveforms are much more friendly to power amplifiers.

In the Starlink system, however, essentially the same or a very similar OFDM modem architecture is used throughout the entire network. This undoubtedly simplifies the overall system design, reduces hardware cost, and decreases the number of different modem vendors that must be managed. It is an aggressive and bold engineering decision.

This is absolutely not the kind of design that would come from a traditional aerospace organization. Had such a proposal gone through an internal design review at a conventional aerospace company, it would almost certainly have been heavily criticized.

Only an outsider who is unconstrained by conventional thinking would be willing to attempt something like this.

2. There appears to be an error in the patent’s Summary section

The patent states:

“The modem can further include an orthogonal frequency division multiplexing baseband processing component and a digital front-end processing component for at least one of digital pre-distortion and a channel frequency response (CFR).”

Here, I believe CFR is incorrect.

It should most likely refer to Crest Factor Reduction (CFR) rather than Channel Frequency Response, because Crest Factor Reduction is commonly paired with DPD (Digital Pre-Distortion) in digital front-end processing for OFDM power amplifier linearization. The pairing of DPD and CFR is standard practice, whereas “Channel Frequency Response” does not fit naturally in this context.

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
