---
date: 2014-05-29 12:00:00
layout: post
title: ADS-B out by HACKRF and received over the air by rtl-sdr dongle and dump1090
thread: 564371
categories: ads-b
tags:  ADS-B ADSB-OUT HACKRF SDR Software-Defined-Radio Gnu-Radio GRC rtl-sdr dump1090
---

(And, see also some works on [GPS relplay](https://goo.gl/cu7uWf)

Hi,

I did a simple experiment on ADS-B. See attached picture for principle. 

![](../media/hackrf-adsb-rtl-sdr-dump1090.png)

And video here: [http://youtu.be/xpLDqBkUiKc](http://youtu.be/xpLDqBkUiKc)

In China, see here: [http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html](http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html)

You can get GRC for replay here: [https://github.com/JiaoXianjun/GNSS-GPS-SDR/tree/master/adsb](https://github.com/JiaoXianjun/GNSS-GPS-SDR/tree/master/adsb)

**But I need your attention!!! if you want do some experiments.**

**ATTENTION!!!**

Transmit at 1090MHz is dangerous if you are not authorized!

Do not transmit at 1090MHz!

Select legal frequency and legal/low-enough power to transmit very locally for example in a basement or chamber/room via a close-loop RF cable.

Make sure you are not interfering any other systems! 

Have fun with safety.


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


<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-56112029-1', 'auto');
  ga('send', 'pageview');

</script>
