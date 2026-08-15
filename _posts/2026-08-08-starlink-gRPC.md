---
date: 2026-08-08 11:05:00
layout: post
title: "Starlink Terminal Codename “Panda”: Capture IQ Signals? The Complete Satellite Access Procedure Begins to Emerge!"
thread: 2026080835
categories: starlink
tags:  gRPC Panda Starlink Starlink-mini LNB SDR AD9361 FPGA Uplink QPSK Pilot
---

The Starlink terminal provides a gRPC service, which makes it possible to access information that is not displayed in the Starlink app.

For example, the hardware version reported by my Starlink Mini through gRPC is:

"hardware_version": "mini1_panda_prod2"

- mini1: identifier for the first-generation/first-version Starlink Mini hardware platform
- panda: an internal Starlink hardware/platform codename. Since it is called “Panda,” could it have some connection to China?
- prod2: production hardware revision / production variant

Although gRPC exposes many configurable and readable interfaces, most of them return Unimplemented when actually accessed. A smaller number explicitly return PermissionDenied.

One particularly interesting gRPC interface is called iqCapture. This really looks like an interface that could allow the Starlink terminal to capture IQ samples for signal analysis. 
Another piece of supporting evidence, the interface immediately following iqCapture is getRadioStats, which also looks like an interface for obtaining low-level radio information.

After searching around, I have not found any publicly documented example of using the Starlink gRPC interface to obtain low-level radio information such as the real-time transmit frequency, channel, modulation scheme, etc. 
Perhaps accessing this information requires deeper firmware analysis or reverse engineering as some others did.

Interestingly, gRPC can provide the complete terminal power-up initialization sequence and its timing.

```
"initialization_duration_seconds": {
    "attitude_initialization": 285,
    "burst_detected": 169,
    "ekf_converged": 361,
    "first_cplane": 175,
    "first_pop_ping": 215,
    "gps_valid": 64,
    "initial_network_entry": 171,
    "network_schedule": 176,
    "rf_ready": 23,
    "stable_connection": 0
}
```

Initialization timeline

23 s — RF ready

The RF subsystem becomes ready first.

64 s — GPS valid

The GPS solution becomes valid.

169 s — Burst detected

This may indicate that the terminal has detected a valid satellite downlink burst for the first time.

171 s — Initial network entry

This probably means that the terminal has successfully completed the initial network access procedure.

175 s — First C-plane

The control-plane connection is established.

176 s — Network schedule

This may mean that the terminal has received its initial radio resource allocation from the network scheduler.

215 s — First PoP ping

The terminal successfully reaches the network/Point of Presence for the first time.

285 s — Attitude initialization

This may refer to initialization of the terminal's attitude estimation. It may be related to antenna pointing and beam-pointing calibration.

After all, Starlink terminals support operation while moving — i.e., connectivity while in motion. This requires a good internal inertial measurement unit (IMU), 
allowing the phased-array antenna to continuously track fast-moving LEO satellites using electronic beam steering.

361 s — EKF converged

EKF usually means Extended Kalman Filter. It converges after 361 seconds.

Kalman filtering has been an important algorithm in spacecraft navigation and docking since the Apollo era and is still widely used in positioning, navigation, and guidance systems. 
Considering the excellent in-motion connectivity capability of Starlink terminals, the presence of an EKF is not surprising.

From power-on to the first successful ping takes only a little over three minutes. Is that fast or slow?

The gRPC interface also provides a surprising amount of information about beam pointing, terminal attitude, and sky obstructions.

It even provides the quaternions needed to transform between the terminal coordinate system and the geographic coordinate system.

There are also various performance metrics, event logs, and statistics, including:

- Ping packet loss rate
- Ping latency
- Downlink and uplink throughput
- Connection-loss events
- Handover events
- And other link statistics

Based on this information, several open-source projects can use the gRPC interface to display, in real time:

- Terminal attitude
- Beam pointing
- Sky obstruction maps
- Link performance
- The Starlink satellite that is most likely to be serving the terminal, based on orbital ephemeris data

gRPC can also report information from the terminal's internal GPS module, including:

- Number of visible satellites
- Satellite health/status
- Convergence status of the GPS internal positioning and navigation filter

According to information available online, older firmware versions could reportedly provide SNR (Signal-to-Noise Ratio). In newer firmware, however, this appears to have been replaced by only a binary indicator:
```
is_snr_above_noise_floor = true/false
```

The gRPC interface also exposes the ready state of several internal modules:

```
"ready_states"
cady = false
scp = true
l1l2 = true
xphy = true
aap = true
rf = true
```
My current guess is that these modules form a hierarchy from lower-level hardware toward higher-level processing:
```
rf — RF subsystem
aap — reportedly the Active Antenna Processor
xphy — transceiver / PHY-related subsystem
l1l2 — Layer 1 and Layer 2
scp — reportedly the System Control Processor
cady — based on information from network analysis, possibly related to the clock source or time reference
```
The exact meaning of some of these internal module names remains unknown. Further firmware analysis may reveal their actual functions.

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
