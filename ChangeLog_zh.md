# 更新
## v1.13.0

- 整合了 hpm_sdk v1.13.0

- 更新：
  - UART：为所有 UART 补齐 DMA ping-pong 缓冲大小选项并说明约束
  - ADC：适配新版 hpm_sdk 的 ADC 时钟钩子，清理过时的 ADC16 配置与指针转换
  - PWM：驱动按“每通道状态缓存”重构，新增周期、脉宽、死区、相位、重载中断等细粒度控制命令，增加共享计数器与互补对校验，并修正运行时更新与 PWMv2 通道关闭行为
  - ENET：通过 ENET API 配置硬件校验和，并与 RT_LWIP_USING_HW_CHECKSUM 对齐
  - ENET：适配新版 hpm_sdk 的 PHY 接口，中断处理与 hpm_sdk 对齐
  - Kconfig：外设选项与 hpm_sdk IP 支持对齐

- 修复：
  - 链接脚本：修正 flash 脚本 __fast_load_addr__ 的计算
  - ENET：上报 link-up 前先等待 speed_valid，避免链路状态误报
  - ENET：修复 PTP 配置空指针崩溃，以及 LWIP_PTP 下发送变慢的问题
  - SPI：修复 QSPI 数据线映射错误，以及 DMA 路径下字节/元素单位混淆
  - I2C：修复 DMA 传输同步与轮询传输消息计数
  - MCAN：修复扩展帧 ID 掩码位宽与滤波器掩码溢出
  - I2S：修复 reset 后未重新使能 TX/RX
  - 修正板级元数据与文档

- 新增：
  - ENET 硬件时间戳，支持 PTP 同步

## v1.11.0

- 整合了hpm_sdk v1.11.0
- 升级RT-Thread 到 v5.2.2
- 新增 RT-Thread 标准动态中断注册机制，支持在静态绑定与动态管理模式间灵活切换
- 新增通用启动文件（generic startup）支持，同时兼容原有启动方式

- 更新：
  - CherryUSB协议栈从软件包支持改为RT-Thread的组件驱动支持
  - 增强UART V2驱动
  - 增强MCAN驱动
  - 增强CAN驱动
  - 增强ENET驱动
  - 增强ENET PHY驱动

- 修复：

- 新增：
  - 新增软件I2C驱动
  - 新增软件SPI驱动
  - UART V2驱动新增PUART
  - TMR驱动新增PTMR


## v1.10.0

- 整合了 hpm_sdk v1.10.0

- 更新：
  - 升级CherryUSB协议栈到1.5.0

- 新增
  - 支持zcc编译器
  - 支持SEGGER Embedded Studio IDE，版本为8.24

- 修复：
  - 修复中断向量模式在某些条件下工作异常的问题
  - 修复shell 执行`reset`命令后，看门狗复位失效的问题

## v1.9.0

- 整合了 hpm_sdk v1.9.0
- 新增示例:
  - blink_led
  - timer_demo
  - adc_example
  - mcan_example
  - ethernet_demo
  - ethernet_ptp_master_demo
  - ethernet_ptp_slave_demo
  - flashdb_demo
  - uart_dma_demo
  - usb_device_generic_hid
  - usb_host_msc_udisk
  - pwm_demo
