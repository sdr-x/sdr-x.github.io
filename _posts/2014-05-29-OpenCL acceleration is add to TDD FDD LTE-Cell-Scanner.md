---
date: 2014-05-29 12:00:00
layout: post
title: OpenCL acceleration is add to TDD FDD LTE-Cell-Scanner
thread: 64653
categories: LTE
tags:  LTE TD-LTE SDR 4G Beijing Cell-Search Cell-Scanner OpenCL POCL Portable-OpenCL Intel AMD Nvidia GeForce
---

Hello there,

New features comes to [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)

1. Now MATLAB isn't needed. All in C/C++. All you need are rtl-sdr dongle and Linux computer. TDD and FDD are both supported. External LNB/Mixer (allow mixer and A/D use non-coherent clock sources) is also supported.

2. OpenCL parallel computation is supported! It searches per frequency point in

**0.5s (desktop i7-3930K 6 cores@3.2GHz) 22x faster!**
**or 2~3s (ThinkPad T410 i5 CPU M520@2.40GHz).**

Intel, AMD and Nvidia OpenCL SDK are tested. Acceleration ratio is gotten by Intel SDK.

Nvidia GPU hasn't reached so amazing acceleration ratio. It maybe because of slow PCI-E and memory. I haven't found time to optimize on that.

Another open source OpenCL lib pocl - Portable Computing Language (https://github.com/pocl/pocl) is also tested, and it seems as fast as Intel's! (See video I attached in the end.)

3. Some improvements on algorithm. Higher sensitivity and more robust.

video (out fo China): [http://www.youtube.com/watch?v=SxZzEVEKuRs](http://www.youtube.com/watch?v=SxZzEVEKuRs)

video (in China) [http://pan.baidu.com/s/1o6qbLGY](http://pan.baidu.com/s/1o6qbLGY)

Have fun with LTE. Have fun with OpenCL!

BR

Jiao Xianjun (Ryan)