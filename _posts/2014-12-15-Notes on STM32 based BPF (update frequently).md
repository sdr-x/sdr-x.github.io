---
date: 2014-12-15 12:00:00
layout: post
title: Notes on STM32 based BPF (update frequently)
thread: 2014121523
categories: misc
tags:  STM32 BPF STM32F4DISCOVERY STM32F4-DISCOVERY
---

**0x00** All you need is a discovery board + a mini-usb cable to computer

**0x01** CN3 connected mode: computer -- USB cable -- onboard STLINK -- onboard STM MCU

**0x02** CN3 disconnected mode: computer --USB  cable -- onboard STLINK -- onboard CN2 -- offboard/other STM MCU

**0x03** CN3 disconnected mode is not for offboard USB-STLINK adapter programming discovery board via CN2! This mode turns discovery board to a BIG USB-STLINK adapter for you to develop your own or the third part STM MCU program!

**0x04** [Using the STM32F4DISCOVERY on the Ubuntu 13.04 Command Line](https://www.alexwhittemore.com/stm32f4discovery-on-ubuntu-command-line/)



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
