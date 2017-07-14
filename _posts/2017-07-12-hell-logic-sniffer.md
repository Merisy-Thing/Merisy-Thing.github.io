---
layout: post
title: "Hell Logic Sniffer"
date: 2017-07-14 14:59:59
description: 'Hell Logic Sniffer 是一款Xilinx FPGA 和 Cypress USB开发板，还是一款逻辑分析仪，作为开发板时可以不需要JTAG下载器。'
main-class: 'misc'        # 主类型，固定几种：misc css dev html jekyll js svg
ribbon-text: 'Device'     # 彩带文本
color: '#7D669E'
tags:
- 
categories:
twitter_text: ''
introduction: 'Hell Logic Sniffer is a FPGA + USB board with logic sniffer function.'
---

# Features
---
* 开发板模式：
  * CY7C68013A：
    * CY7C68013A-56PVXC
    * AT24C64
    * Slave FIFO模式的端口与FPGA连接
  * FPGA：
    * XC3S50AN-TQG144
    * 2片512KB SRAM：IS61LV25616AL-10T
    * 两个8位可分别切换输入/输出状态的外接IO口
    * 利用Xilinx Spartan-3AN系列FPGA的Reconfiguration和MultiBoot特性，通过CY7C68013A和FPGA的IAP可以用USB更新XC3S50AN的用户程序，不需要使用JTAG下载器
* 逻辑分析仪模式：
  * 通道数量：16个输入通道
  * 输入电压：0 ~ 5.5V
  * 采样速率：100MHz(Max)
  * 存储容量：1M字节
  * 采样时间：
  * 100MHz：无压缩 10.48毫秒 (Max)
  * 10Hz ~ 100MHz： 40秒 (Max)
  * 触发方式：任意通道，上升沿/下降沿触发
  * 数据压缩：无压缩/RLE压缩
  * 预 触 发：预触发深度可调
  * 协议分析：I2C、RS232、SPI、自定义 (协议解码截图)
  * 时序查看：全鼠标操作进行缩放、光标定位、波形滚动/滑动、测量，光标边沿自动吸附
  * 供电方式：USB供电，待机电流85mA, 最大工作电流167mA
  * 通信接口：USB2.0
  * 固件升级：可通过USB在线升级USB控制器固件和FPGA固件，保证所有功能可以得到更新
  * 插件扩展：内置Lua脚本语言的插件模块，方便功能扩展，详见插件说明
* 协议嗅探器模式：
  * 支持协议：
    * UART：波特率：3100bps~460800bps、校验位(奇/偶/标记)、最多4四通道；
    * I2C： 支持所有标准速率，起始位/停止位，最多两路；
    * SPI： 支持字节顺序、工作模式、片选使能选择，理论最高25M采样，1路4个片选；
  * 字节间隔计时：0~3.2秒，50uS最小分辨率。
  * 所有通道可同时工作。
  * 片上1KB缓存，12MB/秒的USB传输。
  * 可视化图形显示。
  * 简化版和增强版 两种模式，增强版需要序列号。

# Status
---
* 2014.04.14: PC软件更新 – V2.2, FPGA固件更新 – HellLS_FPGA_FW_05
* 2013.10.14: 软件更新 – V2.1
* 2013.08.16: FPGA固件更新 – HellLS_FPGA_FW_04_(2013.8.16)
* 2013.08.15: USB固件更新 – HellLS_USB_FW_2013.8.15
* 2013.04.13: FPGA固件更新 – HellLS_FPGA_FW_03_(2013.4.13)
* 2013.02.03: USB固件更新 – HellLS_USB_FW_2013.2.3
* 2013.02.02: FPGA固件更新 – HellLS_FPGA_FW_02_(2013.2.2)
* 2013.02.02: 软件更新 – V2.0 – Log
* 2013.02.02: 软件更新 – For Linux V2.0 – Log
* 2013.01.06：添加USB显卡模式
* 2012.12.16: 软件更新 – V1.3 – Log
* 2012.12.16: 软件更新 – For Linux V1.3 – Log
* 2012.11.23: 软件更新 – V1.2 – Log
* 2012.10.18: 软件更新 – V1.1 – Log
* 2012.10.14: 第一批6套样品上架 – Buy One
* 2012.09.19: 发布第一个Linux版本
* 2012.09.17: 发布第一个Windows版本 -V1.0

# Picture
---

# Logic Sniffer Mode
---
## 软件下载：
  * 上位机软件：
    * 2014.04.14 : [Hell_Logic_Sniffer_V2.2](http://pan.baidu.com/s/1kT5bweb)
  * USB 驱动
    * [Zadig](http://zadig.akeo.ie/)
  * FPGA 固件：
    * 2014.04.14 : [HellLS_FPGA_FW_05](http://pan.baidu.com/s/1hq0xM9u)
  * USB 固件：
    * 2012.08.15 : [HellLS_USB_FW_2013.8.15](http://pan.baidu.com/s/1DsEyF)

## 插件系统：
* 用户端软件使用Lua做为插件脚本语言，对软件进行功能扩展。
* 插件系统可以通过插件API访问用户端软件内的大量数据，如读取通道数据、操作光标、输出结果等…
* 插件类型有两种：
  * 解码插件：用来对通道数据进行协议分析，解码数据，如RS232,I2C,SPI。解码插件接口还开放了一个可以自定义解码协议的接口，方便扩展处理特殊的解码协议。
  * 功能插件：用来扩展用户端软件功能，通过插件API可以进行功能扩展，如：频率计、正/负脉宽计数器、字符搜索等等…
* 已经实现的插件可以在用户端软件的“Addon”目录中找到。
* 插件API说明文件：Addon/Plugin_API_Doc.lua，文件做成了功能插件模式，可以在用户端软件运行查看
* 插件API提供调试插件需要的API，调试接口可以在用户端软件上输出调试信息，方便自定义插件调试，插件调试说明
* Lua版本: 5.3

## 固件升级：
* 的FPGA芯片使用的是XC3S50AN-TQG144，片内带有1Mbits的Flash，支持存放两个FPGA的固件（每个FPGA固件需要427Kbits）。
* FPGA固件可以通过USB升级的基础就是利用了Spartan-3AN系列芯片的MultiBoot功能，FPGA芯片内共存放了两个固件，一个是IAP固件，另一个通过IAP下载的固件（下称：HLS固件），芯片上电后自载加载的是IAP固件，IAP固件和USB接口芯片的固件实现升级/更新HLS固件。
* 在正常使用逻辑分析仪功能时，PC端的逻辑分析仪程序连接USB后会向IAP固件发送一条MultiBoot reconfigure 命令，使FPGA运行HLS固件。所以当要进行FPGA固件升级时需要给逻辑分析仪下电后再上电。
* 下图是XC3S50AN的 ISF 内部空间分配图，见Xilinx 用户指南手册，文档编号：UG333， 第21页。

![hls-fpga-flash-map]({{site.baseurl}}/images/hls-fpga-flash-map.jpg)

### FPGA固件升级方法
* 下载新版本的FPGA固件，以及对应固件文件的MD5码。
* 如果逻辑分析仪的USB已经连接电脑，请先断开。
* 打开PC端的FPGA&USB固件升级程序点“FPGA Firmware”标签。
* 连接逻辑分析仪的USB。
* 使用Open File按钮加载新的FPGA固件程序。
* 在MD5码行的右边填入新固件的MD5值，点击MD5 Check按钮。
* 如果以上步骤全部正确完成，那么Download按钮将变成有效。
* 点击Download开始下载。
* 注：由于受各种因素的影响，如果下载失败，请断开USB后重新连接，再次下载。
![hls-fpga-updater]({{site.baseurl}}/images/hls-fpga-updater.jpg)

### USB固件升级方法1
* 下载新版本的USB控制器固件；
* 打开PC端的FPGA&USB固件升级程序点“USB Firmware”标签；
* 点击“Open File”打开下载的USB固件；
* 将逻辑分析仪的USB连接到PC；
* 点击“Download”按钮开始下载；
* 固件升级完成后需要拔插USB线，重新上电后新固件才能生效。
* 注意：
  * 出果出现异常情况无法再用本方法升级USB固件时，请使用下面的”USB固件升级方法2—Cypress官方方式”升级；
  * Hell Logic Sniffer IAP程序的USB固件升级功能需要将USB固件升级到HellLS_USB_FW_2013.2.3或更高版本后才能使用；
  * USB固件从低版本升级请使用下面的”USB固件升级方法2—Cypress官方方式”；
![hls-usb-updater1]({{site.baseurl}}/images/hls-usb-updater1.jpg)

### USB固件升级方法2—Cypress官方式
* 在网站上下载以下两个文件：
  * 新版本的USB控制器固件。
  * 下载cy3684_ez_usb_fx2lp_development_kit_15.exe 并安装。
* 如果逻辑分析仪的USB已经连接电脑，请先断开。
* 启动PC端的USB控制器固件升级程序：CyConsole EZ-USB。(开始 -> 所有程序 -> Cypress -> USB -> CyConsole EZ-USB)
* 使用导线或其它导电材料，将板上标有“I2C JP”字符边上的两个跳线点短接，然后将逻辑分析仪的USB连接到PC，如果一切正常CyConsole EZ-USB会检测到USB控制器，如下图所示。
* 点击图中所示的固件下载按钮“Lg EEPROM”加载固件文件，然后程序将自动开始升级。
* 下载成功后，撤消第四步的短接处理。
* 重新上电，USB控制器固件升级完成。
* 注：由于受各种因素的影响，如果下载失败，请断开USB后重新连接，再次下载。
![hls-usb-updater2]({{site.baseurl}}/images/hls-usb-updater2.jpg)
