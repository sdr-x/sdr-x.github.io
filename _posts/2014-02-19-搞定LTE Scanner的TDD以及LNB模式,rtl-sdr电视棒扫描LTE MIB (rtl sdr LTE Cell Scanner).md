---
date: 2014-02-19 12:00:00
layout: post
title: 搞定LTE Scanner的TDD以及LNB模式,rtl-sdr电视棒扫描LTE MIB (rtl sdr LTE Cell Scanner)
thread: 956
categories: LTE
tags:  LTE rtl-sdr SDR 4G Cell-Search Cell-Scanner Beijing MMDS-LNB LNB
---

(原文刊于被sina关闭的我的sina博客)

首先关于LTE频率，网上都有，比如这里： [http://labs.chinamobile.com/news/101634_p2](http://labs.chinamobile.com/news/101634_p2)

简单总结就是：

-------TD-LTE：

中国移动 1880-1900MHz、2320-2370MHz、2575-2635MHz

中国联通 2300-2320MHz、2555-2575MHz

中国电信 2370-2390MHz、2635-2655MHz

-------FDD LTE：

中国移动 无 (据说将来会有？)

中国联通 1955-1980MHz（上行）/ 2145-2170MHz（下行）

中国电信 1755-1785MHZ（上行）/ 1850-1880MHz（下行）

最近终于搞定TD-LTE解调（原软件只支持FDD LTE），在立水桥和雍和宫室内粗扫了一下，扫到12个LTE小区信息，其中两个为电信的FDD LTE，其余全是三家的TD-LTE信号，见附表。可以确认之前扫到的20MHz的频谱的确都是LTE的。
之前频谱扫描见：[4G LTE(北京)信号捕捉(rtl-sdr 电视棒软件无线电)新扫到电信(LTE SDR Beijing)](http://sdr-x.github.io/4G%20LTE%28%E5%8C%97%E4%BA%AC%29%E4%BF%A1%E5%8F%B7%E6%8D%95%E6%8D%89%28rtl-sdr%20%E7%94%B5%E8%A7%86%E6%A3%92%E8%BD%AF%E4%BB%B6%E6%97%A0%E7%BA%BF%E7%94%B5%29%E6%96%B0%E6%89%AB%E5%88%B0%E7%94%B5%E4%BF%A1%28LTE%20SDR%20Beijing%29/)
 和 [土法again LTE D频段(2500~2690MHz)三大运营商TD-LTE频谱全抓(TD-LTE Beijing capture scan)](http://sdr-x.github.io/%E5%9C%9F%E6%B3%95again%20LTE%20D%E9%A2%91%E6%AE%B5%282500~2690MHz%29%E4%B8%89%E5%A4%A7%E8%BF%90%E8%90%A5%E5%95%86TD-LTE%E9%A2%91%E8%B0%B1%E5%85%A8%E6%8A%93%28TD-LTE%20Beijing%20capture%20scan%29/)

![](../media/initial-cells-get-in-beijing.png)

(最新更多的Cell信息参见这篇：[LTE Cell detected in Beijing China](http://sdr-x.github.io/China-Beijing-LTE-Cell-List/))

细说一下：

前段时间这个[4G LTE(北京)信号捕捉(rtl-sdr 电视棒软件无线电)新扫到电信(LTE SDR Beijing)](http://sdr-x.github.io/4G%20LTE%28%E5%8C%97%E4%BA%AC%29%E4%BF%A1%E5%8F%B7%E6%8D%95%E6%8D%89%28rtl-sdr%20%E7%94%B5%E8%A7%86%E6%A3%92%E8%BD%AF%E4%BB%B6%E6%97%A0%E7%BA%BF%E7%94%B5%29%E6%96%B0%E6%89%AB%E5%88%B0%E7%94%B5%E4%BF%A1%28LTE%20SDR%20Beijing%29/)
 和 [土法again LTE D频段(2500~2690MHz)三大运营商TD-LTE频谱全抓(TD-LTE Beijing capture scan)](http://sdr-x.github.io/%E5%9C%9F%E6%B3%95again%20LTE%20D%E9%A2%91%E6%AE%B5%282500~2690MHz%29%E4%B8%89%E5%A4%A7%E8%BF%90%E8%90%A5%E5%95%86TD-LTE%E9%A2%91%E8%B0%B1%E5%85%A8%E6%8A%93%28TD-LTE%20Beijing%20capture%20scan%29/)
主要是频谱扫描，扫描到1860,1880，2565, 2585, 2605, 2645MHz中心频率有疑似20MHz带宽LTE频谱，但仅成功解调1860的电信FDD-LTE的小区MIB信息。

原因是这个LTE-Cell-Scanner仅支持FDD模式：

[https://github.com/Evrytania/LTE-Cell-Scanner](https://github.com/Evrytania/LTE-Cell-Scanner)

鉴于TD-LTE和FDD的LTE差别有限，于是我给上面的软件加入了TDD支持，链接如下：

[https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)

（因为里面有大量空中抓取的信号样本，所以初次clone较慢，之后增量pull就会比较快了。） 

终于能解调1890的中国移动的TD-LTE了。

但是对于用了MMDS-LNB的2500~2700MHz信号还是不能解调。因为LTE-Cell-Scanner的算法中假设了载波频偏和采样频偏有严格的解析关系，即下变频本振和A/D采样钟同源（电视棒本身的确如此），但用了外置的MMDS-LNB之后，这个解析关系就不存在了，因为外置LNB本振和电视棒不相干。

于是我写了一个前置的采样频偏补偿模块，先于原来的LTE-Cell-Scanner程序调用，并且简单的把原来的程序改为仅对付载波频偏（使它认为无采样频偏，因为前面补偿过了），于是也能检测2500~2700MHz里的LTE信号了。代码在此：

[https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)

做了一个简单的demo记录视频：

youku： [http://v.youku.com/v_show/id_XNjc1MjIzMDEy.html](http://v.youku.com/v_show/id_XNjc1MjIzMDEy.html)

youtube：[http://www.youtube.com/watch?v=4zRLgxzn4Pc](http://www.youtube.com/watch?v=4zRLgxzn4Pc)

新浪视频直接给我审核不通过。。。。


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
