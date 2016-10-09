---
date: 2016-08-20 12:00:00
layout: post
title: 知乎首答-我对手机射频软件的理解
thread: 20160820
categories: hw-sw-fw
tags:  ZHIHU RFSW RF-Software Radio-Frequency-Software iPhone Cell-Phone Antenna-switch LNA PA Matrix
---

作者：纸飞机
链接：https://www.zhihu.com/question/20496717/answer/117644319
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

我曾经是Apple北京射频软件组工程师。现在已离开。干的时间不是很长，谈谈我对射频软件的一些粗浅理解。

简而言之，射频软件是实时控制手机射频部分（由一些射频芯片元器件组成），使得射频部分实时的满足通信需求的软件。

既然是软件，那么它一定跑在某颗处理器上。它一般是跑在基带处理器上。手机里大的处理器芯片有两颗：应用处理器和基带处理器。应用处理器可以粗略的认为就是一台电脑，即跑我们日常应用（比如从app store下载的应用）的处理器。基带处理器类比我们电脑上网用的调制解调器（modem）（即很久以前叫做猫的东西，最早是电话线猫，现在是光猫）。

手机射频系统的复杂之处在于：它需要支持从2g到4g的移动通信全部制式：gsm，cdma，wcdma，tdscdma，lte fdd，lte tdd；它需要支持世界各国划给移动通信的所有频段，离散分布于低频段700MHz（大约）到高频段2.6GHz（大约）之间；它需要支持移动通信之外的其他无线系统：蓝牙，wifi，nfc，gnss（gps，北斗等）。各制式，各频段的带宽，射频指标（最大功率，带外特性，灵敏度等等），工作模式（tdd，fdd）多种多样，而且各国各运营商要求也多种多样。还要通过配置射频系统使得各系统干扰最小化。

实际上手机里的基带芯片（基带处理器）并不是直接连接天线即可通信，在基带芯片和天线之间有一个由很多射频芯片和器件组成的射频系统。射频系统的基本功能是按照基带芯片的实时工作状态需求提供一条合适的射频信号收发链路。基带芯片的需求是：收发信号（不止一路，比如测量时可能会有多路信号，多天线也意味着不止一路）的制式，带宽，频率，功率，带外特性等等。射频系统根据这些需求，通过配置不同外围射频芯片和器件的状态和连接，构成一条或多条从基带的ad转换器da转换器至正确天线的收发链路。一般说来接收链路由天线、收发双工器，低噪放，射频接收机构成，还包括一些控制带外参数的分布于各处的滤波器。发射链路包括发射机、功放、收发双工器，天线，以及一些滤波器。

如果为基带芯片的每种需求都专门配置一套专用的收发链路硬件和天线，那么射频软件将会不需要或者非常简单。但很不幸，这样会造成极大的硬件资源浪费和电路体积增大。比如给手机配置gsm900专用收发链路和天线，gsm1800专用收发链路和天线，lte band 1 专用收发链路和天线，lte band 5 专用收发链路和天线，等等，掰着指头数完了基带芯片支持的所有模式可能需要上百条收发链路和天线。mission impossible。实际的手机射频系统和天线资源很有限，比如通过配置多个射频芯片和器件，射频系统最多可支持x条收发链路，x具体是多少又和具体的制式频段工作状态等有关。手机因为空间限制，天线数量更是少的可怜，因此射频软件和硬件工作起来可以看作是多频段和制式共享的一个射频链路和天线动态资源池，可自动实时适配基带处理器n多种不同的制式频段对收发链路的需求。

连接基带芯片和天线的射频系统里包含的主要芯片和元器件：比如天线切换/复用矩阵（移动通信mimo多天线，wifi多天线，蓝牙，gps等，还有不同频段的天线可能还是分开的，比如wifi 2.4G和5G），射频收发机芯片（常说的transceiver芯片，里面一般不止一路），低噪放，功放，改善信号特性或为了满足各种指标的可调可切换滤波器，等等。以上各种芯片可能不止一个，因为不同制式和频段可能需要不同的芯片，一个都不能少。这些并非我们想象的能够全部集成到一起。射频软件就通过控制这些硬件的配置参数及其连接来实时给基带芯片提供需要的收发链路。

以上是一些背景以及射频系统硬件组成。

射频软件需要正确配置以上射频系统，使之正常工作。除了有庞杂的针对各种频段制式状态的各种配置参数外，更重要的是要实现实时控制：各种状态组合，状态切换。这是因为：

我们的手机在每个瞬间会处在不同状态或者n种状态的组合之下。比如当前服务小区是一种制式和频段，而同时根据网络要求可能需要测量不同制式和频段的其他信号，以便需要时切换到信号更好或者网络期望的工作小区。这种测量意味着我们的手机时而接收服务小区信号，时而搜索目标制式和频段的信号，即射频前端需要很快的在不同的频段和制式之间切换（即动态的射频收发链路建立，拆除，以及各种排列组合），根据频段和制式不同，射频系统可能切换的策略也不同。此外，手机在数据，通话，待机等情况下射频系统状态也不同。各手机厂商也会有一些自定义的不同状态（避免泄密，此处略去）。考虑到之前说的n种制式和频率组合，再加上这多种状态，复杂性可想而知。这种状态的控制和切换往往还会有很严格的时间要求，要求快速而准确。

更进一步的，因为硬件有一定的冗余性和灵活性，实际上某个瞬间满足需求的射频链路配置不止一种（可以使链路经过某器件A管脚，也可以经过B管脚，因为器件可能是宽带的且内部有冗余。但AB管脚通路因为芯片或电路板物理位置不同而性能有差异），这时就需要一些准则来进行实时的最优配置选择：比如功耗低，干扰小，性能高等等。

如何使复杂的射频硬件系统在正确的时刻工作在最优的状态下或者折衷的n种状态组合下？这就是射频软件要做的事情。


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
