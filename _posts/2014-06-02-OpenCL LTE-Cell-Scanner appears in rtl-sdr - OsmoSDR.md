---
date: 2014-06-02 12:00:00
layout: post
title: OpenCL LTE-Cell-Scanner appears in rtl-sdr - OsmoSDR
thread: 35074
categories: lte
tags:  LTE TD-LTE SDR 4G Beijing Cell-Search Cell-Scanner rtl2832 rtl-sdr OsmoSDR dongle LTE-Cell-Scanner OpenCL
---

(Now you can find ["LTE-Cell-Scanner OpenCL accelerated (new)"](https://github.com/JiaoXianjun/LTE-Cell-Scanner) in this page: [http://sdr.osmocom.org/trac/wiki/rtl-sdr](http://sdr.osmocom.org/trac/wiki/rtl-sdr)

And see latest progress: [Whole 20MHz config LTE signal is decoded by HACKRF 19.2Msps with ASN1 SIB parsed](http://sdr-x.github.io/Whole%2020MHz%20config%20LTE%20signal%20is%20decoded%20by%20HACKRF%2019.2Msps%20with%20ASN1%20SIB%20parsed/)

Here is my application.

Hi Administrator,

May I apply that if you can add my improved LTE Cell Scanner ([https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)) to the list of APPs at [http://sdr.osmocom.org/trac/wiki/rtl-sdr](http://sdr.osmocom.org/trac/wiki/rtl-sdr)

You may have found track of improvement in this mail-list. The main new features are:

1. TD-LTE support; (Original one only support FDD mode Cell)

2. external LNB support. Just like the method of "NRF24-BTLE-Decoder", external LNB can be added to extend band of rtl-sdr because most of TD-LTE frequencies here are above 2GHz. But external LNB breaks strict relationship of carrier-timing offset which is a basic assumption in the algorithm of the original one. Algorithm is improved to support this feature which make it work in many TDD bands locally.

3. OpenCL acceleration which accelerates speed of Cell Scanner much.

4. Capture and reload rtl-sdr raw uint8 bin file

5. Multiple tries at one frequency point to increase detection probability.

6. manual gain mode

7. ...

So many changes have been made, I think it is hard or heavy time cost for James Peroulas 
([https://github.com/Evrytania/LTE-Cell-Scanner](https://github.com/Evrytania/LTE-Cell-Scanner))
 to review and merge back. 
 
 TDD is an important feature for Chinese users. 
 Most released system here are TDD, FDD even hasn't been released officially here. 
 
 So I think it would be good to mention this improved Cell Scanner somewhere, 
 and it would be useful and interesting for people who wants to scan TD-LTE.
 
( Now I have decoded [LTE RRC SIB message in DL-SCH successfully](http://sdr-x.github.io/Whole%2020MHz%20config%20LTE%20signal%20is%20decoded%20by%20HACKRF%2019.2Msps%20with%20ASN1%20SIB%20parsed/) in the same repo by HACKRF. 
Some time in the future maybe SIB decoding will be made also for rtl-sdr dongle, 
because I found that some SIBs are narrow band also (6RB, similar/like PBCH) and located in fixed frequency)

BR

Jiao Xianjun


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
