---
date: 2026-07-06 01:05:00
layout: post
title: "Starlink Analysis – Supplement (2): A Hypothesis on the Unique Word (UW)"
thread: 2026070645
categories: starlink
tags:  OFDM Starlink modem CDMA Spreading-code Signature-sequence Multi-User-Detection Unique-Word
---

I continued reading the Starlink patent US12003350 to kill the time.

Facts

Each uplink packet begins with a Unique Word (UW) consisting of:

* An identical 128-sample sequence repeated 8 times.
* A 48-sample cyclic prefix (CP).
* The first 128-sample sequence is phase-inverted by 180° relative to the remaining seven repetitions.

However, it is still unclear whether this 128-sample sequence is identical for all user terminals. If different terminals use different sequences, how are they assigned? For example, are they derived from the terminal’s unique MAC address?

What the patent says about the Unique Word

According to the patent, the UW is used to:

* Detect uplink bursts.
* Estimate the carrier frequency offset (CFO) of each terminal.
* Allow signals from multiple terminals to overlap.
* Estimate CFO by detecting the phase rotation of a set of peaks associated with differential metrics:
    “The burst detection component further can identify a phase rotation of a set of peaks associated with differential metrics in the received waveform to determine the estimation of the carrier frequency offset.”

A hypothesis

Based on these descriptions, it seems likely that each terminal uses a different UW (i.e., a terminal-specific signature sequence). Otherwise, it would be difficult to estimate the CFO of individual terminals when their uplink signals overlap.

In this sense, the UW resembles a signature sequence in a CDMA system.

An interesting observation

From the captured uplink waveform, the full-bandwidth UW is immediately followed by partial-bandwidth (RB: resource-block) OFDM symbols.

This suggests that a UW lasting only one OFDM-symbol duration is sufficient for uplink access. The satellite appears able to identify the transmitting terminal, estimate its CFO, and perform compensation using only the UW at the beginning of each packet, even multi terminals’ signal are overlapping.

Compared with the conventional random access procedures used in terrestrial cellular systems (e.g., the four-step contention-based or three-step contention-free procedures in LTE), the Starlink approach appears much simpler and more efficient.

Because satellite links have much longer propagation delays, multi-step stateful random access procedures may introduce unacceptable overhead. This appears to be an optimization specifically designed for satellite communications.

Why might this work?

My hypothesis is that Starlink terminals perform relatively accurate propagation-delay and Doppler pre-compensation before transmission.

As a result, even if the UWs from multiple terminals overlap, they arrive at the satellite in approximate time alignment (perhaps with timing errors smaller than the CP duration). This would significantly reduce the complexity of detection or enable more advanced multi-user detection algorithms.

Another possible reason is that the number of active terminals within a single satellite beam is relatively small—much lower than in a terrestrial cellular cell—making multi-user detection more manageable.

In contrast, terrestrial cellular systems such as LTE generally do not assume timing or Doppler pre-compensation during PRACH random access. Consequently, the base station must handle a fundamentally asynchronous, CDMA-like multi-user detection problem. This is more challenging for the network, but it keeps the terminal implementation simpler. Reliable identification of different terminals is then achieved through the subsequent multi-step random access signaling procedure.

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

