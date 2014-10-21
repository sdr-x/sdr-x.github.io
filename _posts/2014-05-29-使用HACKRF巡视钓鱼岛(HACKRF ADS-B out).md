---
date: 2014-05-29 12:00:00
layout: post
title: 使用HACKRF巡视钓鱼岛(HACKRF ADS-B out)
thread: 676
categories: ADS-B
tags:  ADS-B ADSB-OUT HACKRF SDR Software-Defined-Radio Gnu-Radio GRC
---

(原文刊于被sina关闭的我的sina博客)

好吧，承认我是标题党。其实就是用HACKRF发射ADS-B信号，因为技术含量偏低，只好标题博眼球了。。。
  
做了这么个简单的实验（见附件图和视频：[http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html](http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html) ）。
  
实现的功能是：
  
matlab按照ads-b协议生成信号，因为自己生成可以填入任意经纬度等信息，经HACKRF和天线发射，然后由rtl-sdr电视棒配合dump1090或者gr-air-modes接收。
  
生成信号的坐标序列是按照钓鱼岛周围一圈设置的，所以你在ADS-B接收界面中看到的就是一架飞机在以3000ft的高度巡视钓鱼岛。。。

**警告：**
  
不要真的用1090MHz实验！如果实验，你该找一个合法的或者足够安全的频率（比如某些敌国卫星的生癖频率），以最最最低的功率发射，使用闭路射频电缆和衰减器，最好在地下室或者暗室玩这个。
  
出了事后果自负。
  
不要问我源代码在哪里，不会公开。
  
其实发射相对接收简单许多，按照协议做就是了，对于熟手，依靠互联网公开的资料，半天到一天就能搞定。

用于回放信号的grc在这里： [https://github.com/JiaoXianjun/GNSS-GPS-SDR/tree/master/adsb](https://github.com/JiaoXianjun/GNSS-GPS-SDR/tree/master/adsb)
