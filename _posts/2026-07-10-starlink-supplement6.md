---
date: 2026-07-10 00:01:00
layout: post
title: "Starlink Analysis – Supplement(6):The Effect of Relativity on OFDM Clock Errors in LEO Communications"
thread: 2026071001
categories: starlink
tags: OFDM Starlink modem burst radio-frame OFDM pilot-cluster pilot CFO SFO doppler Relativity
---

I continued reading the Starlink patent US12003350 and came across the following statement:

A small change in clock rate is also caused by special (motion) and general (gravity) relativistic effects but it is quite small in low Earth orbits (about 0.2 ppb or parts per billion).

This is the relativistic effect considered by Starlink when analyzing synchronization error sources in an OFDM system.

The conclusion is that the clock-rate difference caused by relativistic time dilation is much smaller than other synchronization error sources, such as Doppler shift and sampling-time drift due to satellite motion, crystal oscillator manufacturing tolerance, and temperature drift. Therefore, it can be safely neglected.

Perhaps I simply have not read enough papers, but this is the first time I have seen relativistic effects explicitly discussed in the context of OFDM synchronization algorithms. （of course I know that GNSS does consider it)

I do wonder whether, in deep-space communication scenarios—such as solar probes or Mars missions—the clock errors introduced by relativity become significant enough to require explicit compensation for communication synchronization. 

Although I have not yet finished reading the patent, I now strongly feel that this is not merely a patent. It is a comprehensive treatise on modern OFDM communication engineering, covering fundamental principles, system design, and implementation details with remarkable completeness. Anyone who thoroughly understands every section of this patent would gain an excellent foundation in communication theory, system design, and engineering practice.

The first author of the patent, Martin McCormick, once remarked with characteristic humility:

“I’ve earned the somewhat dubious distinction of holding more patents than anyone else working for Elon Musk. I’ve invented about 10% of SpaceX’s entire patent portfolio.”

After reading this patent, I understand that statement very differently. He was not simply writing patent applications—he was writing what is effectively a technical textbook. There is nothing “dubious” about that achievement. The breadth and depth of his expertise are truly remarkable.

It is also said that he was among the last students of Alan V. Oppenheim. If so, his work certainly lives up to that reputation.

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

