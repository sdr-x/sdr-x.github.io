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
    make
    dfu-util --device 1fc9:000c --alt 0 --download hackrf_usb.dfu
    cmake .. -DBOARD=HACKRF_ONE
    make
    ~/git/hackrf/host/build/hackrf-tools/src/hackrf_spiflash -w hackrf_usb.bin

