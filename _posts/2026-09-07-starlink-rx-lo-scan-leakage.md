---
date: 2026-09-07 03:05:00
layout: post
title: "Starlink Terminal Local Oscillator Leakage Reveals Rx Scanning and Further RF Calibration Discussion"
thread: 2026090795
categories: starlink
tags:  Starlink Terminal Cold-Start Lo-Leakage Satellite Self-Calibration Phased-Array Scanning LNB SDR AD9361 FPGA Downlink Ku-band
---

By monitoring local oscillator leakage from the Starlink terminal's receive chain, we can observe some characteristics, such as scanning, of the terminal's receiver behavior after cold start.

First, a brief explanation of how a receiver can be detected even when no signal is actively transmitted:

To receive a communication signal at a specific carrier frequency, a receiver internally generates a local oscillator (LO) signal at the target Rx frequency (let's consider zero-IF here). 
This LO signal is mixed with the signal received by the antenna, converting it to baseband for digital signal processing, demodulation, decoding, and other operations.

Because RF signals can easily leak through electronic circuits, the LO signal can travel backward from the receiver circuitry to the antenna and radiate into the air.

A radio monitoring system can detect this leaked LO signal from a distance, revealing the presence and potentially the location of the receiver. Some radios periodically turn their receivers on and off, 
for example in TDD or TDMA systems. By monitoring when the LO signal appears and disappears, it is possible to obtain information about the communication scheme and its frame structure.

End of the introduction.

I again used the high-speed AD9361 frequency-scanning system developed in my previous article: "Measure Starlink Channel Switching: 10,000-Hop/s Fast-Frequency Measurements by AD9361".

This time, I configured it to scan the frequencies of the eight Starlink downlink channels, allowing me to observe whether the terminal produces LO leakage or other radiation on these downlink frequencies during cold start.

The measurement revealed some interesting Rx behavior.

![](../media/cold-start-td-rssi-dl-2026-09-06_23-59-05-306909.png)

At around 24 seconds after cold start ("Tx scan" in the above figure), we see our familiar behavior from the previous article: the 8-channel uplink scanning operation. (post: "Starlink Terminal Cold-Start RF Emission Behavior Fingerprinting")

After that, the receive-chain LO appears to turn on continuously while scanning the downlink channels in the following order:

Channel 3 → 4 → 5 → 7 → 6 → 8 → 1 → 2 → 3 → 4 → ...

This continues until the first communication event with a satellite: initial_network_entry in the gRPC logs.

After this event, the continuous Rx LO scanning appears to be interrupted from time to time by communication activity, although the scanning pattern can still be observed (almost).

Why do I believe that the signal clusters at around 24 seconds are the 8-channel uplink scanning operation?

When zoomed in, their timing characteristics are exactly the same as those observed in the previous article (post: "Starlink Terminal Cold-Start RF Emission Behavior Fingerprinting"): each channel scan contains five signal clusters.

![](../media/cold-start-td-rssi-dl-zoom1-2026-09-06_23-59-05-306909.png)

There is also a new observation compared with the previous experiment on uplink: a clear "warm-up" signal appears before the uplink 8-channel scan begins.

But wait, why can we observe uplink transmission on the downlink receive frequencies?

Possible explanations include:

- Because my receiver is very close to the terminal, we may be capturing out-of-band radiation from the transmitter.
- The RF front end (LNB) and AD9361 may not provide sufficient out-of-band rejection.
- Other mixing or frequency-aliasing effects may also be involved.

The Starlink terminal operates in TDD mode (though the system is FDD and has different uplink downlink frequencies), 
so even if some uplink transmission leaks into the downlink receive path, this does not necessarily cause a problem because transmission and reception do not occur simultaneously.

Then at around 31 seconds, starting from receive channel 3, we begin to observe continuous Rx LO activity sequentially on each receive channel.

My current hypothesis, with 89.3% confidence, is that the receiver is performing a blind scan of the downlink frequencies to acquire network broadcast information? This may be similar to how an LTE receiver scans 
for and acquires broadcast information, such as the PBCH.

In my captures, each channel remains active for approximately 8.254 seconds, then switches seamlessly to the next channel.

![](../media/cold-start-td-rssi-dl-crop-2026-09-06_23-59-05-306909.png)

Here is another measurement from a different cold start. The conclusion remains unchanged.

![](../media/cold-start-td-rssi-dl-2026-09-07_00-33-36-480205.png)

After repeated measurements, the same behavior was consistently observed.

In the previous article ("Starlink Terminal Cold-Start RF Emission Behavior Fingerprinting"), I observed that after a cold start, the Starlink terminal begins a continuous scan across the eight uplink frequencies at a fixed time.

This is most likely an Tx RF and phased-array calibration procedure.

For example, the terminal may calibrate transmit power to ensure that the output power and EIRP remain within local regulatory limits.

I suspect that the Starlink terminal also contains monitoring and calibration Rx channel for its Tx chains, similar to designs used in smartphones and other RF chips (such as Analog Devices Inc.).

In addition, CFR (Crest Factor Reduction) and DPD (Digital Predistortion) are important for OFDM transmitters and also require feedback and monitoring Rx paths.

In general, it seems unlikely that the terminal's uplink-frequency transmission is directly used to calibrate the receive chain because the receive chain operates at downlink frequencies.

Furthermore, some receiver calibration algorithms based on statistical and higher-order statistical properties can be performed using only the thermal noise already present in the circuitry and antenna.

So here is the interesting question:

Could the out-of-band radiation and spurious emissions from the terminal's uplink transmission—or even intentional leakage—be used to calibrate the receive chain?

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
