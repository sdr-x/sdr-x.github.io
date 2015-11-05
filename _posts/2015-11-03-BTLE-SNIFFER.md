---
date: 2015-11-03 12:00:00
layout: post
title: BTLE packet sniffer based on HACKRF (function and performance similar to TI's packet sniffer)
thread: 2015110310
categories: btle
tags:  HACKRF DFU firmware 1602 LCD UART
---

[Introduction source code and usage](https://github.com/JiaoXianjun/BTLE)

[Release notes](https://github.com/JiaoXianjun/BTLE/releases/tag/v0.1.1)

[youtube video](https://youtu.be/9LDPhOF2yyw)

[youku video in China](http://v.youku.com/v_show/id_XMTM3NzA0NjgyNA==.html)

Snapshots of HACKRF BTLE packet sniffer VS TI's packet sniffer under fastest flow of continuous/non-gap BTLE packets sequence to demonstrate full real-time processing ability. They capture the same amount of packets and contents:

![](../media/mine-btle-sniffer2.png)

![](../media/TI3.png)

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
