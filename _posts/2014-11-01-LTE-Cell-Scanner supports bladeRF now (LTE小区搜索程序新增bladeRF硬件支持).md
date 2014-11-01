---
date: 2014-11-01 12:00:00
layout: post
title: LTE-Cell-Scanner supports bladeRF now (LTE小区搜索程序新增bladeRF硬件支持)
thread: 2014110123423
categories: LTE
tags:  LTE TD-LTE SDR 4G Beijing Cell-Search Cell-Scanner C C++ bladeRF 1.92Msps MIB HACKRF
---

After [LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner) support rtlsdr hackRF, not it support bladeRF!

Here is part of README:

An OpenCL accelerated TDD/FDD LTE Scanner (from rtlsdr/hackRF/bladeRF A/D samples to PDSCH output and RRC SIB messages decoded). By Jiao Xianjun (putaoshu@gmail.com). Tech blog: http://sdr-x.github.io

------------------------------
New features, make and Usages
------------------------------

**0x00. basic method to build program**
            
            mkdir build
            cd build
            cmake ../
            make
            
CellSearch and LTE-Tracker program will be generated in build/src. Use "--help" when invoke program to see all options

**0x01. cmake to build for different hadware**
      
            cmake ../ -DUSE_BLADERF=1   -- build for bladeRF
            cmake ../ -DUSE_HACKRF=1    -- build for hackRF
            cmake ../                   -- default for rtlsdr

...

See complete README here: [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)

See video in China: 

See video outside China:

