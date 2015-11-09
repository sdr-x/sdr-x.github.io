---
date: 2015-11-03 12:00:00
layout: post
title: BTLE packet sniffer based on HACKRF (function and performance similar to TI's packet sniffer)
thread: 2015110310
categories: btle
tags:  HACKRF DFU firmware 1602 LCD UART
---

Nov. 2015: So excited that the packet sniffer/scanner btle_rx can follow BTLE hopping data channels now! It follows ~8ms hopping link successfully! (Because the fastest speed of my btle_tx is about 8ms gap between successive packets). Currently it only works on a simple case (which is the most cases in real world) where channel mapping is 0x1FFFFFFFFF (This means all data channel 0~36 will be used for freq hopping). btle_rx is a fast moving target currently. More for demonstration purpose than full verification for now. Will release more soon.

New added options:

-f --freq_hz

This frequency (Hz) will override channel setting (In case someone want to work on freq other than BTLE. More general purpose).

-m --access_mask

If a bit is 1 in this mask, corresponding bit in access address will be taken into packet existing decision (In case someone want a shorter/sparser unique word to do packet detection. More general purpose).

-o --hop

This will turn on data channel tracking (frequency hopping) after link setup information is captured in ADV_CONNECT_REQ packet.

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
