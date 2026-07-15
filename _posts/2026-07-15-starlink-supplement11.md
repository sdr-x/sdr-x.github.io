---
date: 2026-07-15 00:01:00
layout: post
title: "Starlink Analysis – Supplement(11):Multiple Access, Duplexing, Beams, Gateway Stations, Phased-Array Interfaces, Subcarriers, Channel Model, and Random Access"
thread: 2026071501
categories: starlink
tags: OFDM Starlink beam phased-array DBF Digital-Beamforming multiple-access duplexing antenna channel-model
---

I continued reading the Starlink patent US12003350 and summarized several system-level details. The patent was filed in early 2021, so readers should consider its timeliness accordingly.

Multiple Access

Starlink uses a combination of OFDMA and TDMA for multiple access.

Duplex Mode

Although all Starlink links operate in Frequency Division Duplex (FDD), with separate uplink and downlink frequencies, the user terminal operates in half-duplex mode.

One possible reason is that half-duplex operation eliminates the need for separate transmit and receive antennas or a frequency duplexer. An RF switch may be sufficient, reducing terminal cost and size.

Satellites and gateway stations operate in full-duplex mode. Satellites use multiple phased-array antennas with separate transmit and receive apertures. When no gateway station is within coverage, a satellite can access a gateway through inter-satellite links.

Beams

The beamwidth of a Starlink satellite is approximately 2 degrees.

Gateway Stations

Communication between a satellite and a gateway station is always point-to-point. There are no scenarios in which multiple satellites communicate with the same gateway simultaneously, or multiple gateways communicate with the same satellite simultaneously.

This may explain why publicly available photos of Starlink gateway stations show many radomes (“mushrooms”). Each radome may communicate with only one satellite at a time.

The patent frequently mentions parabolic antennas when describing gateway stations. This suggests that the radomes may contain mechanically steered Ka-band parabolic antennas, rather than phased-array antennas.

One possible advantage is lower cost by using mature commercial Ka-band parabolic antenna technology. Wideband Ka-band phased arrays are likely much more expensive.

Phased-Array Interface

Parabolic antennas still play a role in the Starlink system, most likely at gateway stations.

This may also explain why the patent specifies that the modem can interface with two types of antennas:

* Parabolic antennas
* Digital Beamforming (DBF) antennas (phased-array antennas)

For parabolic antennas, the modem interface consists of conventional ADC/DAC connections, implying that the antenna includes the RF front-end.

For DBF antennas, the modem connects through a high-speed SerDes interface, indicating that the DBF module is a complex digital subsystem containing substantial digital signal processing.

Subcarriers

The patent states that Starlink OFDM uses a half-subcarrier offset (1/2-bin offset) to achieve spectrum symmetry.

This avoids having one fewer positive-frequency subcarrier than negative-frequency subcarriers.

The approach resembles the uplink DFT-s-OFDM implementation used in LTE and also helps reduce DC interference.

Channel Model

The patent explicitly assumes a predominantly line-of-sight (LOS) propagation environment.

As a result, the pilot clusters located at the two edges of a user’s allocated bandwidth can be used with simple frequency-domain linear interpolation to estimate propagation delay.

Accurate delay compensation is particularly important in satellite communications. The patent also provides extensive discussion of Doppler compensation, Sampling Frequency Offset (SFO), and Carrier Frequency Offset (CFO).

This is consistent with my previous analysis.

MAC Random Access

The patent also specifies the purpose of MAC-layer random access.

It is used for:

* Initial network access
* Bandwidth requests

This design is similar to the random access procedures used in LTE and other cellular systems.

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

