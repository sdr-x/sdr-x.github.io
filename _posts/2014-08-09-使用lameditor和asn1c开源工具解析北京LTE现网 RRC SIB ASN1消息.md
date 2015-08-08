---
date: 2014-08-09 12:00:00
layout: post
title: 使用lameditor和asn1c开源工具解析北京LTE现网 RRC SIB ASN1消息
thread: 153
categories: lte
tags:  asn1c lameditor LTE RRC SIB ASN1 Beijing 4G HACKRF rtl-sdr LTE-Cell-Scanner
---

(原文刊于被sina关闭的我的sina博客)

English version： [http://sdr-x.github.io/LTE-SIB-decoding-by-asn1c/](http://sdr-x.github.io/LTE-SIB-decoding-by-asn1c/)
  
主要参考了这篇blog： 

[http://blog.csdn.net/peng_yw/article/details/22437251](http://blog.csdn.net/peng_yw/article/details/22437251)
  
本工作是这个LTE小区搜索项目的一部分： [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner). [这篇博文](http://sdr-x.github.io/%E5%AE%8C%E6%95%B420MHz%E5%B8%A6%E5%AE%BD%E9%85%8D%E7%BD%AELTE%E4%BF%A1%E5%8F%B7%E8%A2%ABHACKRF-19.2M%E9%87%87%E6%A0%B7%E7%8E%87%E6%88%90%E5%8A%9F%E8%A7%A3%E6%9E%90/)是最新进展

解析RRC SIB ASN1消息的程序已经封装为一个matlab函数parse_SIB()，并用在这个Matlab/LTE_DL_receiver.m 脚本中，用来将PDSCH信道解调出来的SIB原始比特正确解析为SIB消息。成功解析的北京4G现网的三个SIB消息，见三个附件。 
  
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

这里 **regression_test_signal_file** 目录（[https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)）下有一些抓取的per二进制文件（而非文本文件，因为解码程序需要二进制文件作为输入），可以用以测试你的程序是否正确

一些per文件：

[f2360_sib1.per](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2360_sib1.per)

[f2585_sib1-10.per](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2585_sib1-10.per)

[f2585_sib2-3.per](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2585_sib2-3.per)

[f2585_sib3-1.per](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2585_sib3-1.per)

[f2585_sib5-10.per](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2585_sib5-10.per)

一些解析出来的SIB消息（并非与per文件对应）：

附件(2.5KB) [f2360_s19.2_bw20_1s_hackrf_SIB.txt](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2360_s19.2_bw20_1s_hackrf_SIB.txt)

附件(4.5KB) [f2585_s19.2_bw20_1s_hackrf_1_SIB.txt](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2585_s19.2_bw20_1s_hackrf_SIB.txt)

附件(11.3KB) [f2585_s19.2_bw20_1s_hackrf_SIB.txt](https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/f2585_s19.2_bw20_1s_hackrf_1_SIB.txt)


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
