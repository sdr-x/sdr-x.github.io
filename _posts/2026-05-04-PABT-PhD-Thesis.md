---
date: 2026-05-04 12:03:00
layout: post
title: 	My PhD Thesis PABT Pilot Assisted Block Transmission
thread: 2026050403
categories: misc
tags:  OFDM DVB-T DMB-T CMMB DTMB GB20600-2006 Pilot Synchronization Equalization FEC
---

My PhD thesis: [Pilot Assisted Block Transmission](../media/PABT-Pilot-Assisted-Block-Transmission.pdf)

English abstract:

Pilot Assisted Block Transmission (PABT)

JIAO Xianjun (Communication and Information system)

Directed by Prof. XIANG Haige

Abstract:

Pilot Assisted Block Transmission (PABT) is a transmission scheme suitable for
broadband wireless communication. In this scheme, data are grouped into separated
blocks, in which pilot are inserted. Receiver gets channel state information (CSI)
based on observation of received pilot in real time and makes demodulation according
to CSI. Considering the time variety and frequency selective characters of broadband
wireless channel in modern wireless broadcasting, mobile and wireless network
system, PABT is a reasonable choice. In this thesis, a uniform signal framework of
PABT is summarized based on investigations of existing broadband systems, and two
kinds of PABT system are presented under the framework: pilot and data are in time
division multiplexing form; pilot and data are in frequency division multiplexing form.
Researches on key technologies and systems of PABT are carried out. This thesis
include:

1. A uniform signal framework of PABT is summarized, and many signal formats
are presented under the framework. Those formats are classified into two kinds:
pilot and data are in time division multiplexing form; pilot and data are in
frequency division multiplexing form. In the first kind, there are four basic
formats: “PN (Pseudo Noise) pilot + SC (Single Carrier) data”, “PN pilot +
OFDM data”, “OFDM pilot + SC data”, “OFDM pilot + OFDM data”. The
second kind is referred to traditional OFDM systems with frequency domain
pilot in fact. In addition, with different form of guard interval, for example CP
(Cyclic Prefix), ZP (Zero Padding)…, more signal formats can be created. The
formats include not only most existing systems but also new systems. Key 
technologies of PABT are discussed under the framework, which are: Channel
Estimation (CE), Euqalization, Iterative Interference Canceling and guard
interval signal design for higher efficency.

2. All kinds of CE algorithm are studied and summarized. The procedure of CE is
separated into three steps: pre-process, channel estimate, post-process. Two
algorithms of estimating time domain channel impulse response (CIR) are
studied, which are move correlation and circular correlation. Two algorithms of
estimating channel frequency response are also studied, which are frequency
domain estimation after “cut-and-add” and zero padding frequency domain
estimation. A new CE algorithm based on PN pilot with circular correlation is
proposed. The new CE algorithm has almost the same performance with existing
algorithm, but the complexity of new algorithm is much lower than that of
existing algorithm. A new post-process algorithm is also proposed, which leads
to visible performance gain compared to CE without post-process. The idea of
new post-process algorithm is that certain number of strong path is reserved and
other paths are removed as noise. The performance of new post-process
algorithm has no “floor” phenomenon, which is a drawback of exsisting
algorithm.

3. Time domain equalization (TDE) and frequency domain equalization (FDE) of
CP/ZP block are summarized and studied. TDE and FDE are proved to be
equivalent. Two MMSE (Minimum Mean Square Error) FDE algorithms are
studied: direct FDE and FDE after “cut-and-add” pre-process. Quasi-MMSE
FDE alogrithms is studied. The complexity of quasi-MMSE algorithm is much
lower than that of strict-MMSE algorithm, and performance degradation is only
about 0.1dB, when 7, [171 133], 1/2 convolution code and random interleaver
are employed.

4. Iterative interference canceling algorithm is studied in the situation that pilot and data are in time division multiplexing form and without guard interval. A
simplified algorithm is proposed for situation where pilots are invariant. As
applications of iterative interference canceling, “OFDM pilot + SC data” and
“OFDM pilot + OFDM data” PABT system is studied. Simulation shows that
two systems are suitable for broadband wireless communication with high
mobility (moving speed 130km/h; doppler frequency 100Hz in 800MHz band).
When CIR is short, for example DVB-T portable reception channel, performance
of new systems can approach that of reference system with guard, which needn’t
interference canceling, and new systems have higher bit rate.

5. In the situation that pilot and data are in time division multiplexing form, a new
PP-OFDM (Pilot Postfixing - OFDM) scheme is proposed as a modification of
traditional CP-OFDM. Replacing CP signal with PP signal in guard interval
ensures that PP-OFDM has higher efficiency of utilizing pilot power than
CP-OFDM. ZP-OFDM can be derived from PP-OFDM. Pilot power allocation is
studied in PP-OFDM, and analytic result is derived. When 1/4 guard interval is
adopted, PP-OFDM has about 1dB performance gain compared with CP-OFDM.

Key words: 

PABT (Pilot Assisted Block Transmission); OFDM (Orthogonal
Frequency Division Multiplexing); Channel Estimation; Frequency Domain
Equalization; Iterative Interference Canceling

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
