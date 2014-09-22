---
date: 2010-10-02 12:00:00
layout: post
title: 关于清华留美学生高杏欣破解北斗扩频码事件(About Gao Xingxin crack compass codes)
thread: 22
categories: satellite
tags:  GPS Compass Beidou China Crack
---

美国《Inside　GNSS》杂志，披露2007年美国斯坦福大学研究人员成功破解我国“北斗”导航卫星信号编码程序的情况。因其中一研究人员为中国留学生高杏欣，这一消息传回国内后，一度在网上传得沸沸扬扬。甚至在民众引起一定恐慌和愤怒。

但从具体的资料分析下来，无需大惊小怪：

就公开的高杏欣论文内容来说，她破解的应该是民用部分（即本来就要公开或者低价许可公开的）。

因为破解的两个码都是周期1ms的（加上一个20ms的扰变成周期20ms），一个2M chip率，一个10M chip率，军事用的码周期不可能这么短，至少是若干天的周期。比如GPS 的民用C/A码周期1ms，1M chip率，军用的P码周期就若干天，10M chip率。

这也说明北斗系统面向民用可以轻易地提供比GPS更 高的定位精度，毕竟是2M chip率和10M chip率的民用码，比起1M chip率的GPS民用码时间/距离分辨率提高2倍或10倍。因此如果北斗建成应该在民/商用领域领先于GPS（晚于美国几十年建设系统，后发优势）。当然，GPS也可以升级。

她的论文中也提到了北斗的两个地方是没有破解的，一个是E6载波上的不明信号（Also visible is an as yet unidentified 1 MHz–wide transmission centered around 1257 MHz）.另一个是：载波的正交分量上（与同相分量相对）的更长周期信号码结构（她破解的只是E2、E5b、E6载波上的同相信号码结构：Analysis of the short data sets from May 7 indicated that the Compass E2 quadrature channel (Q-channel) had a significantly longer sequence than the in phase channel (I-channel). As a result, we collected additional data in June in order to obtain longer data sets with which to work.）。即更长周期的码（估计是若干天周期量级的军用部分）她仍未破解。

从BBS: newsmth.net上的讨论来看，她破解这种1ms周期码所用的技术也很常规。此事为何在美国弄得如此高调大家也都参不透。可能是政治上的一些考虑也未可知，纯属猜测。

因此总的说来公开的论文部分就是一项针对北斗系统的普通的学术界研究工作。（如果没有更多的未公开的工作的话）

