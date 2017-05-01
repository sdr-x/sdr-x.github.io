---
date: 2017-05-01 12:00:00
layout: post
title: 	Study on faking GPS signal for iPhone 6 iPhone 6s iPhone SE iPhone 7
thread: 20170501
categories: gps
tags:  BladeRF GPS replay SDR Software-Defined-Radio bladeRF-cli osqzss gps-sdr-sim
---

I created a simple project [poke-move](https://github.com/JiaoXianjun/poke-move) to generate fake GPS signal 
via BladeRF playing Pokemon Go on predefined geography trace (You just sit in room with your iPhone while playing).

The script works quite well for iPhone 6, 6s, SE, but not for my iPhone 7 (My iPhone 7 has Intel baseband).

Why it doesn't work for Intel baseband iPhone 7? I don't have conclusion yet, but some guess: Maybe it is 
related to different iPhone GPS solution? (Broadcomm GPS performs better than Qualcomm GPS in aspect of anti-fake-GPS?)

GPS information of iPhone from internet:

iPhone 6: [teardown](https://www.ifixit.com/Teardown/iPhone+6+Teardown/29213): Baseband chip [MDM9625](https://www.qualcomm.com/news/releases/2013/02/25/qualcomm-technologies-announces-first-4g-lte-advanced-embedded-connectivity) includes GPS

iPhone 6s: [teardown](https://www.ifixit.com/Teardown/iPhone+6s+Teardown/48170): Transceiver chip [WTR3925](http://www.chipworks.com/sites/default/files/Apple_iPhone_6s_A1688_Smartphone_Chipworks_Teardown_Report_BPT-1509-801_with_Commentary.pdf) includes GPS

iPhone SE: [teardown](https://www.ifixit.com/Teardown/iPhone+SE+Teardown/60902): Baseband chip [MDM9625](https://www.qualcomm.com/news/releases/2013/02/25/qualcomm-technologies-announces-first-4g-lte-advanced-embedded-connectivity) includes GPS

iPhone 7: [teardown: Intel baseband](http://www.techinsights.com/about-techinsights/overview/blog/apple-iphone-7-teardown/): GPS chip [BCM47734](https://www.broadcom.com/products/wireless/gnss-gps-socs/bcm4774/)

iPhone 7: [teardown: Qualcomm baseband](https://www.ifixit.com/Teardown/iPhone+7+Teardown/67382): Transceiver chip [WTR3925](http://www.chipworks.com/sites/default/files/Apple_iPhone_6s_A1688_Smartphone_Chipworks_Teardown_Report_BPT-1509-801_with_Commentary.pdf) includes GPS

Now question is: Will Qualcomm baseband iPhone 7 be attracted by fake GPS signal as easy as iPhone 6/6s/SE or not?

Anyone has Qualcomm baseband iPhone 7?

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
