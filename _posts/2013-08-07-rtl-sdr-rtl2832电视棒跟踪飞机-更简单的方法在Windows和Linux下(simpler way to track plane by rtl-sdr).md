---
date: 2013-08-07 12:00:00
layout: post
title: rtl-sdr-rtl2832电视棒跟踪飞机-更简单的方法在Windows和Linux下(simpler way to track plane by rtl-sdr)
thread: 78316
categories: ADS-B
tags:  rtl-sdr rtl2832 SDR ADS-B plane-track aircraft-track gr-air-modes dump1090 rtl1090
---

(原文刊于被sina关闭的我的sina博客)

在这两篇blog（[1](http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BA%28ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes%29/) 和 [2](http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BAstep-by-step%E6%95%99%E7%A8%8B%28tutorial%20ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes%29/)）中使用gr-air-modes软件在Linux下追踪飞机，比较繁琐。

这里给出两个比较简单的软件。

** Windows下 **

windows下实际上需要安装驱动等多个步骤，但是这里 [http://rtl1090.web99.de/homepage/index.php?USER=rtl1090&XURL=web99.de&goto=1](http://rtl1090.web99.de/homepage/index.php?USER=rtl1090&XURL=web99.de&goto=1)
老外给出了一个一揽子解决方案。

具体的： 从这里下载 [http://globe-s.eu/download/rtl1090imu.exe](http://globe-s.eu/download/rtl1090imu.exe) 安装，这个程序会完成一系列步骤（需要人参与），并进入可用状态。

这样你在windows下插上电视棒就能方便的“看飞机”了。

软件界面如下图（图片来自互联网）：

!()[../media/rtl-sdr-rtl1090_list_data.png]

** Linux下 **

用dump1090这个程序.

这个程序可以配合电视棒实时在google map上显示飞机位置和飞行方向,而且是动态更新的,同时在命令行提供各飞机跟踪的详细信息.
    
方法也很简单.

**1. 下载和安装dump1090**. (可以参见那篇[step by step](http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BAstep-by-step%E6%95%99%E7%A8%8B%28tutorial%20ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes%29/)里对gr-air-modes下载安装方法)

软件是开源的: [https://github.com/antirez/dump1090](https://github.com/antirez/dump1090)

**2. 运行命令:**

    ./dump1090 --aggressive --net --interactive
    
这时候一般应该能在命令行看到实时更新的飞机信息了(如果天线靠近窗户或者在室外,而且附近有航线或机场)

**3. 打开浏览器,输入:**

    [http://127.0.0.1:8080/](http://127.0.0.1:8080/)

google map会自动打开, 右侧栏会看到目前地图上飞机的数量和航班号等信息.

地图上看不到?不要急,你需要手工把地图平移缩放到你所在地点周围,你就能看到箭头样子的飞机了.

下图是在室内抓到的一架飞机: 南五环外一个朝南的箭头. 

!()[../media/rtl-sdr-dump1090.jpg]
