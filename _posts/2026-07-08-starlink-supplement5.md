---
date: 2026-07-08 00:01:00
layout: post
title: "Starlink Analysis – Supplement(5):Pilot Design for LEO Broadband Access Compared with Wi-Fi and Cellular Systems"
thread: 2026070801
categories: starlink
tags: OFDM Starlink modem burst radio-frame OFDM pilot-cluster pilot CFO SFO doppler LTF Wi-Fi LTE 4G 5G
--

Analysis of measured Starlink uplink signals and the Starlink patent US12003350 shows that the pilot structure is specifically designed for LEO satellite broadband access. Compared with terrestrial cellular systems and Wi-Fi, the similarities and differences are as follows.

The most distinctive feature of the Starlink pilot design is the pilot structure embedded within data OFDM symbols. It uses block-based pilot clusters. Two pilot clusters are placed at the two edges of the allocated frequency band, and each cluster contains eight consecutive pilot subcarriers. In contrast, Wi-Fi and cellular systems use distributed pilot subcarriers scattered across the frequency band.

A Starlink packet (burst) begins with an OFDM symbol consisting entirely of pilot subcarriers, similar to the Long Training Field (LTF) in Wi-Fi. This is followed by pilots embedded within data OFDM symbols for tracking. Wi-Fi follows a similar approach.

At the burst level, Starlink is architecturally closer to Wi-Fi. Cellular systems are closer to a continuous streaming structure, where pilots are periodically distributed across the entire time-frequency grid. However, Starlink also defines a periodic radio frame structure above the burst level. In this sense, Starlink combines concepts from both Wi-Fi and cellular systems, taking advantage of each.

The use of two pilot clusters instead of distributed pilots reflects the different situations faced by different systems.

The phased-array antennas on both the satellite and the user terminal provide strong directivity, suppressing multipath propagation. As a result, the channel exhibits relatively weak frequency selectivity. However, because of the high velocity of LEO satellites and the higher carrier frequencies, the channel exhibits much stronger time variations.

Block-based pilot clusters make it convenient to estimate inter-carrier interference (ICI). The ICI pattern resembles an FIR filter response. Once the ICI coefficients are estimated, equalization across multiple adjacent subcarriers can suppress ICI. The main sources of ICI include:

* Residual carrier frequency offset (CFO)
* Residual Doppler components from multipath propagation
* The high-frequency components of phase noise, which become more significant at higher carrier frequencies

Block-based pilots also allow averaging across adjacent pilot subcarriers to improve the estimation accuracy of Common Phase Error (CPE), resulting in better tracking of the low-frequency components of phase noise. This averaging is effective because adjacent subcarriers experience similar channel responses, especially when frequency selectivity is weak.

Finally, placing the two pilot clusters at the edges of the allocated frequency band makes them more sensitive to the subcarrier-dependent phase rotation caused by Sampling Frequency Offset (SFO), which accumulates from one OFDM symbol to the next. This improves SFO tracking and compensation. SFO includes both the intrinsic frequency error of the sampling clock and sampling-time drift caused by the motion of the LEO satellite.

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

