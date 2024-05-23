---
date: 2014-05-29 12:00:00
layout: post
title: 使用HACKRF巡视钓鱼岛(HACKRF ADS-B out)
thread: 676
categories: ads-b
tags:  ADS-B ADSB-OUT HACKRF SDR Software-Defined-Radio Gnu-Radio GRC rtl-sdr dump1090
---

(原文刊于被sina关闭的我的sina博客)

(这里还有一篇[GPS回放](http://sdr-x.github.io/%E4%B8%80%E4%BA%9B%E5%85%B3%E4%BA%8EGPS%E9%87%8D%E6%94%BE%E6%A8%A1%E6%8B%9F%E7%9A%84%E5%8A%AA%E5%8A%9B%E4%BB%A5%E5%8F%8A%E5%B0%8F%E5%B7%A5%E5%85%B7%E8%84%9A%E6%9C%AC/)的工作，若感兴趣可以看看)

好吧，承认我是标题党。其实就是用HACKRF发射ADS-B信号，因为技术含量偏低，只好标题博眼球了。。。
  
做了这么个简单的实验，原理如下图。

![](../media/hackrf-adsb-rtl-sdr-dump1090.png)

实验视频在此：[http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html](http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html) ）。

国外请访问：[http://youtu.be/xpLDqBkUiKc](http://youtu.be/xpLDqBkUiKc)

实现的功能是：

matlab按照ads-b协议生成信号，因为自己生成可以填入任意经纬度等信息，经HACKRF和天线发射，然后由rtl-sdr电视棒配合dump1090或者gr-air-modes接收。
  
生成信号的坐标序列是按照钓鱼岛周围一圈设置的，所以你在ADS-B接收界面中看到的就是一架飞机在以3000ft的高度巡视钓鱼岛。。。

**警告：**
  
不要真的用1090MHz实验！如果实验，你该找一个合法的或者足够安全的频率（比如某些敌国卫星的生癖频率），以最最最低的功率发射，使用闭路射频电缆和衰减器，最好在地下室或者暗室玩这个。

出了事后果自负。

不要问我源代码在哪里，不会公开。

其实发射相对接收简单许多，按照协议做就是了，对于熟手，依靠互联网公开的资料，半天到一天就能搞定。

用于回放信号的grc在这里： [https://github.com/JiaoXianjun/GNSS-GPS-SDR/tree/master/adsb](https://github.com/JiaoXianjun/GNSS-GPS-SDR/tree/master/adsb)


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
