---
date: 2014-11-02 12:00:00
layout: post
title: test rtlsdr hackRF and bladeRF in VMware virtual machine (虚拟机内使用软件无线电)
thread: 2014110223423
categories: LTE
tags:  LTE TD-LTE SDR Cell-Search Cell-Scanner rtlsdr hackRF bladeRF VMware Virtual-Machine
---

test some cases to see if rtlsdr hackRF and bladeRF can work in virtual machine smoothly.

rtlsdr dongle and bladeRF work well in virtual machine for this [LTE Cell Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner), 
while hackrf_info always reports:

Found HackRF board.

hackrf_board_id_read() failed: HACKRF_ERROR_LIBUSB (-1000)

Since rtlsdr and bladeRF work in virtual machina, there is a hope for hackrf to work also? Any ideas or suggestions?

My computer configuration:

host OS: Win7

guest OS: Ubuntu14 (LTS)

virtual machine: VMware 10.0.4

mainboard: ASUS P9X79 PRO -LGA 2011/Intel X79 /ATX

CPU: i7 3930K -3.2GHz/LGA 2011/12M 120W

memory: Kingston HyperX 16G(4GBx4) DDR3 1600 KHX1600C9D3K4/16GX

graphic: GTX550TI 950MHz/4100MHz 1GB/192BIT DDR5 PCI-E

Hope above is useful for someone.
