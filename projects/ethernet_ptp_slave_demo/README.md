# lwIP PTP Slave V1

## 概述

本示例展示PTP Slave的功能

- Master与Slave通过PTP协议通讯，从而实现Slave端的时钟同步

## 硬件设置

- 使用USB Type-C线缆连接PC USB端口和PWR DEBUG端口
- 使用以太网线缆连接开发板RGMII或者RMII端口和另一块开发板的RGMII或RMII端口

## 编译设置

- 在双网口的开发板上，默认支持的是RGMII接口
- 在双网口的开发板上，仅支持一个网口，若需要使用另一个网口，可以通过如下两种方式之一：
    - RT-Thread Settings->硬件->芯片设备驱动->Enable Ethernet
    - menuconfig->Hardware Drivers Config->On-chip Peripheral Drivers->Enable Ethernet

## 运行示例

- 编译下载程序（J-Link JTAG）
- 串口终端显示（Slave 与 Master 直连以太网，使能 `LWIP_PTP` 硬件时间戳后）

```console
 \ | /
- RT -     Thread Operating System
 / | \     5.2.2 build Jul 18 2026 14:33:01
 2006 - 2024 Copyright by RT-Thread team
lwIP-2.1.2 initialized!
[29] I/sal.skt: Socket Abstraction Layer initialize success.
(D 0.006545830) event POWER UP
(D 0.009401820) initClock
(D 0.011801060) state PTP_INITIALIZING
(D 0.015319080) manufacturerIdentity: PTPd;1.0.1
(D 0.019719740) netShutdown
(D 0.022270470) netInit
(D 0.024685660) initData
(D 0.026998380) initTimer
(D 0.029375450) initClock
(D 0.031773150) sync message interval: 1000
(D 0.035727290) clock identifier: DFLT
(D 0.039238740) 256*log2(clock variance): -4000
(D 0.043536660) clock stratum: 255
(D 0.046697570) clock preferred?: no
(D 0.050038070) bound interface name:
(D 0.053549740) communication technology: 1
(D 0.057499400) uuid: 98:2c:bc:b1:9f:18
(D 0.061108120) PTP subdomain name: _DFLT
(D 0.064878230) subdomain address: 224.0.1.129
(D 0.069093370) event port address: 3f 1
(D 0.072837220) general port address: 40 1
(D 0.076697760) initClock
(D 0.079064100) state PTP_LISTENING
(D 0.082318010) initClock
(D 0.084685740) state PTP_SLAVE
msh />
[4119] I/NO_TAG: ENET0
[4122] I/NO_TAG: PHY Status: Link up
[4126] I/NO_TAG: PHY Speed: 1000Mbps
[4130] I/NO_TAG: PHY Duplex: full duplex

(D 3.658592760) addForeign: new record (0,1) 1 1 98:2c:bc:b1:9f:17
(D 3.664606710) event MASTER_CLOCK_CHANGED
(D 3.668457560) state PTP_UNCALIBRATED
(D 3.671970110) toState: Q = 0, R = 7
(D 2426.522862410) updateOffset
(D 2426.525766190) one-way delay:           0s      18902ns
(D 2426.531111910) offset from master:      0s         83ns
(D 2426.536455190) observed drift:          -9892
(D 2427.523183830) updateOffset
(D 2427.526086050) one-way delay:           0s      18902ns
(D 2427.531431380) offset from master:      0s         60ns
(D 2427.536774960) observed drift:          -9888
(D 2428.523501900) updateOffset
(D 2428.526407200) one-way delay:           0s      18902ns
(D 2428.531752590) offset from master:      0s         49ns
(D 2428.537095810) observed drift:          -9885
(D 2429.523814450) updateOffset
(D 2429.526721610) one-way delay:           0s      18902ns
(D 2429.532067030) offset from master:      0s         34ns
(D 2429.537410120) observed drift:          -9883
(D 2430.524139980) updateOffset
(D 2430.527043760) one-way delay:           0s      18902ns
(D 2430.532389100) offset from master:      0s         26ns
(D 2430.537732230) observed drift:          -9882
(D 2431.524456850) updateOffset
(D 2431.527360610) one-way delay:           0s      18902ns
(D 2431.532706440) offset from master:      0s         37ns
(D 2431.538049600) observed drift:          -9880
(D 2432.524775060) updateOffset
(D 2432.527680610) one-way delay:           0s      18902ns
(D 2432.533027040) offset from master:      0s         42ns
(D 2432.538370150) observed drift:          -9878
(D 2433.525092200) updateOffset
(D 2433.527995870) one-way delay:           0s      18902ns
(D 2433.533341260) offset from master:      0s         55ns
(D 2433.538684510) observed drift:          -9875
(D 2434.525416580) updateOffset
(D 2434.528322260) one-way delay:           0s      18902ns
(D 2434.533668700) offset from master:      0s         52ns
(D 2434.539011890) observed drift:          -9872
(D 2435.525730410) updateOffset
(D 2435.528635900) one-way delay:           0s      18902ns
(D 2435.533982610) offset from master:      0s         50ns
(D 2435.539325830) observed drift:          -9869
(D 2436.526046680) updateOffset
(D 2436.528950380) one-way delay:           0s      18902ns
(D 2436.534295740) offset from master:      0s         44ns
```

同步稳定后，`offset from master` 可收敛并维持在约 ±100ns 以内（上例连续多次采样均在 100ns 内）。
