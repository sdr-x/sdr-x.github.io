---
date: 2014-11-01 12:00:00
layout: post
title: LTE-Cell-Scanner supports bladeRF now (LTE小区搜索程序新增bladeRF硬件支持)
thread: 2014110123423
categories: lte
tags:  LTE TD-LTE SDR 4G Beijing Cell-Search Cell-Scanner C C++ bladeRF 1.92Msps MIB HACKRF
---

After [LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner) supports rtlsdr hackRF, now it supports bladeRF!

Here is part of README:

An OpenCL accelerated TDD/FDD LTE Scanner (from rtlsdr/hackRF/bladeRF A/D samples to PDSCH output and RRC SIB messages decoded). By Jiao Xianjun ([putaoshu@gmail.com](mailto:putaoshu@gmail.com)). Tech blog: [http://sdr-x.github.io](http://sdr-x.github.io)

------------------------------
New features, make and Usages
------------------------------

**0x01. basic method to build program**

    mkdir build
    cd build
    cmake ../                   -- default for rtlsdr and OpenCL ON;   OR
    cmake ../ -DUSE_BLADERF=1   -- build for bladeRF;    OR
    cmake ../ -DUSE_HACKRF=1    -- build for hackRF
    cmake ../ -DUSE_OPENCL=0    -- disable OpenCL (See notes in later chapter)
    make

CellSearch and LTE-Tracker program will be generated in build/src. Use "--help" when invoke program to see all options.

(You may need some related libraries, such as itpp, fftw, libboost-, Curses, ... etc.)

**0x02. basic usage (If you have OpenCL, make sure those .cl files in LTE-Cell-Scanner/src have been copy to program directory)**

*0x02.1* CellSearch --freq-start 1890000000   (try to search LTE Cell at 1890MHz)

    output:
    ...
    Detected a TDD cell! At freqeuncy 1890MHz, try 0
    cell ID: 253
    PSS ID: 1
    RX power level: -17.0064 dB
    residual frequency offset: -48.0366 Hz
                k_factor: 1
    ...
    Detected the following cells:
    Meaning -- DPX:TDD/FDD; A: #antenna ports C: CP type ; P: PHICH duration ; PR: PHICH resource type
    DPX  CID  A     fc  freq-offset RXPWR  C   nRB  P   PR  CrystalCorrectionFactor
    TDD  253  2  1890M         -48h   -17  N  100   N  1/2   0.99999997458380551763
    
*0x02.2* LTE-Tracker -f 1890000000  (try to track LTE Cell at 1890MHz)

*0x02.3* LTE_DL_receiver (Matlab script. Decode RRC SIB message in PDSCH by reading captured signal bin file)

*0x02.4* LTE_DL_receiver 1890 40 40 (Matlab script. Decode SIB at 1890MHz lively with LNA VGA gain of hackRF 40dB 40dB)

    output:
    ...
    TDD SFN-864 ULDL-2-|D|S|U|D|D|D|S|U|D|D| CID-216 nPort-2 CP-normal PHICH-DUR-normal-RES-1
    SF0 PHICH1 PDCCH1 RNTI: 
    ...
    SF4 PHICH1 PDCCH1 RNTI: SI-RNTI SI-RNTI 
    PDCCH   No.0  4CCE: Localized VRB from RB0 to RB11 MCS-7 HARQ-0 NEWind-0 RV-0 TPC-1 DAI-0
    Calling asn1c decoder (../asn1_test/LTE-BCCH-DL-SCH-decode/progname) for BCCH-DL-SCH-Message.
    ../asn1_test/LTE-BCCH-DL-SCH-decode/progname tmp_sib_info.per -p BCCH-DL-SCH-Message
    <BCCH-DL-SCH-Message>
        <message>
            <c1>
                <systemInformation>
                    <criticalExtensions>
                        <systemInformation-r8>
                            <sib-TypeAndInfo>
                                    <sib2>
                                        <radioResourceConfigCommon>
                                            <rach-ConfigCommon>
                                                <preambleInfo>
                                                    <numberOfRA-Preambles><n52/></numberOfRA-Preambles>
    ...

See complete README here: [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)

See video outside China: [http://youtu.be/rg6ENh-tbJY](http://youtu.be/rg6ENh-tbJY)

See video in China: [http://v.youku.com/v_show/id_204158978.html](http://v.youku.com/v_show/id_204158978.html)


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
