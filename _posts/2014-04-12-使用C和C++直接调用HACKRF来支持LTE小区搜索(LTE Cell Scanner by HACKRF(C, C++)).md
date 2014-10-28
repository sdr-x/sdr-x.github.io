---
date: 2014-04-12 12:00:00
layout: post
title: 使用C和C++直接调用HACKRF来支持LTE小区搜索(LTE Cell Scanner by HACKRF(C, C++))
thread: 793
categories: LTE
tags:  LTE TD-LTE SDR 4G Beijing Cell-Search Cell-Scanner C C++ HACKRF
---

(原文刊于被sina关闭的我的sina博客) (最新进展参见[完整20MHz带宽配置LTE信号被HACKRF-19.2M采样率成功解析](http://sdr-x.github.io/%E5%AE%8C%E6%95%B420MHz%E5%B8%A6%E5%AE%BD%E9%85%8D%E7%BD%AELTE%E4%BF%A1%E5%8F%B7%E8%A2%ABHACKRF-19.2M%E9%87%87%E6%A0%B7%E7%8E%87%E6%88%90%E5%8A%9F%E8%A7%A3%E6%9E%90/))

软件无线电（Software Defined Radio）以前往往以高（zhuang）大（bi）上（fan）的面目示人，是大公司或学校实验室的高级玩具，一套系统动辄几万甚至几十万，从来都是一个2B（2B = to business）的模式。自从一个芬兰黑客（考证癖出来考证一下）破解了rtl2832芯片的电视棒之后（http://sdr.osmocom.org/trac/wiki/rtl-sdr），SDR终于像树莓派一样普及了。你可以淘宝一个50或60RMB的电视棒，然后给妹子show off跟踪飞机，偷听空管说话，扫描GSM信号，再然后在妹子崇拜的眼神里严辞拒绝出去吃饭的提议，潜心钻研下一个可玩的无线电信号--LTE。

（本文是写给 [http://www.hackrf.net/](http://www.hackrf.net/) 的，已刊载于 [http://www.hackrf.net/2014/04/lte-hackrf/](http://www.hackrf.net/2014/04/lte-hackrf/)
相关demo video的链接在这篇中已经给出: [LTE小区搜索软件加入对HACKRF的支持，有望将来解调LTE SIB信息(Add hackrf support to LTE Cell Scanner)](http://sdr-x.github.io/LTE%E5%B0%8F%E5%8C%BA%E6%90%9C%E7%B4%A2%E8%BD%AF%E4%BB%B6%E5%8A%A0%E5%85%A5%E5%AF%B9HACKRF%E7%9A%84%E6%94%AF%E6%8C%81%EF%BC%8C%E6%9C%89%E6%9C%9B%E5%B0%86%E6%9D%A5%E8%A7%A3%E8%B0%83LTE%20SIB%E4%BF%A1%E6%81%AF%28Add%20hackrf%20support%20to%20LTE%20Cell%20Scanner%29/)
言归正转。

# 一、背景介绍

世界各地的SDR玩家们肯定不会放过前面提到的LTE。比如这个LTE小区搜索和跟踪程序（[https://github.com/Evrytania/LTE-Cell-Scanner](https://github.com/Evrytania/LTE-Cell-Scanner)），它可以用前述的廉价电视棒扫描并解调LTE基站下行PBCH（物理广播信道）信号，获取一些基本的小区信息。

后来中国也宣布上4G了，我想那就git clone下来试一下吧，从而一发而不可收了。先后为这个程序加入了TD-LTE支持（原来只支持FDD模式，居然无视我泱泱大国自主知识产权4G标准！），外部LNB/变频器算法支持，OpenCL加速（单频点扫描速度提高约20倍，缩减到半秒左右）等。以上改进的整个历程和程序在此： [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner) 。折腾历程参见系列博文。

电视棒便宜又好玩，但局限性也很明显：

1. 频率覆盖范围有限。E4K版本覆盖到约2.2GHz（但高端频率接收性能已严重下降），820t版本覆盖到约1.76GHz。国内目前开通的LTE信号最低也在1.8GHz以上，超出820t版的覆盖范围。虽然部分在E4K电视棒覆盖范围内，但E4K版的电视棒早已停产，奇货可居，手里有E4K电视棒的各位且用且珍惜吧！显然，电视棒也玩不了最热闹的2.4GHz频段，除非外接MMDS-LNB或者其他变频器进行频率扩展。

2. 带宽和采样率有限，也就最高2、3M的样子，只能玩玩1、2M带宽以下的信号，对LTE和WiFi的信号带宽就望尘莫及了。所幸按照LTE标准，PBCH信号（基站广播的最基本信息）固定在中心1MHz左右带宽内，刚好还在电视棒的能力之内。但是对于LTE基站广播的更多信息（比如SIB）就无能为力了，因为更多信息就可能分布在超出中心1MHz带宽范围了。

3. 只能收不能发。

4. RF指标一般。毕竟太廉价，以及接收目标是很强的电视广播信号。

虽然电视棒有种种缺点，但它实在太便宜，便宜到价格都快要忽略不计了。世界各地的人们玩的不亦乐乎。各种“打磨”“改装”“提升”方案层出不穷。与此同时，各路大神悉数登场，要为世人打造更强大的开源SDR硬件，同时还要把以往的2B价格（比如USRP）降到2C范围。这其中的明星产品就是BladeRF和HACKRF了，两者都在kickstarter上成功募集到资金，前者已开始交付用户。后者HACKRF据说生产上遇到一些问题，一再跳票，还未开始大规模交付kickstarter上交了钱的用户。NB的是 [http://www.hackrf.net](http://www.hackrf.net) 居然按照HACKRF图纸先造了出来并开始销售！于是就有了我将HACKRF的支持加入LTE小区搜索程序的事情：

中华人民共和国二零一四年三月十五日，就是HFD硬件自由日清华站活动在清华大学五教5102召开讲座的那一天，我独自前往5102聆听大神们布道，遇见王康，前来问我道，“先生可曾用过HACKRF没有？”我说“没有”。他就正告我，“先生还是用一用罢；HACKRF就是为先生这样的人设计的。”
 
# 二、对HACKRF的初步试用感受

介绍具体细节之前，先给出一些直观的试用感受。

先说优点：

1. 覆盖频率范围、支持的带宽和采样率自然是比电视棒高太多了，可玩的信号种类更多，更重要的是比电视棒多了发射（友情提示，国家有法规，发射需谨慎）。HACKRF的配置和功能从电视棒的玩票性质已经进入高大上的专业选手圈子。

2. 灵敏度比我珍藏的绝版E4K电视棒明显的要好，在家里E4K电视棒搜到LTE信号要凭运气，但是用HACKRF基本就每次都能搜到了。

3. 功耗（或者是启动电流？）居然比E4K电视棒要低，这个有些出乎我的意料，毕竟HACKRF的板子看起来比电视棒还是大不少，上面的芯片也多不少。电视棒在Thinkpad T410笔记本上用的时候，如果不用笔记本底座（220V电源供电）上的USB口而直接用笔记本自带的USB口，则经常会驱动报错甚至导致笔记本死机（这个问题困扰我好久，最后发现是供电不足），HACKRF则直接用笔记本本身USB口，无需外接电源，就已经运行的很稳定，这样我笔记本就不用插电源，可以到处跑了。

如果非要说缺点，HACKRF价格比起电视棒当然高了n倍。但对比专业设备和业余设备的价格本身就是没意义的。我感觉HACKRF应该是专业选手里价格最低的了。

另一个美中不足（或者说也是价格所限）是仅支持单天线和半双工。单天线的话就玩不了MIMO和测向之类的了。如果能像BladeRF那样支持双天线(更正：bladeRF也是单天线的，只不过收发分开两根天线而已)，那么就不但可以提高解调信号的质量，并且有可能找出信号的方向从而可能定位目标的物理位置，岂不更有趣。
 
# 三、使用纯C语言操作HACKRF

罗嗦半天，终于要进入正题了。

GNURadio，开源SDR领域最大的生态系统。图形界面，脚本语言，支持各种硬件（USRP，BladeRF，rtl-sdr，HACKRF，etc），软件资源极为丰富，上手快，深得爱好者、学校、研究人员的喜爱。几句命令，几下鼠标，搭个框图便能玩弄空中的信号于掌股之中。

但，今天我们不说GNURadio。

说说如何用纯C/C++程序直接调用HACKRF（或者rtl-sdr电视棒以及其他基于USB的SDR硬件，都应该是类似的）。这样你就可以在别人用鼠标搭框图对各种采样率变换焦头烂额的时候，飞快的敲键盘用VI编写C/C++程序来使用HACKRF，不经意流露鄙视的眼神，深藏功与名。逼格顿时提升几个档次。好（dan）牛（teng）！

以下介绍的都是在Linux下的一些工作。这里假设你对Linux比较熟悉（不太熟悉的，可以看这篇blog中的一些基础操作介绍： [rtl-sdr-rtl2832电视棒跟踪飞机step-by-step教程(tutorial ADS-B aircraft tracking by rtl-sdr rtl2832 gr-air-modes)](http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BAstep-by-step%E6%95%99%E7%A8%8B(tutorial%20ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes)/) ）。

（前方“野程序员”预警！本人不是计算机专业科班出身，本文关于程序方面的内容都是照猫画虎或者频繁试错的结果，未必最优或者最正确，仅供参考，欢迎指正。大言不惭的说本人其实是通信专业科班出身，咳咳。）

## 1）环境的建立

hackrf的程序包是一个典型的cmake包，可以从github上自由下载、编译和安装。以我在Ubuntu12.04系统下的操作为例。

    git clone https://github.com/mossmann/hackrf.git
    cd hackrf/host
    mkdir build
    cd build
    cmake ..
    make
    sudo make install

祈祷你的系统已经安装了hackrf所需的其他程序包，以上过程会非常顺畅和快速。否则cmake步骤和make步骤可能会给一些错误提示，按照提示安装所需的包即可。libusb是必须的，USB接口的SDR设备驱动一般都是基于libusb构建。

最后一步的安装操作（sudo make install）会把其他程序进行API调用所需的头文件以及编译好的二进制库文件放在一些预定义的位置：

头文件：/usr/local/include/libhackrf/hackrf.h

库文件：/usr/local/lib/libhackrf.a，libhackrf.so

其实hackrf源代码包里已经包含了一些有益的参考和例子：

host/libhackrf/src/ 下面是库文件的源代码：hackrf.h; hackrf.c，我们要调用的API的具体实现都在这里。

host/hackrf-tools/src/ 下面是附带的一些小测试程序，比如我主要是通过hackrf_transfer.c学习的如何通过C/C++使用HACKRF。

你可以先玩玩这些小工具。至此，无需庞大的GNURadio，HACKRF已经可以玩起来了，是不是很简单？

## 2）如何使你的代码编译时找到hackrf库

接下来你想让自己的C/C++信号处理程序用上HACKRF，那么首先就是如何让你的代码找到HACKRF库。

直接包含hackrf头文件，并且链接hackrf库当然可以，对于Linuxer怎么弄都成。

如果你的代码工程也是基于cmake构建，也很容易写cmake脚本来使你的代码编译时自动找到hackrf库。以LTE小区搜索程序为例（可以从 [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner) 下载），在cmake/Modules/目录下，写了一个FindHACKRF.cmake的脚本，该脚本会搜索一些预定义的头文件和库文件位置来检测hackrf库的存在。如果你编译时提示找不到hackrf的库，可以看看你的hackrf库是否没有安装在FindHACKRF.cmake搜索的几个位置中，以下摘自FindHACKRF.cmake：

    。。。
    FIND_PATH(HACKRF_INCLUDE_DIR hackrf.h
    ${HACKRF_DIR}/include
    /usr/local/include/libhackrf
    )
    FIND_LIBRARY(HACKRF_LIBRARY
    NAMES hackrf
    PATHS ${HACKRF_DIR}/libs
    "${HACKRF_DIR}\\win32\\lib"
    /usr/pkgs64/lib
    /usr/lib64
    /usr/lib
    /usr/local/lib
    NO_DEFAULT_PATH
    )
    。。。

在代码cmake过程中，顶层脚本CMakeLists.txt会调用FindHACKRF.cmake并且执行相应的操作，比如告诉src/CMakeLists.txt去链接hackrf库。

## 3）调用hackrf的API以及编写回调函数

好了，下面讲解具体的，如何在你的C/C++信号处理代码中调用HACKRF，获取空中的信号样本。

首先介绍一下一般性的流程。

在你的程序中，需要先调用一些初始化API完成最初的设备open、初始化操作。之后调用LNA增益、VGA增益、射频频率、采样率等API按照你的需要进行相关的设置。而后调用接收/发射API（也会把你写的回调函数作为参数告知该API）开始接收/发射。以接收为例，当hackrf驱动接收到USB接口传来的I&Q采样数据时，会自动调用你写的回调函数，你的回调函数中会看到驱动程序传过来的一些信息（通过传入回调函数的参数），比如本次接收到数据的长度、buffer的起始地址之类的信息，根据这些信息，你可以进行相应的处理，比如告诉告诉前台程序：数据来了，拿去处理！

仍旧以LTE小区搜索程序为例进行介绍。

在src/CellSearch.cpp中，函数config_hackrf()完成初始化和初次信号接收的测试。依次调用的hackrf API有（这些API都定义在hackrf.h中，实现在hackrf.c中）：

    hackrf_init(); //初始化
    hackrf_open(&device); //设备打开后，context信息会保存在device结构中
    hackrf_set_sample_rate_manual(device, 1.92e6, 1); //设置采样率为1.92MHz，该数值和LTE信号标准相关。
    hackrf_set_baseband_filter_bandwidth(device, 1.45e6); //设置基带滤波器带宽1.45MHz，虽然我们要解调的LTE PBCH信号带宽只有1MHz左右，但这里留了一些余量以防有较大的频偏导致信号被推到滤波器边缘造成损伤。但现在看来没太大必要，因为我拿到的hackrf的板子频偏很小，20PPM以下吧。
    hackrf_set_vga_gain(device, vga_gain); //设置可变增益放大器VGA的增益。最大值62.（hackrf.c）
    hackrf_set_lna_gain(device, lna_gain); //设置低噪放LNA的增益。最大值40（hackrf.c）. vga和lna的增益好像不接受任意值，而只能是某些组合。为了提高灵敏度，最好把较大的增益分配给lna，用vga增益来调整I&Q信号幅度大小（目的是减小量化误差），vga增益也不能过大，过大会有切顶失真。对于接收机来说AGC是一个很重要的功能，程序和驱动还待改进。
    hackrf_set_freq(device, fc); //设置中心接收频率为fc，单位Hz。

然后调用信号接收子函数capture_data()，进行初次接收测试。信号接收子函数capture_data()实现在src/capbuf.cpp中。具体代码：

    hackrf_stop_rx(hackrf_dev); //中止接收，因为可能之前处于接收使能状态。
    hackrf_rx_count = 0; // 接收数据长度清零，这是一个全局变量，回调函数会用此变量来告诉前台程序现在接收到多少数据了，前台程序应该在每次接收前把此变量清零，这样回调函数下一次被驱动调用时，才会认为用户buffer是空的，从而把驱动收到的数据搬进用户buffer（其实是个全局数组）
    hackrf_start_rx(hackrf_dev, capbuf_hackrf_callback, NULL); // 启动接收，并且告诉驱动回调函数为capbuf_hackrf_callback
    while(hackrf_is_streaming(hackrf_dev) == HACKRF_TRUE) { //不断检测HACKRF设备是否处于正常streaming状态
    if( hackrf_rx_count == (CAPLENGTH*2) ) //如果接收数据长度达到了我们预订长度CAPLENGTH即退出，*2是因为每个样点有I和Q两个数据
    break;
    }
    hackrf_is_streaming(hackrf_dev); //再次调用此函数

本次接收至此完成，接收的CAPLENGTH*2数据存在全局数组hackrf_rx_buf中了。程序可以进一步处理接收到的数据了。数据格式很简单，就是I和Q交替存储，均为有符号8bit整数（因为hackrf目前最新的CPLD程序已经做了无符号有符号变换，因此我们在host程序里看到的已经是处理完的有符号数，信号处理可以直接用了。当然其中可能有一些RF的非理想因素，比如DC offset， IQ imbalance之类的，这些都是信号处理可以用算法去搞的了）

下面介绍回调函数。

回调函数除了把接收数据总长度更新到全局变量hackrf_rx_count中外，还会把接收数据存储到全局数组
int8 hackrf_rx_buf[CAPLENGTH*2];
中。回调函数capbuf_hackrf_callback(hackrf_transfer* transfer)也实现在src/capbuf.cpp中。当回调函数被驱动自动调用时，驱动本次接收数据的长度会在transfer->valid_length中，而数据内容会在transfer->buffer中。注意，回调函数不是驱动的一部分，而是用户自己根据需要写的用户程序，然后通过hackrf_start_rx()这个API告知驱动，每当驱动收到数据时就会自动调用回调函数（你可以类比中断服务程序）。

回调函数及其注释说明：

    static int capbuf_hackrf_callback(hackrf_transfer* transfer) {
      size_t bytes_to_write; //本次调用向全局数组hackrf_rx_buf写入的长度
      size_t hackrf_rx_count_new = hackrf_rx_count + transfer->valid_length; //初步计算本次接收后数据总长度。
      int count_left = (CAPLENGTH*2) - hackrf_rx_count_new; //计算期望接收总长度与本次接收后总长度的差值。
      if ( count_left <= 0 ) { //如果本次接收后，总长度超出期望接收总长度
        bytes_to_write = transfer->valid_length + count_left; //则要减去超出的长度，否则待会儿hackrf_rx_buf装不下这么多数据会越界。
      } else { //如果不超
        bytes_to_write = transfer->valid_length; //则本次收到的数据可以全写入hackrf_rx_buf
      }
      if (bytes_to_write!=0) //如果需要把数据写入hackrf_rx_buf，即hackrf_rx_buf还没满
      {
        memcpy( hackrf_rx_buf+hackrf_rx_count, transfer->buffer, bytes_to_write ); 把数据从驱动的buffer中搬到用户全局数组hackrf_rx_buf中（附加在之前的数据之后）
        hackrf_rx_count = hackrf_rx_count + bytes_to_write; //更新当前接收数据总长度。这个回调函数具有“自锁”能力，即接收数据总长度达到期望的CAPLENGTH*2后，即使被驱动调用，也不会再往用户全局数组hackrf_rx_buf中写了，因为已经满了。
      }
      return(0);
    }

以上就是使用C/C++直接操作HACKRF的方法。

当然不能算是我完全原创，只是照着之前的LTE小区搜索程序作者James Peroulas对rtl-sdr驱动的调用方法，依葫芦画瓢做的。

和James Peroulas有过几次交流，得知他居然还在深圳的中兴工作过很长时间，后来去了硅谷那边的Intel，现在在那边专门做SDR系统的range network公司工作（据说是做OpenBTS的人创建的，有兴趣的可以考证一下）。在无线通信信号处理和编程方面都有非常非常丰富的经验。通过学习和改进他的代码，真的学到了很多很多东西。所以，想深挖的，强烈推荐读代码并且写代码做实验。
 
## 四、未来工作展望

有了HACKRF这样的专业选手，尝试解调更大带宽内的LTE信息成为一个自然的尝试。

其实这方面的开源的LTE代码也有一些：

[https://github.com/kit-cel/gr-lte](https://github.com/kit-cel/gr-lte)

[http://sourceforge.net/projects/openlte/](http://sourceforge.net/projects/openlte/)

可以“站在巨人的肩膀上”开始起跳。

如果你也对继续改进这个LTE搜索程序 [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner) 有兴趣，可以查看它的TODO，一起来HACKRF！

