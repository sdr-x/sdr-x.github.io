---
date: 2014-08-05 12:00:00
layout: post
title: A BTLE (Bluetooth Low energy) BT4.0 radio packet sender (BladeRF, HACKRF)
thread: 45318
categories: BTLE
tags:  BTLE HACKRF BladeRF BT4.0 packet-sender packet-sniffer iBeacon
---

Hi,

I just add bladeRF support to my BTLE radio packet sender (only support HACKRF before), which is verified by iPhone/iPad with "Locate" APP and TI's packet sniffer. 
You can find code and how to use it here: [https://github.com/JiaoXianjun/](https://github.com/JiaoXianjun/) repo BTLE

Seems that the bladerf_sync_tx() mechanism is pretty easy to use. There is already bladerf_sync_rx() example in libbladeRF.h, this project actually gives bladerf_sync_tx() example. You may check btle_tx.c in the project.

(I am planning to add bladeRF support to the OpenCL LTE Cell Scanner ([https://github.com/JiaoXianjun/](https://github.com/JiaoXianjun/) repo LTE-Cell-Scanner). Anyone interested in porting it to bladeRF, feel free to work together with me.)

Here is BTLE packet sender project README:

All BTLE link layer packet formats are supported. ( Chapter 2&3, PartB, Volume 6, Core_V4.0.pdf. find the spec in [https://www.bluetooth.org/en-us/specification/adopted-specifications](https://www.bluetooth.org/en-us/specification/adopted-specifications) )

It can be used to transmit arbitrary pre-defined BTLE signal/packet sequence, such as raw bits to GFSK modulator, 
iBeacon packet, Connection establishment procedure packet in TI's website: 
[http://processors.wiki.ti.com/index.php/BLE_sniffer_guide](http://processors.wiki.ti.com/index.php/BLE_sniffer_guide), 
or any other packets you want. Together with TI's packet sniffer, you will have full TX and RX abilities. 

See video demo 1 [http://youtu.be/Y8ttV5AEb-g](http://youtu.be/Y8ttV5AEb-g) (outside China) or 
video demo 2 [http://v.youku.com/v_show/id_XNzUxMDIzNzAw.html](http://v.youku.com/v_show/id_XNzUxMDIzNzAw.html) (inside China)

----Build:

    cd host
    mkdir build
    cd build
    cmake ../ -DUSE_BLADERF=1 (without -DUSE_BLADERF=1, HACKRF is used by default. )
    make
    sudo make install (or not install, just use btle_tx in hackrf-tools/src)

----Usage method 1:

    btle_tx packet1 packet2 ... packetX ... rN

----Usage method 2:

    btle_tx packets.txt

In method 2, just those command line parameters (packet1 ... rN) in method 1 are written/grouped in a .txt 
file as input of btle_tx tool. One parameter one line. A line start with "#" is regarded as comment. 
See packets.txt example in btle-tools/src.

"packetX" is one string which describes one packet. All packets compose a packets sequence.

"rN" means the sequence will be repeated for N times. If it is not specified, the sequence will only be sent once.

----Format of packet descriptor "packetX"

    channel_number-packet_type-field-value-field-value-...-Space-value

Each descriptor string starts with BTLE channel number (0~39), then followed by packet_type 
(RAW/iBeacon/ADV_IND/ADV_DIRECT_IND/etc. See all format examples at the end), 
then followed by field-value pair which is packet_type specific, 
at last there is Space-value pair (optional) where the value specifies how many 
millisecond will be waited after this packet sent (by this Space field, actually you can 
define any packet sequence pattern as you want).

----iBeacon example: (iBeacon principle: [http://www.warski.org/blog/2014/01/how-ibeacons-work/](http://www.warski.org/blog/2014/01/how-ibeacons-work/) )

    ./btle_tx 37-iBeacon-AdvA-010203040506-UUID-B9407F30F5F8466EAFF925556B57FE6D-Major-0008-Minor-0009-TxPower-C5-Space-100 r100

Above command sends iBeacon packet and repeats it 100 times with 100ms time space 
(If you have "Locate" app in your iPhone/iPad, it will detect the packet and show the iBeacon info.). 
The packet descriptor string:
    
    37-iBeacon-AdvA-010203040506-UUID-B9407F30F5F8466EAFF925556B57FE6D-Major-0008-Minor-0009-TxPower-C5-Space-100
    
    37 -- channel 37 (one of BTLE Advertising channel 37 38 39)
    
    iBeacon -- packet format key word which means iBeacon format. (Actually it is ADV_IND format in Core_V4.0.pdf)
    
    AdvA -- Advertising address (MAC address) which is set as 010203040506 (See Core_V4.0.pdf)
    
    UUID -- here we specify it as Estimote’s fixed UUID: B9407F30F5F8466EAFF925556B57FE6D
    
    Major -- major number of iBeacon format. (Here it is 0008)
    
    Minor -- minor number of iBeacon format. (Here it is 0009)
    
    Txpower -- transmit power parameter of iBeacon format (Here it is C5)
    
    Space -- How many millisecond will be waited after this packet sent. (Here it is 100ms)

----Connection establishment example: (See "Connection establishment" part of 
[http://processors.wiki.ti.com/index.php/BLE_sniffer_guide](http://processors.wiki.ti.com/index.php/BLE_sniffer_guide) )

    ./btle_tx 37-ADV_IND-TxAdd-0-RxAdd-0-AdvA-90D7EBB19299-AdvData-0201050702031802180418-Space-1000 37-CONNECT_REQ-TxAdd-0-RxAdd-0-InitA-001830EA965F-AdvA-90D7EBB19299-AA-60850A1B-CRCInit-A77B22-WinSize-02-WinOffset-000F-Interval-0050-Latency-0000-Timeout-07D0-ChM-1FFFFFFFFF-Hop-9-SCA-5-Space-1000 9-LL_DATA-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-DATA-X-CRCInit-A77B22-Space-1000

Above simulates a Connection establishment procedure between device 1 and device 2.

The 1st packet 37-ADV_IND-TxAdd-0-RxAdd-0-AdvA-90D7EBB19299-AdvData-0201050702031802180418-Space-1000

device 1 sends ADV_IND packet in channel 37.

The 2nd packet 37-CONNECT_REQ-TxAdd-0-RxAdd-0-InitA-001830EA965F-AdvA-90D7EBB19299-AA-60850A1B-CRCInit-A77B22-WinSize-02-WinOffset-000F-Interval-0050-Latency-0000-Timeout-07D0-ChM-1FFFFFFFFF-Hop-9-SCA-5-Space-1000

After device 2 (in scanning state) receives the ADV packet from device 1, device 2 sends CONNECT_REQ packet to request connection setup with device 1. In this request packet, there are device 2 MAC address (InitA), target MAC address (device 1 MAC address AdvA), Access address (AA) which will be used by device 1 in following packet sending in data channel, CRC initilization value for following device 1 sending packet, Hopping channel information (ChM and Hop) for data channel used by device 1, etc.

The 3rd packet 9-LL_DATA-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-DATA-X-CRCInit-A77B22-Space-1000

device 1 send an empty Link layer data PDU in channel 9 (decided by hopping scheme) according to those connection request information received from device 2. (One "X" after field "DATA" means there is no data for this field )

Time space between packets are 1s (1000ms). Tune TI's packet sniffer to channel 37, then above establishment procedure will be captured.

----Packet descriptor examples for all formats:

**RAW packets**: (All bits will be sent to GFSK modulator directly)

    13-RAW-AAD6BE898E5F134B5D86F2999CC3D7DF5EDF15DEE39AA2E5D0728EB68B0E449B07C547B80EAA8DD257A0E5EACB0B

**ADVERTISING CHANNEL packets**:

    37-IBEACON-AdvA-010203040506-UUID-B9407F30F5F8466EAFF925556B57FE6D-Major-0008-Minor-0009-TxPower-C5
    37-ADV_IND-TxAdd-1-RxAdd-0-AdvA-010203040506-AdvData-00112233445566778899AABBCCDDEEFF
    37-ADV_DIRECT_IND-TxAdd-1-RxAdd-0-AdvA-010203040506-InitA-0708090A0B0C
    37-ADV_NONCONN_IND-TxAdd-1-RxAdd-0-AdvA-010203040506-AdvData-00112233445566778899AABBCCDDEEFF
    37-ADV_SCAN_IND-TxAdd-1-RxAdd-0-AdvA-010203040506-AdvData-00112233445566778899AABBCCDDEEFF
    37-SCAN_REQ-TxAdd-1-RxAdd-0-ScanA-010203040506-AdvA-0708090A0B0C
    37-SCAN_RSP-TxAdd-1-RxAdd-0-AdvA-010203040506-ScanRspData-00112233445566778899AABBCCDDEEFF
    37-CONNECT_REQ-TxAdd-1-RxAdd-0-InitA-010203040506-AdvA-0708090A0B0C-AA-01020304-CRCInit-050607-WinSize-08-WinOffset-090A-Interval-0B0C-Latency-0D0E-Timeout-0F00-ChM-0102030405-Hop-3-SCA-4

**DATA CHANNEL packets**:

    9-LL_DATA-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-DATA-X-CRCInit-A77B22
    9-LL_CONNECTION_UPDATE_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-WinSize-02-WinOffset-000F-Interval-0050-Latency-0000-Timeout-07D0-Instant-0000-CRCInit-A77B22
    9-LL_CHANNEL_MAP_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-ChM-1FFFFFFFFF-Instant-0001-CRCInit-A77B22
    9-LL_TERMINATE_IND-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-ErrorCode-00-CRCInit-A77B22
    9-LL_ENC_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-Rand-0102030405060708-EDIV-090A-SKDm-0102030405060708-IVm-090A0B0C-CRCInit-A77B22
    9-LL_ENC_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-SKDs-0102030405060708-IVs-01020304-CRCInit-A77B22
    9-LL_START_ENC_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-CRCInit-A77B22
    9-LL_START_ENC_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-CRCInit-A77B22
    9-LL_UNKNOWN_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-UnknownType-01-CRCInit-A77B22
    9-LL_FEATURE_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-FeatureSet-0102030405060708-CRCInit-A77B22
    9-LL_FEATURE_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-FeatureSet-0102030405060708-CRCInit-A77B22
    9-LL_PAUSE_ENC_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-CRCInit-A77B22
    9-LL_PAUSE_ENC_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-CRCInit-A77B22
    9-LL_VERSION_IND-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-VersNr-01-CompId-0203-SubVersNr-0405-CRCInit-A77B22
    9-LL_REJECT_IND-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-ErrorCode-00-CRCInit-A77B22


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
