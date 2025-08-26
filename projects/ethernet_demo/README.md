# 网络连通性及吞吐性能测试

## 概述

本示例演示了基于RT-Thread 网络连通性测试和吞吐性能测试

## 硬件设置

* 使用USB Type-C线缆连接PC USB端口和PWR DEBUG端口
* 使用以太网线缆连接PC以太网端口和开发板RGMII或RMII端口

## 软件设置

* 使用flash_release编译运行，可以获得更好的网络性能

## 运行示例

* 编译下载程序
* 串口终端显示

```console
 \ | /
- RT -     Thread Operating System
 / | \     5.0.2 build Apr 19 2025 10:18:07
 2006 - 2022 Copyright by RT-Thread team
lwIP-2.1.2 initialized!
[27] I/sal.skt: Socket Abstraction Layer initialize success.
msh />[4067] I/NO_TAG: ENET0
[4070] I/NO_TAG: PHY Status: Link up
[4074] I/NO_TAG: PHY Speed: 1000Mbps
[4078] I/NO_TAG: PHY Duplex: full duplex
```

## 功能验证

### 1. IP分配查询及DHCP状态确认

```console
msh />ifconfig
network interface device: ET (Default)
MTU: 1500
MAC: 98 2c bc b1 9f 17
FLAGS: UP LINK_UP INTERNET_DOWN DHCP_ENABLE ETHARP BROADCAST
ip address: 192.168.100.6
gw address: 192.168.100.1
net mask  : 255.255.255.0
dns server #0: 192.168.100.1
dns server #1: 0.0.0.0

```

**注： 若DHCP开启，则DHCP状态为“DHCP_ENABLE”，需要将网口连接至路由器或具有DHCP服务的PC  **   

### 2. PING测试

  （1）Windows系统中，打开cmd, 运行ping

```console
C:\Users>ping 192.168.100.6

正在 Ping 192.168.100.6 具有 32 字节的数据:
来自 192.168.100.6 的回复: 字节=32 时间<1ms TTL=255
来自 192.168.100.6 的回复: 字节=32 时间<1ms TTL=255
来自 192.168.100.6 的回复: 字节=32 时间<1ms TTL=255
来自 192.168.100.6 的回复: 字节=32 时间<1ms TTL=255

192.168.100.6 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 0ms，最长 = 0ms，平均 = 0ms
```

  （2）开发板Ping PC

```console
msh />ping 192.168.100.5
ping: not found specified netif, using default netdev ET.
60 bytes from 192.168.100.5 icmp_seq=0 ttl=64 time=0 ms
60 bytes from 192.168.100.5 icmp_seq=1 ttl=64 time=0 ms
60 bytes from 192.168.100.5 icmp_seq=2 ttl=64 time=0 ms
60 bytes from 192.168.100.5 icmp_seq=3 ttl=64 time=0 ms

```

### 3. **iperf测试**

- **TCP服务端模式**

  - MCU端输入命令

    ```console
    msh /> iperf -s
    ```

  -  PC端输入命令

    ```console
    C:\Users>iperf -c 192.168.100.6 -i 1
    ```

  - 观察PC端结果

    ```console
    ------------------------------------------------------------
    Client connecting to 192.168.100.6, TCP port 5001
    TCP window size: 64.0 KByte (default)
    ------------------------------------------------------------
    [360] local 192.168.100.5 port 53516 connected with 192.168.100.6 port 5001
    [ ID] Interval       Transfer     Bandwidth
    [360]  0.0- 1.0 sec  31.6 MBytes   265 Mbits/sec
    [360]  1.0- 2.0 sec  32.9 MBytes   276 Mbits/sec
    [360]  2.0- 3.0 sec  35.4 MBytes   297 Mbits/sec
    [360]  3.0- 4.0 sec  35.9 MBytes   301 Mbits/sec
    [360]  4.0- 5.0 sec  36.8 MBytes   309 Mbits/sec
    [360]  5.0- 6.0 sec  37.6 MBytes   316 Mbits/sec
    [360]  6.0- 7.0 sec  37.3 MBytes   313 Mbits/sec
    [360]  7.0- 8.0 sec  37.6 MBytes   315 Mbits/sec
    [360]  8.0- 9.0 sec  37.5 MBytes   314 Mbits/sec
    [360]  9.0-10.0 sec  37.9 MBytes   318 Mbits/sec
    [360]  0.0-10.0 sec   361 MBytes   302 Mbits/sec
    ```

  - 观察MCU端结果

    ```console
    msh />[32750] I/iperf: new client connected from (192.168.100.5, 53516)
    [37756] I/iperf: iperfd01: 290.0850 Mbps!
    [42753] W/iperf: client disconnected (192.168.100.5, 53516)
    ```

    

- **TCP客户端模式**

  - PC端输入命令

    ```console
    C:\Users>iperf -s -i 1
    ```

  - MCU端输入命令

    ```console
    msh />iperf -c 192.168.100.5
    ```

  - 观察PC端结果

    ```console
    ------------------------------------------------------------
    Server listening on TCP port 5001
    TCP window size: 64.0 KByte (default)
    ------------------------------------------------------------
    [412] local 192.168.100.5 port 5001 connected with 192.168.100.6 port 52432
    [ ID] Interval       Transfer     Bandwidth
    [412]  0.0- 1.0 sec  33.1 MBytes   277 Mbits/sec
    [412]  1.0- 2.0 sec  35.4 MBytes   297 Mbits/sec
    [412]  2.0- 3.0 sec  35.9 MBytes   301 Mbits/sec
    [412]  3.0- 4.0 sec  33.6 MBytes   282 Mbits/sec
    [412]  4.0- 5.0 sec  35.1 MBytes   295 Mbits/sec
    [412]  5.0- 6.0 sec  33.5 MBytes   281 Mbits/sec
    [412]  6.0- 7.0 sec  35.7 MBytes   299 Mbits/sec
    [412]  7.0- 8.0 sec  36.8 MBytes   309 Mbits/sec
    [412]  8.0- 9.0 sec  33.6 MBytes   282 Mbits/sec
    [412]  9.0-10.0 sec  36.7 MBytes   308 Mbits/sec
    [412] 10.0-11.0 sec  34.8 MBytes   292 Mbits/sec
    [412] 11.0-12.0 sec  35.8 MBytes   301 Mbits/sec
    [412] 12.0-13.0 sec  33.4 MBytes   280 Mbits/sec
    [412] 13.0-14.0 sec  33.6 MBytes   282 Mbits/sec
    [412] 14.0-15.0 sec  36.8 MBytes   309 Mbits/sec
    [412] 15.0-16.0 sec  33.2 MBytes   279 Mbits/sec
    [412] 16.0-17.0 sec  35.4 MBytes   297 Mbits/sec
    [412] 17.0-18.0 sec  36.9 MBytes   310 Mbits/sec
    [412] 18.0-19.0 sec  35.5 MBytes   297 Mbits/sec
    [412] 19.0-20.0 sec  36.6 MBytes   307 Mbits/sec
    ```

  - 观察MCU端结果

    ```console
    msh [13963] I/iperf: Connect to iperf server successful!
    />[18968] I/iperf: iperfc01: 290.3760 Mbps!
    [23968] I/iperf: iperfc01: 295.7110 Mbps!
    [28968] I/iperf: iperfc01: 292.8340 Mbps!
    [33968] I/iperf: iperfc01: 298.0770 Mbps!
    ```

- **UDP服务端模式**

  - MCU端输入命令

    ```console
    msh />iperf -u -s
    ```

  - PC端输入命令

    ```console
    C:\Users>iperf -u -c 192.168.100.6 -i 1 -b 1000M -t 20
    ```

  - 观察PC端结果

    ```console
    ------------------------------------------------------------
    Client connecting to 192.168.100.6, UDP port 5001
    Sending 1470 byte datagrams
    UDP buffer size: 64.0 KByte (default)
    ------------------------------------------------------------
    [360] local 192.168.100.5 port 61017 connected with 192.168.100.6 port 5001
    [ ID] Interval       Transfer     Bandwidth
    [360]  0.0- 1.0 sec  83.6 MBytes   701 Mbits/sec
    [360]  1.0- 2.0 sec  82.4 MBytes   691 Mbits/sec
    [360]  2.0- 3.0 sec  82.0 MBytes   688 Mbits/sec
    [360]  3.0- 4.0 sec  83.0 MBytes   697 Mbits/sec
    [360]  4.0- 5.0 sec  83.0 MBytes   696 Mbits/sec
    [360]  5.0- 6.0 sec  82.4 MBytes   691 Mbits/sec
    [360]  6.0- 7.0 sec  82.7 MBytes   693 Mbits/sec
    [360]  7.0- 8.0 sec  83.6 MBytes   701 Mbits/sec
    [360]  8.0- 9.0 sec  83.4 MBytes   699 Mbits/sec
    [360]  9.0-10.0 sec  82.8 MBytes   695 Mbits/sec
    [360] 10.0-11.0 sec  83.6 MBytes   701 Mbits/sec
    [360] 11.0-12.0 sec  82.9 MBytes   696 Mbits/sec
    [360] 12.0-13.0 sec  82.1 MBytes   689 Mbits/sec
    [360] 13.0-14.0 sec  83.3 MBytes   699 Mbits/sec
    [360] 14.0-15.0 sec  82.8 MBytes   694 Mbits/sec
    [360] 15.0-16.0 sec  80.0 MBytes   671 Mbits/sec
    [360] 16.0-17.0 sec  82.5 MBytes   692 Mbits/sec
    [360] 17.0-18.0 sec  82.1 MBytes   689 Mbits/sec
    [360] 18.0-19.0 sec  81.8 MBytes   686 Mbits/sec
    [360] 19.0-20.0 sec  83.3 MBytes   699 Mbits/sec
    [ ID] Interval       Transfer     Bandwidth
    [360]  0.0-20.0 sec  1.61 GBytes   693 Mbits/sec
    [360] WARNING: did not receive ack of last datagram after 10 tries.
    [360] Sent 1179300 datagrams
    ```

  - 观察MCU端结果

    ```console
    msh />[170085] I/iperf: iperfd01: 137.4670 Mbps! lost:51630 total:110076
    
    [175092] I/iperf: iperfd01: 354.6550 Mbps! lost:144134 total:294923
    
    [180099] I/iperf: iperfd01: 351.5980 Mbps! lost:144616 total:294105
    
    [185106] I/iperf: iperfd01: 352.3900 Mbps! lost:143713 total:293539
    
    [190246] I/iperf: iperfd01: 195.3810 Mbps! lost:-2238985 total:-2153714
    
    ```
    
    

- UDP客户端模式

  - PC端输入命令

    ```console
    C:\Users>iperf -u -s -i 1
    ```

  - MCU端输入命令

    ```console
    msh />iperf -u -c 192.168.100.5
    ```

  - 观察MCU端结果

    ```console
    ------------------------------------------------------------
    Server listening on UDP port 5001
    Receiving 1470 byte datagrams
    UDP buffer size: 64.0 KByte (default)
    ------------------------------------------------------------
    [344] local 192.168.100.5 port 5001 connected with 192.168.100.6 port 62510
    [ ID] Interval       Transfer     Bandwidth       Jitter   Lost/Total Datagrams
    [344]  0.0- 1.0 sec  74.2 MBytes   622 Mbits/sec  0.007 ms 9125/62035 (15%)
    [344]  1.0- 2.0 sec  73.7 MBytes   618 Mbits/sec  0.006 ms  223/52791 (0.42%)
    [344]  2.0- 3.0 sec  74.0 MBytes   621 Mbits/sec  0.009 ms    0/52782 (0%)
    [344]  3.0- 4.0 sec  75.0 MBytes   629 Mbits/sec  0.008 ms  215/53721 (0.4%)
    [344]  4.0- 5.0 sec  84.4 MBytes   708 Mbits/sec  0.004 ms  255/60447 (0.42%)
    [344]  5.0- 6.0 sec  84.6 MBytes   710 Mbits/sec  0.004 ms    0/60334 (0%)
    [344]  6.0- 7.0 sec  84.7 MBytes   710 Mbits/sec  0.014 ms    0/60416 (0%)
    [344]  7.0- 8.0 sec  84.4 MBytes   708 Mbits/sec  0.004 ms  281/60483 (0.46%)
    [344]  8.0- 9.0 sec  84.7 MBytes   710 Mbits/sec  0.035 ms   33/60441 (0.055%)
    [344]  9.0-10.0 sec  84.7 MBytes   710 Mbits/sec  0.009 ms  119/60519 (0.2%)
    [344] 10.0-11.0 sec  84.1 MBytes   706 Mbits/sec  0.011 ms  410/60410 (0.68%)
    [344] 11.0-12.0 sec  84.7 MBytes   710 Mbits/sec  0.007 ms   86/60478 (0.14%)
    [344] 12.0-13.0 sec  84.7 MBytes   711 Mbits/sec  0.003 ms    0/60431 (0%)
    [344] 13.0-14.0 sec  84.8 MBytes   712 Mbits/sec  0.009 ms    0/60513 (0%)
    [344] 14.0-15.0 sec  84.4 MBytes   708 Mbits/sec  0.010 ms  242/60413 (0.4%)
    [344] 15.0-16.0 sec  80.1 MBytes   672 Mbits/sec  0.003 ms 3362/60492 (5.6%)
    [344] 16.0-17.0 sec  84.7 MBytes   711 Mbits/sec  0.003 ms    7/60426 (0.012%)
    [344] 17.0-18.0 sec  84.4 MBytes   708 Mbits/sec  0.045 ms  283/60480 (0.47%)
    [344] 18.0-19.0 sec  84.5 MBytes   708 Mbits/sec  0.051 ms  253/60499 (0.42%)
    [344] 19.0-20.0 sec  84.3 MBytes   707 Mbits/sec  0.051 ms  375/60492 (0.62%)
    ```

    

  - 观察MCU端结果

    ```console
    [66131] I/iperf: iperf udp mode run...
    ```

    **注：此模式下，MCU端无统计信息输出，且无退出机制，需要按reset键重启MCU。**

  

  

