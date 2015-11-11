---
date: 2015-11-03 12:00:00
layout: post
title: BTLE packet sniffer based on HACKRF (function and performance similar to TI's packet sniffer)
thread: 2015110310
categories: btle
tags:  HACKRF DFU firmware 1602 LCD UART
---

Nov. 2015: So excited that the HACKRF BTLE packet sniffer/scanner can follow hopping data channels automatically now! Just like TI's sniffer.

![](../media/cap-freq-hopping.png)

New added options:

-o --hop

This will turn on data channel tracking (frequency hopping) after link setup information is captured in ADV_CONNECT_REQ packet.

-f --freq_hz

This frequency (Hz) will override channel setting (In case someone want to work on freq other than BTLE. More general purpose).

-m --access_mask

========================================

This is extended from my [btle_tx project](http://sdr-x.github.io/A%20BTLE%20%28Bluetooth%20Low%20energy%29%20BT4.0%20radio%20packet%20sender%20%28BladeRF,%20HACKRF%29/)

Now all BTLE channels (0~39, bothe ADV and DATA channels) are supported. You can use btle_tx and btle_rx to send or sniff on any BTLE channel.

A Raw mode is added to both btle_tx and btle_rx. Under this mode, after access addr is detected, following raw 42 bytes (without descrambling, parsing) are printed out. By this way, you can do other experiments or communication between HACKRF boards easily.

[Introduction source code and usage](https://github.com/JiaoXianjun/BTLE)

[youtube video](https://youtu.be/9LDPhOF2yyw)

[video in China](https://vimeo.com/144574631)

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
