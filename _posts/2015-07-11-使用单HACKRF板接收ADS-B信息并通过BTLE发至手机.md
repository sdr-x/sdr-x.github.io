---
date: 2015-07-11 12:01:00
layout: post
title: 使用单HACKRF板接收ADS-B信息并通过BTLE发至手机
thread: 2015071121
categories: ads-b
tags:  HACKRF DFU firmware ADS-B BTLE relay 1090MHz 2.4GHz TDD
---

(本文提到的固件也可以这里下载: [https://github.com/JiaoXianjun/ADS-B-BTLE-air-relay-HACKRF-firmware](https://github.com/JiaoXianjun/ADS-B-BTLE-air-relay-HACKRF-firmware) )

(为了降低风险，转发至手机的的经纬度信息已经人为降级)

写了一个hackrf固件，可以把接收的ADS-B (1090MHz) 包解析后，信息通过BTLE (2.4GHz) 链路广播. 这时你用手机就能查看飞机信息了. (比如用这个APP LightBlue。APP store下载). ([youku视频](http://v.youku.com/v_show/id_XMTI4MjY2NDc0OA==.html), [youtube视频](https://youtu.be/MqX74sk-sa4))

![](../media/adsb-btle-air-relay.png)

两种尝试我的固件的方法:

####1. 把固件临时加载进hackrf的RAM并运行

(hackrf掉电或者重启后会丢失此固件，因此不会影响原有固件)

下载固件 [adsb-btle-air-relay.dfu](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/adsb-btle-air-relay.dfu)

下载和安装dfu-util:

    git clone git://gitorious.org/dfu-util/dfu-util.git

也放了一个dfu-util在这里 [dfu-util.tar.gz](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/dfu-util.tar.gz)

启动HackRF One 至 DFU 模式：在上电或者重启hackrf时按下 DFU键不放. 在 3V3 LED 亮起后释放DFU键. (详细介绍  [https://github.com/mossmann/hackrf](https://github.com/mossmann/hackrf))

加载我的固件进RAM并且运行:

    sudo dfu-util --device 1fc9:000c --alt 0 --download adsb-btle-air-relay.dfu

(注意，这种方法掉电或者重启后固件不保留，必须重新DFU进RAM)

打开LightBlue APP, 你将会看到类似下面的飞机信息:

    ICAO-addr/Flight   altitude   speed   latitude   lontitude

####2. 把我的固件刷进hackrf的flash固化

(掉电也不会丢失，但会冲到你原有的固件，所以你可能将来需要刷回原有固件)

下载 [adsb-btle-air-relay.bin](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/adsb-btle-air-relay.bin).

进行下一步之前，请确保你的hackrf正在跑原始固件（因为只有原始固件能和hackrf_spiflash通信）, 然后：

    hackrf_spiflash -w adsb-btle-air-relay.bin

hackrf_spiflash 工具应该已经随你的hackrf驱动安装了 [https://github.com/mossmann/hackrf](https://github.com/mossmann/hackrf). 我也放了一个hackrf_spiflash在这里 [here](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_spiflash)

运行时, TX led (红) 应该闪动的很快. 如果没有闪, 你可能需要把板子启动到DFU模式，再按一下reset键，可能就跑起来了.

####* 如何刷回hackrf原始固件

因为我的固件不支持和hackrf_spiflash工具通信, 所以你需要先把hackrf原始固件以DFU方式刷进RAM跑起来（这时就能和hackrf_spiflash正常通信了）, 然后再用hackrf_spiflash工具把hackrf原始固件刷进flash固化.

进DFU模式，然后：

    dfu-util --device 1fc9:000c --alt 0 --download hackrf_usb.dfu

我也放了一个在这里 [hackrf_usb.dfu](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_usb.dfu))

注意：此时千万不要重启你的板子。

再刷原始固件进板子的flash固化:

    hackrf_spiflash -w hackrf_usb.bin

我也放了一个[hackrf_usb.bin]在这里 [hackrf_usb.bin](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_usb.bin))
 
如果没有错误，你的板子就恢复了原始固件.

想知道如何编译固件和刷新固件的详细信息，可以参见 [https://github.com/mossmann/hackrf](https://github.com/mossmann/hackrf) （firmware目录）



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
