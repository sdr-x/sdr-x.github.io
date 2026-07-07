---
date: 2026-07-07 17:01:00
layout: post
title: "Starlink Analysis – Supplement (4):Frame Structure"
thread: 2026070845
categories: starlink
tags:  OFDM Starlink modem burst radio-frame frame-structure channel-estimation
---

I continued reading the Starlink patent US12003350 and obtained the following information.

Each burst begins with a Unique Word (UW), followed by multi-user OFDM symbols.

For an individual user:

* The first OFDM symbol is used for channel estimation. It can be configured as one or multiple OFDM symbols. The content is generated from a Golay sequence.
* The remaining OFDM symbols carry payload. Each symbol consists of data subcarriers, pilot clusters (with a configurable number of pilot subcarriers), and configurable DC null subcarriers.

The pilot clusters are placed at the two edges of the user’s allocated frequency band. Their offsets from the band edges are configurable, and the placement is symmetric.

A radio frame may contain multiple bursts. Starting from the second burst, the UW is optional.

Each user may occupy multiple Resource Blocks (RBs), with multiple RBs mapped to contiguous subcarriers. Different users occupy non-overlapping frequency bands, beginning with the non-overlapping channel estimation subcarriers in frequency domain. 

A channel supports up to four users.

The UW is used for:

* Burst detection
* OFDM symbol alignment
* Power estimation
* Carrier frequency offset (CFO) estimation


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
