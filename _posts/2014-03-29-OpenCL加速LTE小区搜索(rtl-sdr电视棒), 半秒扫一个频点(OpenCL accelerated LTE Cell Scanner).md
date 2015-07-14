---
date: 2014-03-29 12:00:00
layout: post
title: OpenCL加速LTE小区搜索(rtl-sdr电视棒), 半秒扫一个频点(OpenCL accelerated LTE Cell Scanner)
thread: 893
categories: LTE
tags:  LTE TD-LTE SDR 4G Beijing Cell-Search Cell-Scanner OpenCL acceleration rtl-sdr
---

(原文刊于被sina关闭的我的sina博客)

视频链接： (国内) [http://pan.baidu.com/s/1o6qbLGY](http://pan.baidu.com/s/1o6qbLGY) ，（国外） 
[http://www.youtube.com/watch?v=SxZzEVEKuRs](http://www.youtube.com/watch?v=SxZzEVEKuRs)

开源地址： [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)

是这篇 
[“搞定LTE Scanner的TDD以及LNB模式,rtl-sdr电视棒扫描LTE MIB (rtl sdr LTE Cell Scanner)”](http://sdr-x.github.io/%E6%90%9E%E5%AE%9ALTE%20Scanner%E7%9A%84TDD%E4%BB%A5%E5%8F%8ALNB%E6%A8%A1%E5%BC%8F,rtl-sdr%E7%94%B5%E8%A7%86%E6%A3%92%E6%89%AB%E6%8F%8FLTE%20MIB%20%28rtl%20sdr%20LTE%20Cell%20Scanner%29/)
的延续。 
  
改进： 

1. 不用Matlab，而是全C/C++了。现在你需要只是一个电视棒和一个装了Linux的电脑。TDD FDD全模式。支持外部LNB、变频器（即支持变频器时钟和A/D采样时钟不相干） 
  
2. 加入OpenCL并行计算支持，用台式机i7-3930K 6 cores@3.2GHz，可以做到半秒扫一个频点，比原来大约快22倍（算法简化大约3.4倍，OpenCL大约6.5倍）！用Thinkpad T410 i5 CPU M520@2.40GHz大概2～3秒扫一个小区。 
  
3. 其他一些算法方面的改进，更灵敏和鲁棒一些。 
  
OpenCL SDK使用Intel，AMD，Nvidia都测过了。加速数值是用Intel SDK得到的。Nvidia GPU加速没这么夸张，和估计主要是数据传输/内存瓶颈（PCI-E等），还没有来的及进行针对性优化。 
  
此外还测试了一个开源OpenCL driver： pocl - Portable Computing Language 

[https://github.com/pocl/pocl](https://github.com/pocl/pocl)

性能不亚于Intel的SDK。  


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
