---
date: 2014-11-02 12:00:00
layout: post
title: test rtlsdr hackRF and bladeRF in VMware virtual machine (虚拟机内使用软件无线电)
thread: 2014110223423
categories: SDR
tags:  LTE TD-LTE SDR Cell-Search Cell-Scanner rtlsdr hackRF bladeRF VMware Virtual-Machine
---

test some cases to see if rtlsdr hackRF and bladeRF can work in virtual machine smoothly.

rtlsdr dongle and bladeRF work well in virtual machine for this [LTE Cell Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner), 
while hackrf_info always reports:

    Found HackRF board.
    hackrf_board_id_read() failed: HACKRF_ERROR_LIBUSB (-1000)

Since rtlsdr and bladeRF work in virtual machina, there is a hope for hackrf to work also? Any ideas or suggestions?

My computer configuration:

    host OS: Win7
    guest OS: Ubuntu14 (LTS)
    virtual machine: VMware 10.0.4
    mainboard: ASUS P9X79 PRO -LGA 2011/Intel X79 /ATX
    CPU: i7 3930K -3.2GHz/LGA 2011/12M 120W
    memory: Kingston HyperX 16G(4GBx4) DDR3 1600 KHX1600C9D3K4/16GX
    graphic: GTX550TI 950MHz/4100MHz 1GB/192BIT DDR5 PCI-E

Hope above is useful for someone.


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
