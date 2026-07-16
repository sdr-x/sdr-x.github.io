---
date: 2026-07-16 08:01:00
layout: post
title: "Starlink Analysis – Supplement(12):Done Is Better Than Perfect — The Missing MIMO"
thread: 2026071621
categories: starlink
tags: OFDM Starlink LTE MIMO MU-MIMO Wi-Fi6 beam phased-array DBF Digital-Beamformin LoS Near-Field-MIMO
---

After reading the core Starlink patent US12003350, my strongest impression is this:

From a communications perspective, Starlink largely adopts technology comparable to LTE from about 18 years ago.

Starlink can be viewed as a simplified SISO LTE system or a SISO Wi-Fi 6 system, with carrier aggregation,.

Although it incorporates onboard processing, digital beamforming, and phased-array antennas, these are less communication-standard-specific technologies. They are enabling technologies for efficiently constructing multiple beams (cells) and multiple SISO links.

The LTE standard was largely defined by 2008.

Even in 2026, Starlink still lacks one of LTE’s key features: MIMO. The patent contains no discussion of spatial multiplexing or multiple spatial data streams.

Does this mean the Starlink engineers were unfamiliar with advanced communication techniques?

Obviously not.

The patent was filed in 2021, thirteen years after LTE had already become well established in 2008.

Possible reasons include:

* Under a line-of-sight (LoS) channel model, long-distance communication between a single satellite and a single terminal is dominated by far-field plane-wave propagation, rather than near-field spherical-wave propagation. As a result, it is difficult to construct a MIMO channel matrix with rank greater than one.
* Although multi-user MIMO (MU-MIMO) could be implemented using multiple satellites serving multiple terminals, the high orbital velocity and varying geometry of multiple LEO satellites make MU-MIMO considerably more challenging.

In the future, Starlink may gradually introduce technologies such as MU-MIMO, Inter-Cell Interference Coordination (ICIC), cell-free architectures, and inter-satellite coordination to improve spatial reuse and system capacity without increasing spectrum or satellite count.

Starlink may also introduce multiple spatial data streams at the terminal, for example through simultaneous connections to multiple polarizations or multiple satellites. This could further improve user experience and price premium.

The current burst structure already appears potential with future MIMO expansion.

The patent allows multiple channel estimation (CE) symbols, similar to the multiple LTFs used in Wi-Fi (for MIMO). Multiple CE symbols can naturally support channel estimation for multiple spatial streams.

Maybe, Some of these capabilities may already be appearing through Starlink’s frequent over-the-air (OTA) updates.

By continuously monitoring Starlink uplink and downlink signals, future changes of this kind may be observed.

Technology is neither “advanced” nor “outdated” by itself. Its value depends on how well it matches the application.

The essence of engineering is trade-offs.

Done is better than perfect.

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

