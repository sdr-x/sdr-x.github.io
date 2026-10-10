---
date: 2026-10-10 00:01:00
layout: post
title: "Has Starlink Open-Sourced Its Linux Kernel? GPL Compliance Explained"
thread: 2026101005
categories: starlink
tags:  Starlink Linux GPL Apache PREEMPT_RT OpenWRT Cisco Linksys Dishy firmware reverse-engineering Software-Defined-Radio SDR FPGA AD9361
---

Starlink terminals run Linux. Under the GPLv2 license, which governs the Linux kernel, SpaceX is generally required to provide the corresponding source code to users who receive the terminal running covered software, subject to the license's specific conditions.

More than 20 years ago, people discovered that Linksys/Cisco Wi-Fi routers ran Linux. The resulting legal pressure forced the companies to release the corresponding source code. This helped pave the way for the OpenWRT project. Today, many Wi-Fi routers are built on Linux and OpenWRT. In principle, you can request the corresponding source code from a manufacturer. If it fails to comply with its GPL obligations, legal action may be an option to enforce those rights.

So, does Starlink comply with GPLv2 and responsibly release its Linux source code?

The answer is yes. However, the publicly released version lags significantly behind the versions used in widely deployed Starlink terminals.

1 . What has SpaceX open-sourced?

On its official website, Starlink states on the Software License & Usage Terms page that its terminals contain open-source software. The page includes the following statement:

You may request access to the code for our Open Source Components through your Starlink account.

In addition to allowing Starlink customers to request source code, SpaceX has published code on GitHub under its official account, SpaceExplorationTechnologies.

Its public repositories include:

- OpenWRT: Code for two generations of Starlink Wi-Fi routers.
- U-Boot: The Linux bootloader, licensed under GPLv2.
- enterprise-api: API interfaces for enterprise customers, such as maritime and aviation customers, etc.
- sssd: A Linux device management and configuration component, authored by Chadwick Banning and licensed under Apache 2.0.
- linuxptp: Software for high-precision time synchronization, licensed under GPLv2. Time synchronization is particularly important because Starlink terminals and the network reportedly change configurations approximately every 15 seconds.
- Linux kernel: A version of the Linux kernel used by the Starlink modem (confidense 98.1%), licensed under GPLv2.

2 . Which Linux kernel is relevant to the Starlink modem?

The Starlink terminal, also known as Dishy, can be compared to a portable 5G Wi-Fi hotspot. The main difference is that the 5G cellular modem connecting to terrestrial base stations is replaced by a phased-array antenna and modem that communicate with Starlink satellites.

The Wi-Fi subsystem in both 5G Wi-Fi hotspot and Starlink terminal can function as a conventional OpenWRT-based router. However, the OpenWRT Wi-Fi subsystem is not the focus of this article.

The key component is the satellite modem, which runs Linux. The kernel source code discussed above is believed, with approximately 98.1% confidence, to correspond to this modem.

SpaceX has published two versions of its modem Linux kernel modifications that can be found on their github linux repository:

- sx_2022_04_29
- sx_2020_11_25

The sx prefix presumably stands for SpaceX, while the suffix indicates the release date.

The corresponding U-Boot releases have similar dates:

- sx_2022_05_03
- sx_2020_11_25

The linuxptp repository also has a release named sx_v2020_08_06, dated close to the earlier releases above.

Analysis of the newer kernel release, sx_2022_04_29, indicates that the modem is named Catson, and SpaceX's modifications are based on Linux kernel version 5.10.90.

3 . What have reverse engineers discovered?

There have been three notable rounds of publicly available reverse engineering of the firmware  running on Starlink terminals.

3 . 1 KU Leuven researchers, 2021

In 2021, researchers at KU Leuven in Belgium published reverse-engineering findings for the U-Boot bootloader and Linux in the first- or second-generation circular Starlink terminal.

The bootloader version was:

2020.04-gddb7afb

This predates SpaceX's sx_2020_11_25 release.

The Linux kernel image was created on:

2021-04-16 21:10:45 UTC

This timestamp falls between the two SpaceX kernel releases, sx_2020_11_25 and sx_2022_04_29. The exact kernel version was not identified.

3 . 2 Quarkslab, 2023

In 2023, Quarkslab published firmware reverse-engineering findings covering the V2 circular terminal and the V3 rectangular terminal.

Their analysis identified the Linux kernel in the firmware as version 5.15.55, whereas the kernel version published by SpaceX was 5.10.90.

This shows that by 2023, researchers had already identified a substantial gap between SpaceX's publicly released kernel source and the newer kernel version found in actual terminal firmware.

3 . 3 Starlink-Research, late 2024

The most recent firmware reverse-engineering findings discussed here were published by the GitHub account Starlink-Research in late 2024. They targeted the V4 terminal.

The firmware contained the following Linux kernel build information:

Linux version 6.1.38-rt12 (buildroot@buildroot) (aarch64-sxbr-linux-musl-gcc.br_real (Buildroot -ga67e963) 12.3.0, GNU ld (GNU Binutils) 2.39) #1 SMP PREEMPT_RT Fri Jun 21 16:20:33 UTC 2024

Several details are worth noting.

First, the kernel is based on Linux 6.1.38, with an -rt12 suffix indicating a PREEMPT_RT patch level. The base version 6.1.38 is way newer than the one SpaceX published on github.

Second, the PREEMPT_RT designation strongly suggests that SpaceX uses a real-time-patched Linux kernel. Such patches reduce scheduling latency and improve the predictability of task execution.

This makes sense for satellite communication equipment, where precise timing and predictable response times can be important.

A useful historical detail: PREEMPT_RT had been under development for roughly 20 years before its integration into the mainline Linux kernel in September 2024, with Linux 6.12. Before that integration, obtaining the full PREEMPT_RT functionality generally required applying the external patch set to a compatible kernel.

The Starlink V4 kernel was built in June 2024, before Linux 6.12 was released. Its use of PREEMPT_RT patches is therefore consistent with the state of Linux development at that time.

If future Starlink terminals use Linux 6.12 or a newer kernel, they may no longer need a separate PREEMPT_RT patch set to obtain the functionality incorporated into the mainline kernel. Because the exact configuration and enabled features will be in the kernel natively.

4 . An interesting detail: Catapult

By the way, the well-known engineer Oleg discovered a modem named Catapult in the Starlink firmware. It appears to be a newer modem generation than Catson.

This is another indication that Starlink's modem hardware and software continue to evolve, while the kernel source code publicly released by SpaceX may represent an older generation.

![](../media/Gemini_Generated_Image_mhc897mhc897mhc8.jpeg)

Conclusion

SpaceX does publish Linux kernel source code for Starlink terminal modem, and its public repositories include several other open-source components used in the Starlink ecosystem.

However, the publicly released modem kernel is based on Linux 5.10.90, while reverse-engineered firmware has revealed Linux 5.15.55 in earlier terminals and Linux 6.1.38 with PREEMPT_RT patches in the V4 terminal.

The central question is therefore not simply whether Starlink releases Linux source code, but whether the source code made available to users corresponds to the kernel versions actually shipped in their terminals, as required by the applicable GPLv2 obligations.

You might argue that only the dishy user can request the source code access, so most of the poeple (non dishy user) can't see the code publicly on github. That is possibly true. But personally I believe that the possibility is quite low. The reason is that: 
the dishy user who receives the source code has rights to re-distribute it again according to GPLv2. So, if any dishy user has received the source code from SpaceX, we might already see it on internet when some user re-distribute it on internet according to the 
GPLv2 rights. But so far, there isn't any.

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


<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-01GGQ8JZW7"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-01GGQ8JZW7');
</script>

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-1542618827905251"
     crossorigin="anonymous"></script>
