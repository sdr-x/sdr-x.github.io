---
date: 2026-09-26 00:01:00
layout: post
title: "The Mysterious Leakage During a Starlink Terminal Deep Cold Start"
thread: 2026092695
categories: starlink
tags:  Starlink Cold-Start Leakage Lo-Leakage Uplink Signal Finger-Print Software-Defined-Radio SDR FPGA AD9361
---

Normal cold start: Power on the Starlink terminal shortly after power off previously.

Deep cold start: Power on the Starlink terminal after it has been powered off for several days.

New finding: During a deep cold start, in addition to the 8-channel scanning transmission we previously observed around 24 seconds after the Starlink terminal starts up (https://sdr-x.github.io/starlink-UL-cold-start/), another mysterious, drifting signal can also be observed. 
This signal can't be observed during the normal cold start.

![](../media/starlink-deep-cold-start-ul-ch3-leak-1.png)

It starts at a relatively low frequency, gradually becomes stronger, and moves toward higher frequencies.

![](../media/starlink-deep-cold-start-ul-ch3-leak-2.png)

It finally disappears with a rapid "swoosh" at an even higher frequency.

The center frequency of 1356.25 MHz shown in the screenshot is the center frequency of Starlink uplink channel 3: 14 + 0.03125 + 2 × 0.0625 GHz = 14.15625 GHz. 
after downconversion by a Ku-band LNB with a 12.8 GHz local oscillator.

The phenomenon has been verified with two different SDRs, which largely rules out the possibility that it is caused by the SDR hardware. It has also been observed in two deep cold-start experiments.

Below are more screenshots and videos (). They show that the starting and ending frequencies of this drifting signal can appear at either lower or higher than the center frequency.

![](../media/starlink-deep-cold-start-ul-ch3-leak-3.png)

![](../media/starlink-deep-cold-start-ul-ch3-leak-4.png)

![](../media/starlink-deep-cold-start-ul-ch3-leak-5.png)

![](../media/starlink-deep-cold-start-ul-ch3-leak-6.png)

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
