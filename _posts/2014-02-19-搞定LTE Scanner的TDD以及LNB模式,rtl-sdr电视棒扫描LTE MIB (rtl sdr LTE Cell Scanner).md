---
date: 2014-02-19 12:00:00
layout: post
title: 搞定LTE Scanner的TDD以及LNB模式,rtl-sdr电视棒扫描LTE MIB (rtl sdr LTE Cell Scanner)
thread: 956
categories: LTE
tags:  LTE rtl-sdr SDR 4G Cell-Search Cell-Scanner Beijing MMDS-LNB LNB
---

(原文刊于被sina关闭的我的sina博客)

首先关于LTE频率，网上都有，比如这里： http://labs.chinamobile.com/news/101634_p2

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
之前频谱扫描见：[土法again LTE D频段(2500~2690MHz)三大运营商TD-LTE频谱全抓(TD-LTE Beijing capture scan)](http://sdr-x.github.io/%E5%9C%9F%E6%B3%95again%20LTE%20D%E9%A2%91%E6%AE%B5%282500~2690MHz%29%E4%B8%89%E5%A4%A7%E8%BF%90%E8%90%A5%E5%95%86TD-LTE%E9%A2%91%E8%B0%B1%E5%85%A8%E6%8A%93%28TD-LTE%20Beijing%20capture%20scan%29/)
和 [4G LTE(北京)信号捕捉(rtl-sdr 电视棒软件无线电)新扫到电信(LTE SDR Beijing)](http://sdr-x.github.io/4G%20LTE%28%E5%8C%97%E4%BA%AC%29%E4%BF%A1%E5%8F%B7%E6%8D%95%E6%8D%89%28rtl-sdr%20%E7%94%B5%E8%A7%86%E6%A3%92%E8%BD%AF%E4%BB%B6%E6%97%A0%E7%BA%BF%E7%94%B5%29%E6%96%B0%E6%89%AB%E5%88%B0%E7%94%B5%E4%BF%A1%28LTE%20SDR%20Beijing%29/)

