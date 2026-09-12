---
date: 2026-09-12 04:05:00
layout: post
title: "With Satellites Blocked, the Starlink Terminal Keeps Searching for the Network and Reboots Periodically"
thread: 2026091275
categories: starlink
tags:  Starlink Terminal Cold-Start Lo-Leakage Satellite Self-Calibration Phased-Array Scanning LNB SDR AD9361 FPGA Ku-band Cell-Search Mobile-Network
---

In the previous post, I observed what appears to be a regular downlink channel-scanning behavior after a Starlink terminal cold start. 
The terminal sequentially scans all 8 downlink channels (strictly dwell ~8.26 seconds/channel). However, this behavior becomes irregular and difficult to identify after establishing a satellite connection.

What happens if we block the Starlink satellite signal so that the terminal cannot receive any satellite signal? Will it continue scanning the 8 channels in sequence?

Experiments show that the answer is yes.

![](../media/cold-start-td-rssi-dl-2026-09-11_19-50-29-404533-label.png)

Continuous RF measurements for 1.5 hours clearly show that, when the satellite signal is blocked, the terminal continues searching across the 8 downlink channels, switching channels strictly every ~8.26 seconds. 
This behavior remains clearly visible until the terminal reboots after ~1834 seconds.

After rebooting, we clearly observe again the 8 uplink channels being transmitted sequentially, as seen in the previous article. This is likely related to RF calibration. 
After that, the terminal stops transmitting on the uplink and only continues scanning the 8 downlink channels in sequence. It then reboots again after another 1834 seconds.

This is consistent with our previous analysis and also with the design principles of most cellular communication networks: a terminal should not transmit blindly before acquiring system broadcast information.

Because the reboot interval is highly consistent, it is likely a fixed long-timeout mechanism in the terminal firmware that triggers when no Starlink satellite can be found for an extended period.

During the ~8.26 second dwell time on each channel, how many directions in the sky does the phased array scan on the same channel? This requires further investigation.

Below is a waterfall plot taken near the end of the 1.5-hour measurement (5400 seconds). The sequential switching across all 8 downlink frequencies remains unchanged and clear.

![](../media/cold-start-spectrogram-dl-zoom-in-2026-09-11_19-50-29-404533.png)

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
