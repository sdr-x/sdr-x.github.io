---
date: 2012-12-27 12:00:00
layout: post
title: rtl-sdr-rtl2832电视棒跟踪飞机step-by-step教程(tutorial: ADS-B aircraft tracking by rtl-sdr rtl2832 gr-air-modes)
thread: 56316
categories: ADS-B
tags:  rtl-sdr rtl2832 SDR ADS-B plane-track aircraft-track gr-air-modes step-by-step
---

(原文刊于被sina关闭的我的sina博客)

# 背景1:

前段日子用电视棒成功跟踪到了飞机(见这篇跟踪结果的博文:[rtl-sdr-rtl2832电视棒跟踪飞机(ADS-B aircraft tracking by rtl-sdr rtl2832 gr-air-modes)](http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BA%28ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes%29/)).不少人问有没有windows下的软件.业余无线电爱好者中目前看来熟悉linux的看起来不多.其实这是一种遗憾,因为开源世界里关于业余无线电的资源非常多.

# 背景2: 

虽然玛雅人预测的世界末日并没有发生,但毕竟2012还没有过去,一切皆有可能.趁现在一切还正常,我赶紧再做点贡献吧. 2012做个了解, 迎接新的2013. 躲过末日,大家一起看灰机!

目的:尝试详细的描述如何使用电视棒跟踪飞机,使得之前没怎么接触过linux的网友也能实验成功.

提醒:玩linux其实有时候很折腾,有时会遇到这样或那样的问题,比如就是编译报错不成功之类的.这很正常.会比较考验人的抗挫折能力,以及百折不挠的精神.但业余无线电爱好者大都具有折腾精神.相信问题不大.

声明:本人连呼号都没有,算是个伪业余无线电爱好者.本文纯描述技术how to,请想要实践的各位务必遵守相关无线电法规,后果自负.

(更正，写这篇博文时俺还没呼号。但现在有了：BH1RXH)

btw1: 转载本文请注明出处--本blog.

btw2: 本文内容全部来自互联网,主要是以下网站: [http://sdr.osmocom.org/trac/wiki/rtl-sdr](http://sdr.osmocom.org/trac/wiki/rtl-sdr) 及其内嵌的链接.因此本文内容的大部分并非原创,主要是重复了别人做过的事情并试图翻译和综合.喝水不忘挖井人,这里要向开源精神致敬.向Richard Stallman和Linus Torvalds致敬.

进入正题.

首先欢迎各位看完后提问和报告问题,我会不断补充完善此博文.

# 0. 原理概述

之所以能够很容易的跟踪飞机,是因为航空CNS(通信导航监视)系统里大量采用非常古老的无线标准.因为航空业巨头们建立了一整套适航规定,飞机上任何一点小小的改动若想获得广泛的应用是非常麻烦的,更不要说对CNS系统的升级换代. CNS系统中大量采用脉冲体制以及明文传输,因此我们得以很容易的监听飞机以及地面站发射的信号,并解码.脉冲体制的含义是,通信的基本方式是大功率的脉冲,瞬间功率上千瓦,持续us量级,信息承载在脉冲的位置和相对强度中.

比如二次监视雷达(SSR)系统,地面站发射1030MHz的查询信号,飞机接收到此信号之后在1090MHz发射应答信号,信号中包含了飞机的一些信息,显示在空管的雷达屏幕上.还有空中防撞系统(TCAS),飞机可以自己发射1030MHz的查询信号,其他飞机接收到此信号之后在1090MHz发射应答信号,因此一架飞机得以"看到"周围的飞机.由于以上的查询-应答模式在飞机很多的时候显得效率不是那么高(碰撞,干扰等等),因此新出现了一种ADS-B方式.在ADS-B中,每架飞机不等查询,主动广播自己的信息,这时监视和防撞需要做的就仅仅是接收了.在通用航空当中ADS-B信号经常在978MHz发射.在商业飞行中ADS-B信号经常在1090MHz发射(和SSR和TCAS发射频率相同,即复用物理层数据链).(通用航空可类比与私家车,商业飞行可类比与公交车长途大巴). 欲知这些CNS系统详情,请在en.wikipedia.org搜索Secondary surveillance radar, TCAS,ADS-B等.

电视棒为何能接收飞机发射的信号?本来他们看起来风马牛不相及的.首先电视棒是能收电视信号的(废话),但能收电视信号不意味着就能收飞机.得益于微电子技术和通信技术的飞速发展,目前接收电视信号(包括模拟电视和数字电视)一般由两颗芯片来完成,一颗称之为tuner,另外一颗为解调器.tuner的作用是将指定频率和带宽的信号进行放大,下变频,滤波等,送给解调器芯片.解调器芯片首先把信号进行A/D采样数字化,而后根据想要接收的电视制式进行相应的运算处理,输出视频码流,声音,数据等电视台播发的信息.

因为各种系统的工作频率不同,生产tuner的厂家为了使自己的芯片能够一次投产多次使用长期销售,他们将tuner芯片设计的能覆盖很宽的频带并且能设置不同的接收带宽.而后根据用户需求和政府的无线电规则在不同的产品里开启某些频段,禁用某些频段. 如果tuner被破解,那么就有办法开启它所有频段的接收能力,包括在1090MHz上接收信号.

剩下的就是解调器,电视棒里的解调器当然无法解调飞机发射的信号,但解调器当中对tuner送来的信号进行A/D采样,这个采样功能是解调任何信号所必须的,当然也可以用来解调飞机发射的信号.通过破解电视棒的driver,tuner的信号被解条器进行A/D采样数字化之后被直接通过USB接口送给电脑,使得我们能在电脑上处理原始的tuner信号采样,即用电脑软件担当解调器的任务.加之航空CNS标准是公开的(脉冲位置,相对幅度等),信息是明文的,因此编制解调软件即可正确解调飞机发射的信号了.

以上用计算机软件进行通信信号解调的方法就是所谓的"软件无线电"(Software Defined Radio -- SDR). 实际上软件无线电技术的研究和开发已经有几十年的历史了,最初源于美军的多制式电台项目. 目前我们日常使用的移动通信系统中其实已经大量使用软件无线电技术, 比如基站中的信号处理大量的使用可编程的FPGA和DSP完成, 比如手机当中的基带处理器也越来越多的采用软解调的方法(少数运算量特别大实时性要求特别高的模块除外,比如turbo解码器,扩频相关器等,这些模块往往在基带处理器中嵌入一些高度定制化"硬"核来实现)

# 1. 准备工作
## 1.1 电视棒

首先需要得到一根支持rtl-sdr的电视棒(即有破解驱动的电视棒). 有很多种支持rtl-sdr的电视棒,他们的共同特点是采用了RTL2832U解调芯片.因此此类基于电视帮的SDR玩法统称为rtl-sdr. 支持如下几种tuner芯片:

Tuner|芯片|频率范围
---------|---------|-------------
Elonics| E4000    | 52 - 2200 MHz, 其中1100 MHz to 1250 MHz无法覆盖
Rafael Micro |R820T     |24 - 1766 MHz
Fitipower |FC0013     |22 - 1100 MHz
Fitipower |FC0012     |22 - 948.6 MHz
FCI |FC2580     |146 - 308 MHz, 438 - 924 MHz 

其实以上关于rtl-sdr信息可以在如下网站找到: [http://sdr.osmocom.org/trac/wiki/rtl-sdr](http://sdr.osmocom.org/trac/wiki/rtl-sdr)
那个网站上给出了网友汇总的所有支持的电视棒型号:
[http://www.reddit.com/r/RTLSDR/comments/s6ddo/rtlsdr_compatibility_list_v2_work_in_progress/](http://www.reddit.com/r/RTLSDR/comments/s6ddo/rtlsdr_compatibility_list_v2_work_in_progress/)

据说E4000 tuner已经停产,以后电视棒大量采用R820T tuner. 淘宝上直接搜索电视棒即可（注意现在 **搜索电视棒你不会得到任何结果** 搜索rtl-sdr兴许可以）

## 1.2 linux操作系统

这一点可能是广大业余无线电爱好者最不熟悉的一部分.这里稍微多介绍一下.不过先要提醒一下各位,linux操作系统往往需要花时间和功夫"折腾"才能用的好,因此一定要有心里准备.当然也不排除你人品非常好,顺利的装好就能用.
Linux是一种操作系统的统称(我们常见的另一些操作系统就是windows了,还有mac的操作系统,手机里的android等). linux它有着很多发行版.所谓发行版就是面向最终用户的一个能非常方便的安装,升级,界面友好,并集成了一些自己特色功能的版本.具体到物理形式就是一张光盘,或者光盘镜像文件,或者远程服务器上存储的映像. 我使用的发行版本是Ubuntu 12.04 LTS. 此时此刻我正在笔记本上的Ubuntu操作系统下在Chromium浏览器中写这篇blog,也就是说我日常工作娱乐基本都使用Ubuntu完成,有些不得不使用Windows的场景(比如网银),我便会临时进入Ubuntu里安装的VMware player虚拟机,启动里面的windows进行操作.
关于Linux的介绍和安装你可以搜索到太多的资源和教程了(而且是中文的),这里不再一一重复.相信你在搜索相关资源和安装的过程中会对开源和无私分享的精神有所体会,这种精神也激励大家(包括我)主动去分享更多的东西出来,只有人人奉献才能保证将来个人索取的时候能找到想要的东西.

这里给出一些可能遇到的问题的tips:

**无法安装/启动图形界面.**

往往是因为显卡太新或者太偏门或者厂家支持的不好导致的.这时你需要在命令行下安装显卡驱动.

假如你使用Nvidia显卡,启动至命令行之后,输入:

    sudo apt-get install nvidia-

然后按tab键会显示出可已自动补齐的那些选项. 比如可以选择nvidia-current安装目前支持的驱动,即输入:
    
    sudo apt-get install nvidia-current
    
安装过程中,提问回答y即可.

这里提供了一种通用的安装软件发方式apt-get install 软件名字,以及敲出部分软件名字按下tab可以自动补齐. 前面的sudo表示以root权限进行安装,可能会提示你输入root密码.

**进入Ubuntu图形界面后如何启动命令行**

Ubuntu也提供了类似windows的开始菜单run的功能,例如用鼠标点击相应的菜单,或者按下windows键后在输入框中输入terminal,而后回车即可启动命令行.因为后续几乎所有操作需要在命令行完成,所以务必进入命令行.

**一些基本概念**

根目录: 最顶层目录,你可以打开Ubuntu下的"资源管理器"(一个类似windows文件夹的图标),在左侧栏点击File System即可访问根目录.

Home目录: 在命令行中可以用~表示home目录,即你当前帐号的"家"目录.如果你帐号为xxx,那么home目录一般为/home/xxx, 或者资源管理器左侧栏点击Home

**Ubuntu中安装软件的三种方式**

这里首先提醒各位,linux中安装软件一般不像windows直接运行安装程序一路next即可.往往一个软件需要预先安装一堆依赖软件.因此学会寻找软件和安装软件至关重要.

除了之前介绍的在命令行下使用apt-get install 安装之外,还有两个常用的图形界面app store. 一个是Ubuntu Software Center,一般Ubuntu已经自带装好.如果没有,请使用sudo apt-get install software-center 进行安装. 在Ubuntu Software Center中很容易通过输入框搜索想要的软件,并点击安装.(可能会要求输入root密码).

另外一个图形界面app store是synaptic, 可以在命令行或者开始菜单键入synaptic启动, 键入搜索,并选择想要安装的软件包进行安装.

平时进行各种编译/安装操作时要注意看错误提示,如果是因为缺少某软件包,一般会在提示当中给出,安装那些软件包后从新来一遍即可.

以上各安装手段一般不可同时运行,即同一时间只允许有一种按转方式运行,否则会提示一些"操作无法完成"之类的.

再次提醒,任何以上软件使用问题可以先以相关的关键字搜索,你会发现大量的解答/资源.

## 1.3 飞机以及接收位置

不需要去买一架飞机.只要保证接收地点周边一两百公里内有飞机飞过即可.现在航线很密集,基本上中东部地区都满足此要求. 关于接收位置,因为飞机发射的信号脉冲都很强(千瓦-us量级),因此对开阔与否要求不高.我在14层楼房把天线放在窗台的玻璃外纱窗内都能接收到很多信号.

# 2. 飞机信号解调

## 2.0 安装GNURadio

GNURadio是开源界最大或者说是No.1的软件无线电项目集大成者,包含了非常多的软件无线电功能.

rtl-sdr的正常运行也需要GNURadio(版本>= v3.5.3).

GNURadio项目网站: [http://gnuradio.org](http://gnuradio.org)

GNURadio安装有人做了一个脚本,非常方便,只需要下载该脚本并在命令键入该脚本名字运行即可（方法可能会过时，注意去gnuradio网站查看最新方法）:

打开命令行终端,

    mkdir src
    
建立一个src目录你希望gnuradio源代码存储于此,

    cd src
    wget http://www.sbrac.org/files/build-gnuradio && chmod a+x ./build-gnuradio && ./build-gnuradio (这是一整行)
    
如果下载有问题的话请检查代理配置等.

可以通过以下命令行查看代理服务器设置:

    echo $http_proxy
    echo $https_proxy

可以通过以下命令设置代理服务器:

    export http_proxy=ip地址
    export https_proxy=ip地址

如果不需要代理服务器，则ip地址以空格代替。

也可以在UI下更改代理设置，具体是找到鼠标找到 System Settings --> Network --> Network proxy进行设置。

有防火墙的网络可能会无法下载成功，这时应该考虑关闭防火墙，或者更换一个没有防火墙的网络（比如从公司换到家里）。

漫长的等待(同时祈祷不会中途出错),GNURadio就安装完毕了.

## 2.1 rtl-sdr软件安装及测试

(以下内容大都来自[http://sdr.osmocom.org/trac/wiki/rtl-sdr](http://sdr.osmocom.org/trac/wiki/rtl-sdr)及其相关链接)

在安装真正的飞机信号解调软件之前，需要首先安装驱动（也就是破解的控制软件），以便解调软件能够顺利获取解调器中的A/D采样值。

进入命令行.

进入home目录:

    cd ~

下载git软件（这是个版本管理软件，用于从远端版本仓库中获取rtl-sdr软件至本地）:

    sudo apt-get install git

使用刚才安装的git软件下载rtl-sdr驱动软件: (进入你想放rtl-sdr源码的目录后)

    git clone git://git.osmocom.org/rtl-sdr.git
    
一般会显示:

    Cloning into 'rtl-sdr'...

以及一些网络下载进度...

最后下载完毕. 源代码自动存放于自动新建的rtl-sdr目录.

如果下载进度很久不动,或者下载不成功一般和系统的代理服务器设置或者防火墙有关.

版本管理软件除了git之外常用的还有snv,bzr等,如果你想尝试更多的开源项目,也要粗通这些版本管理软件才行. 很多时候你拿到软件的链接,上网页一看是没办法直接下载的,这时候一般都需要通过各种版本管理软件下载.

进入已经存放源代码的rtl-sdr目录,首先看README,(这是好习惯), 看的方法可以是命令行敲

    gedit README &

打开后,你会发现它给出了网页链接,接下来我们按照网页链接上的内容来编译安装rtl-sdr

你需要预先安装libusb1.0开发包! (还记得介绍过的安装软件的几种方法吗?)

    apt-get install libusb-1.0-0-dev

如果之前安装过rtl-sdr,记得先进入rtl-sdr/build目录运行make uninstall进行卸载.

rtl-sdr安装过程如下(依次敲命令,看各个命令是否都成功运行)

**方法1:**

    cd rtl-sdr/ (如果已经在rtl-sdr目录下,则该步省略)
    mkdir build
    cd build
    cmake ../ -DINSTALL_UDEV_RULES=ON
    make
    sudo make install
    sudo ldconfig

**方法2:**

    cd rtl-sdr/ (如果已经在rtl-sdr目录下,则该步省略)
    autoreconf -i
    ./configure
    make
    sudo make install-udev-rules
    sudo ldconfig

可执行文件在rtl-sdr/src/目录下.

接下来还需要把rtl-sdr作为组件安装到gnuradio中,方法大同小异:

首先进入某个你想放源代码的目录,然后

    git clone git://git.osmocom.org/gr-osmosdr
    cd gr-osmosdr/
    mkdir build
    cd build/
    cmake ../
    make
    sudo make install
    sudo ldconfig

现在可以测试电视棒是否正常.

首先将电视棒插入电脑usb口.

可以运行的命令有 (在命令后加上 --help 可以看到命令帮助)

    rtl_eeprom

一般会显示你的电视棒的信息

    rtl_sdr -f xxx -s yyy -g zzz -n aaa
    xxx是目标频率,单位Hz
    yyy是目标采样率,单位Hz,缺省2048000
    zzz是增益,单位dB,缺省0自动增益
    aaa是读多少个样值,缺省是0无限样值,这个最好设置一下,不然如果有问题没法推出来,可能需要重启电脑.
    
    rtl_test -s yyy -t -p
    yyy是目标采样率,单位Hz,缺省2048000
    -t 测试E4000 tuner性能, 比如频率覆盖范围等等 R820T tuner不支持此测试.
    -p 测试采样时钟PPM误差
    -s -t 和 -p请各自单独使用.
    使用-s加不同的采样率运行,注意观察有没有丢包提示,能够找到最大不丢包采样率.

rtl_fm可以用来听FM广播, 加上--help运行,会看到听广播方法

rtl_tcp没用过,你可以用--help自己研究一下

看起来这个是用来把解调数据转发到网络上的,有兴趣的各位其实可以diy很多个瘦设备,放到全国各地监测天空(或各地放网友自发),做一个属于我们自己的"飞常准"

## 2.2 解调软件安装及使用

电视棒及其破解的驱动就绪了,接下来安装飞机信号解调软件.

打开 [https://github.com/bistromath/gr-air-modes](https://github.com/bistromath/gr-air-modes)

你可以看到该软件的git链接: [https://github.com/bistromath/gr-air-modes.git](https://github.com/bistromath/gr-air-modes.git)

打开命令行,进入一个你希望软件源代码存放的目录,然后

    git clone https://github.com/bistromath/gr-air-modes.git
    cd gr-air-modes
    gedit README & 查看安装方法

在README中给了需要预安装的软件包:

    =================================
    REQUIREMENTS
    
    gr-air-modes requires:
    
    * Python >= 2.5 (written for Python 2.7, Python 3.0 might work)
    ** NumPy and SciPy are required for the FlightGear output plugin.
    * Gnuradio >= 3.5.0
    * Ettus UHD >= 3.4.0 for use with USRPs
    * osmosdr (any version) for use with RTLSDR dongles
    * SQLite 3.7 or later
    * CMake 2.6 or later

其中NumPy and SciPy are required for the FlightGear output plugin.和Ettus UHD >= 3.4.0 for use with USRPs我们这里暂时用不到.osmosdr (any version) for use with RTLSDR dongles和Gnuradio前面安装好了. 其他软件包请按照之前介绍的各种软件安装方法自行搞定(如果在接下来的安装过程中有相关的错误)

接着编译安装:

    mkdir build
    cd build
    cmake ../
    make
    sudo make install
    sudo ldconfig

成功执行后,飞机解调软件的可执行文件已经安装于/usr/local/bin

## 2.3 解调数据/飞机轨迹的可视化

如果想详尽研究该解调软件的用法,请命令行输入modes_rx --help

如果现在就想接收飞机信号,那么请靠近窗口,天线尽可能靠近屋外,命令行运行:

    modes_rx --gain=60 --output-all --rtlsdr --kml=xxx.kml

可以用--gain调整增益,我的经验是增益高一些接收能力强一些。

xxx.kml是把接收到的飞机航班号位置高度信息等存为kml文件的文件名.

正常情况下你应该从命令行打印能实时看到很多飞机的信息了.收集一会儿之后,关掉程序(ctrl + C), 把那个kml文件导入google earth或者其他能导入kml文件的地图网站/软件,就能看到飞机轨迹了.

linux下一个看kml的程序是gpsprune, 请自行安装. apt-get, 软件中心, synaptic都行.

可以把kml导入地图查看的一个网站是 [gpsies.com](gpsies.com).

3. 结束语

至此,如果顺利的话,相信你也成功的监测到了飞机,切身感受到了软件无线电的强大. (这里夹带一点私货:个人觉得开源软件运动配合软件无线电在将来会掀起一场无线信息共享/交换的革命,就像Linux那样对人类社会产生深远的影响)

rtl-sdr还能玩很多其他东西(不就是软件嘛,无所不能),如果你仔细看这个[http://sdr.osmocom.org/trac/wiki/rtl-sdr](http://sdr.osmocom.org/trac/wiki/rtl-sdr)的话,你会发现一些别人已经做或者正在做的工作(你也可以申请加入一起搞):

**GNURadio** 这是软件无线电目前在开源世界的大本营,你可以认为是非常多软件无线电功能的合集,它支持至少两种硬件,一个是rtl-sdr,另外一个是USRP: [http://www.ettus.com/](http://www.ettus.com/) USRP对于业余玩玩来说挺贵的,因此主要是大学以及工业界的一些lab在用. rtl-sdr则亲民许多了.

**多模接收机** multimode RX [https://www.cgran.org/browser/projects/multimode/trunk](https://www.cgran.org/browser/projects/multimode/trunk) 就是各种也余无线电接收方式.

**FM接收** simple_fm_rvc [https://www.cgran.org/browser/projects/simple_fm_rcv/trunk](https://www.cgran.org/browser/projects/simple_fm_rcv/trunk)

**瀑布图扫频** rtlsdr-waterfall [https://github.com/keenerd/rtlsdr-waterfall](https://github.com/keenerd/rtlsdr-waterfall)

**一个非常受欢迎的SDR多模接收软件(尤其在windows下受欢迎) SDR#** [http://sdrsharp.com/](http://sdrsharp.com/) and  Windows Guide or  Linux Guide

**tera集群无线电接收** tetra_demod_fft osmosdr-tetra_demod_fft.py and the  HOWTO

**GSM监听** airprobe [http://git.gnumonks.org/cgi-bin/gitweb.cgi?p=airprobe.git](http://git.gnumonks.org/cgi-bin/gitweb.cgi?p=airprobe.git)

**GPS接收机** GNSS-SDR Documentation and  [http://www.gnss-sdr.org](http://www.gnss-sdr.org)

**LTE小区搜索 LTE Scanner/Tracker** [http://www.evrytania.com/lte-tools](http://www.evrytania.com/lte-tools) [https://github.com/Evrytania/LTE-Cell-Scanner](https://github.com/Evrytania/LTE-Cell-Scanner)

**全模式(支持TD-LTE和外置LNB)LTE小区搜索及OpenCL加速** [https://github.com/JiaoXianjun/LTE-Cell-Scanner](https://github.com/JiaoXianjun/LTE-Cell-Scanner)

**集成进MATLAB/SIMULINK MATLAB/Simulink wrapper**,想干什么都行了. [http://www.cel.kit.edu/simulink_rtl_sdr.php](http://www.cel.kit.edu/simulink_rtl_sdr.php)

**无线电扫描** gr-scan [http://www.techmeology.co.uk/gr-scan/](http://www.techmeology.co.uk/gr-scan/)

**射频校准** kalibrate-rtl  [https://github.com/steve-m/kalibrate-rtl](https://github.com/steve-m/kalibrate-rtl),  Windows build

**多信道试试接收机** Multichannel Realtime Decoder [https://github.com/iZsh/pocsag-mrt](https://github.com/iZsh/pocsag-mrt)

**另外的一个ads-b接收机** [http://sdrsharp.com/index.php/a-simple-and-cheap-ads-b-receiver-using-rtl-sdr](http://sdrsharp.com/index.php/a-simple-and-cheap-ads-b-receiver-using-rtl-sdr)

希望这篇文章给你打开了一扇大门. (通向哪里就看你了)

再次提醒各位,信息的共享,交流,发布务必遵循相关无线电和航空安全法规.

祝你好运. 
