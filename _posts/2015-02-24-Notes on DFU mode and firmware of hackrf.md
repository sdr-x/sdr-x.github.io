---
date: 2015-02-24 12:00:00
layout: post
title: Notes on DFU mode and firmware of hackrf
thread: 2015022413
categories: sdr
tags:  HACKRF DFU firmware
---

**make command**

[https://github.com/mossmann/hackrf/issues/117](https://github.com/mossmann/hackrf/issues/117)

    diff --git a/firmware/hackrf-common.cmake b/firmware/hackrf-common.cmake
    index df5d5c9..c43484c 100644
    --- a/firmware/hackrf-common.cmake
    +++ b/firmware/hackrf-common.cmake
    @@ -164,7 +164,8 @@ macro(DeclareTargets)
            DEPENDS ${PROJECT_NAME}.bin
            COMMAND rm -f _tmp.dfu _header.bin
            COMMAND cp ${PROJECT_NAME}.bin _tmp.dfu
    -       COMMAND dfu-suffix --vid=0x1fc9 --pid=0x000c --did=0x0 -s 0 -a _tmp.dfu
    +       COMMAND dfu-suffix --vid=0x1fc9 --pid=0x000c --did=0x0 -a _tmp.dfu
    +       COMMAND dfu-prefix -s 0 -a _tmp.dfu
            COMMAND python -c \"import os.path\; import struct\; print\('0000000: da ff ' + ' '.join\(map\(lambda s: '%02x' % ord\(s\), struct.pack\('<H', os.path.getsize\('${PROJECT_NAME}.bin'\) / 512 + 1\)\)\) + ' ff ff ff ff'\)\" | xxd -g1 -r > _header.bin
            COMMAND cat _header.bin _tmp.dfu >${PROJECT_NAME}.dfu
            COMMAND rm -f _tmp.dfu _header.bin

**dfu tools download:**

[git://gitorious.org/dfu-util/dfu-util.git](git://gitorious.org/dfu-util/dfu-util.git)

**make sure you use dfu-util dfu-suffix and dfu-prefix of above source. DO NOT USE MIXTURE SOURCES OF THE THREE TOOLS**

[git://git.openezx.org/dfu-util.git](git://git.openezx.org/dfu-util.git)

[http://dfu-util.gnumonks.org/](http://dfu-util.gnumonks.org/)

and search dfu-util in [github.com](github.com)

**dfu operation**

    (ADD sudo IF NOT WORK)
    hackrf/firmware/hackrf_usb/build$ cmake .. -DBOARD=HACKRF_ONE -DRUN_FROM=RAM
    hackrf/firmware/hackrf_usb$ make
    hackrf/firmware/hackrf_usb/hackrf_usb$ dfu-util --device 1fc9:000c --alt 0 --download hackrf_usb.dfu
    cmake .. -DBOARD=HACKRF_ONE
    make
    ~/git/hackrf/host/build/hackrf-tools/src/hackrf_spiflash -w hackrf_usb.bin



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
