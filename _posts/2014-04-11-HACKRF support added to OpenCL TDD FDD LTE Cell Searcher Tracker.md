---
date: 2014-04-11 12:00:00
layout: post
title: HACKRF support added to OpenCL TDD FDD LTE Cell Searcher Tracker
thread: 2014864
categories: lte
tags:  LTE TD-LTE SDR 4G Beijing Cell-Search Cell-Scanner HACKRF
---

(And see latest progress: [Whole 20MHz config LTE signal is decoded by HACKRF 19.2Msps with ASN1 SIB parsed](http://sdr-x.github.io/Whole%2020MHz%20config%20LTE%20signal%20is%20decoded%20by%20HACKRF%2019.2Msps%20with%20ASN1%20SIB%20parsed/))

OpenCL LTE Cell Searcher Tracker ([https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)) supports HACKRF board now!

Thank you scateu that [http://hackrf.net/](http://hackrf.net/) offering me the demo board for debugging purpose!

With HACKRF, decoding LTE SIB informaton becomes possible in the future, 
because HACKRF has 20MHz bandwidth which is much higher than rtl-sdr dongle (20MHz LTE RRC SIB in DL-SCH is parsed actually now: [1](http://sdr-x.github.io/Whole%2020MHz%20config%20LTE%20signal%20is%20decoded%20by%20HACKRF%2019.2Msps%20with%20ASN1%20SIB%20parsed/), [2](http://sdr-x.github.io/LTE-SIB-decoding-by-asn1c/) ). 
(Now the program can only decode LTE MIB information because it was designed for rtl-sdr dongle).

See a demo vide here:

(outside China) [http://www.youtube.com/watch?v=3hnlrYtjI-4](http://www.youtube.com/watch?v=3hnlrYtjI-4)

(inside China) [http://v.youku.com/v_show/id_XNjk3Mjc1MTUy.html](http://v.youku.com/v_show/id_XNjk3Mjc1MTUy.html)

The HACKRF board seemingly has a much higher sensitivity than rtl-sdr dongle after I tune gains of LNA and VGA.

Yes I saw a little bit DC offset in IQ stream. But it shouldn't matter much to LTE, because LTE doesn't use DC sub-carrier.

I haven't think about this issue for general purpose. But DC offset is popular in many system, and there must have been mature algorithm to combat with it.

PS, I found two advantages of hackrf over rtl-sdr E4K dongle:

1. higher sensitivity.

2. lower power consumption. 
(For Thinkpad T410, I have to use the laptop dock to ensure enough power supply to the E4K dongle. 
If the dongle is plugged to T410 directly, it would be very unstable. Sometimes driver crashed, 
sometimes computer lost responses... While hackrf can be used freely with my T410 every where without dock, 
even without AC adapter).

HACKRF, Good board!

I haven't measured it. 820t tuner also causes that issue if I recall correctly.

During that period, I searched Internet and found someone mentioned possibility of power supply. 
It did work after I used dock. Amazing.
These two dongles always work fine in my desktop computer. This reflects the issue indirectly, 
because desktop has enough port power, doesn't chase low power usually.

Maybe it is also related to kind of "startup current" things (instead of total power consumption)? Just a guess.


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
