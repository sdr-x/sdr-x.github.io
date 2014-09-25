---
date: 2014-08-09 12:00:00
layout: post
title: 使用lameditor和asn1c开源工具解析北京LTE现网 RRC SIB ASN1消息
thread: 153
categories: lte
tags:  asn1c lameditor LTE RRC SIB ASN1 Beijing 4G
---

(原文刊于被sina关闭的我的sina博客)

English version： [http://sdr-x.github.io/LTE-SIB-decoding-by-asn1c/](http://sdr-x.github.io/LTE-SIB-decoding-by-asn1c/)
  
主要参考了这篇blog： 

[http://blog.csdn.net/peng_yw/article/details/22437251](http://blog.csdn.net/peng_yw/article/details/22437251)
  
本工作是这个LTE小区搜索项目的一部分： [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)解析RRC SIB ASN1消息的程序已经封装为一个matlab函数parse_SIB()，并用在这个Matlab/LTE_DL_receiver.m 脚本中，用来将PDSCH信道解调出来的SIB原始比特正确解析为SIB消息。成功解析的北京4G现网的三个SIB消息，见三个附件。 
  
具体方法： 
  
####1. 根据LTE RRC 的spec生成ASN1描述文档 
  
从这里： 

[http://www.3gpp.org/ftp/Specs/archive/36_series/36.331/](http://www.3gpp.org/ftp/Specs/archive/36_series/36.331/)

下载36.331协议文档。 

把36331-ac0.zip解压缩，得到36331-ac0.doc 

把36331-ac0.doc另存为36331-ac0.txt. 
  
从这里 

[http://sourceforge.net/projects/lameditor/](http://sourceforge.net/projects/lameditor/)

下载lameditor工具，并编译安装好。 
  
然后执行： 
  
cd lameditor-1.0/src/getasn/ 
  
把36331-ac0.txt拷贝到上述目录，然后运行： 

./getasn 36331-ac0.txt 
  
至此，得到LTE协议的ASN1描述文件36331-ac0.asn 
  
####2. 为LTE协议解析生成ASN1解码器 
  
从这里： 

[https://github.com/vlm/asn1c](https://github.com/vlm/asn1c)

获取asn1c工具，并编译安装。 
  
然后执行： 
  
cd asn1c/examples/ 

mkdir sample.source.LTERRC 

cd sample.source.LTERRC 
  
把第一步里得到的36331-ac0.asn拷贝到目录sample.source.LTERRC，然后执行： 
  
asn1c  -S /usr/local/share/asn1c -fcompound-names -fskeletons-copy -gen-PER -pdu=auto 36331-ac0.asn 
  
得到编译工具所需的源文件。在正式编译我们所需的工具之前，需要对源文件做如下修改： 
  
文件converter-sample.c: 

在#include <asn_internal.h>后面，加入： 

\#define PDU BCCH_DL_SCH_Message 

\#define ASN_PDU_COLLECTION 
  
文件per_opentype.c: 

在ASN_DEBUG("Too large padding %d in open type", (int)padding);后面，加入： 

padding = padding % 8; 

并且注释掉：_ASN_DECODE_FAILED; 
  
现在编译解码工具，执行： 
  
make -f Makefile.am.sample 
  
得到解码工具progname 
  
####3. 解码工具的使用。 
  
执行： 
  
./progname recv_bits.per -p BCCH-DL-SCH-Message 
  
recv_bit.per即收到的PDSCH上的SIB的原始bit存成的二进制文件。-p用来指定消息类型，PDSCH上的SIB的消息类型为：BCCH-DL-SCH-Message 
  
运行命令之后，会打印出解析出来的SIB消息各个字段的名称、内容。 
  
  
附件(2.5KB) [f2360_s19.2_bw20_1s_hackrf_SIB.txt](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2360_s19.2_bw20_1s_hackrf_SIB.txt)

附件(4.5KB) [f2585_s19.2_bw20_1s_hackrf_1_SIB.txt](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2585_s19.2_bw20_1s_hackrf_SIB.txt)

附件(11.3KB) [f2585_s19.2_bw20_1s_hackrf_SIB.txt](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2585_s19.2_bw20_1s_hackrf_1_SIB.txt)
