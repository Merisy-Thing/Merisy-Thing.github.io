---
layout: post
title: "Hell Watch"
date: 2017-07-13 14:52:23
description: 'Hell Watch是一个使用AVR做控制器，使用OLED做显示的小设备，带有RTC、加速度传感器、电子罗盘、蜂鸣器及2.4G控制器模块等，通过加载不同的程序可以实现：电子手表、电子书、2.4G控制器、示波器、逻辑分析仪、互动游戏等功能。'
main-class: 'misc'        # 主类型，固定几种：misc css dev html jekyll js svg
ribbon-text: 'Device'     # 彩带文本
color: '#EB7728'          # 颜色：#637a91    #7D669E   #EB7728    #D6BA32   #7AAB13   #B31917
tags:
- 
categories:
twitter_text: ''
introduction: 'Hell Watch is a small embeded device base on AVR XMEGA with OLED display.'
---

## Overview
软硬件参考了网上许多优秀的开源设计实现，主要如：Zak Kemble的[DIY_Digital_Wristwatch](http://blog.zakkemble.co.uk/diy-digital-wristwatch/),  Gabotronics 的 [XMEGA Xprotolab](http://www.gabotronics.com/development-boards/xmega-xprotolab.htm)系列开发板资料。

引导装载程序(Boot Loader)基于LUFA(许可)实现，用于管理应用程序的USB在线下载和软件脱机切换。

---
## Introduction
* **硬件**：
  * **MCU**： Atmel低功耗的AVR XMEGA微控制器ATxmega32A4U: 0–32MHz，4K RAM，32KB App Flash，4K Boot Flash，1K EEPROM；
  * **显示**：1.3″ 128×64 OLED显示屏；
  * **按键**：5个按键和1个三向拨轮开关；
  * **IO**：对外引出8个通用IO口（带1个UART、1个I2C、2路ADC、2路DAC）；
  * **通信**：配有1个MicroUSB接口，用于下载程序/数据通信；
  * **电池**：系统使用180mAh的锂电池供电，板载锂电池充电管理芯片；所有电路进入省电状态时，整机最低功耗可降到500uA左右，提供较长的待机时间；
  * **提示**：1个电磁式蜂鸣器、1个振动马达；
  * **传感器**：1个加速度传感器、1个电子罗盘；
  * **时钟**：1个RTC时钟，直接由锂电池供电；
  * **无线**：Nordic 单芯片2.4G收发器（nRF24L01+）；
  * **存储器**：1MByte的SPI Flash(256KB用户空间、512KB程序空间(256~768KB)、256KB预存信息空间(写保护)(768~1024KB)；
* 多应用管理：
  * Hell Watch最多可以装载 16+1 个应用程序，通过应用下载工具配合Bootloader程序进行应用程序的下载/删除管理。脱机情况下Bootloader可以进行任意应用程序的加载切换。
  * 每个应用程序占用32KB (32704B(应用程序) + 64B(应用信息))，用户程序的大小不能超过32704B，程序可以下载到控制器内部程序区(没用应用信息)或SPI Flash区内。SPI Flash上的程序放置在“256KB~768KB”间的512KB(16*32KB)空间。
  * 每个设备都有唯一的序列号(通过下载工具读取)，配合下载工具的APP ID下载功能可以让应用程序只在许可的设备上运行(详见许可管理说明)。
  * 程序安全：
  * 下载到SPI Flash上的程序会被加密(AES加密)，保证程序的安全性(详见程序加密说明)；
  * ATxmega32A4U的LOCKBITS被设置为“0x3C”：
    * 应用程序无法读取Bootloader区内的数据；
    * 外部编程接口(PDI)无法读取内部所有存储区。
  * 注意：应用程序**不要修改LOCKBITS**，否则会造成Bootloader无法正常工作，必须回厂重新烧写Bootloader才能恢复。

---
## Block Diagram
* 硬件连接框图：

![hw-block-diagram]({{site.baseurl}}/images/hw-block-diagram.png)

* 系统Flash分配框图：

![hw-flash-usage]({{site.baseurl}}/images/hw-flash-usage.png)

---
## Driver & Tools
* USB驱动：Hell Watch USB Driver，安装方法请参考Hell Logic Sniffer的驱动安装方法。
* 上位机下载工具：
  * Windows:  [Hell Watch Updater V1.0](http://pan.baidu.com/s/1kTofekB)

![hw-updater]({{site.baseurl}}/images/hw-updater.png)

---
## Open Source
* Hell Watch App Demo：Demo程序集结地
  * Git@OSC：[Hell_Watch_App_Demo](http://git.oschina.net/hell-prototypes/Hell_Watch_App_Demo)
* Hell Watch WristWatch：数字时钟，由 DIY Digital Wristwatch 的源码移植过来
  * Git@OSC：[Hell_Watch_WristWatch](http://git.oschina.net/hell-prototypes/Hell_Watch_WristWatch)
* Hell Watch Test：测试程序
  * Git@OSC：[Hell_Watch_Test](http://git.oschina.net/hell-prototypes/Hell_Watch_Test)
* Hell Watch Memory：存储器查看
  * Git@OSC：[Hell_Watch_Memory](http://git.oschina.net/hell-prototypes/Hell_Watch_Memory)
* Hell Watch Reader：电子书（未完成）
  * Git@OSC：[Hell_Watch_Reader](http://git.oschina.net/hell-prototypes/Hell_Watch_Reader)
* Hell Watch LUFA 移植：[LUFA是一款开源的轻量级AVR USB框架](http://www.lufa-lib.org/)
  * Github：[LUFA](https://github.com/hellprototypes/lufa) – Hell_Watch分支
  * 已移植完成的项目：
    * AVRISP-MKII ：当AVR 编程器使用，兼容 ISP（已测试）、PDI（已测试）、TPI（未测试）
    * USBtoSerial： 当USB转串口线使用，也可以作为虚拟串口使用，如将调试信息通过USB送给PC，不通过外接串口。
* 开源程序固件汇总下载：[点这里](http://git.oschina.net/hell-prototypes/Hell_Watch_Applications)
* 程序编译方法：安装编译器和编译环境后在源码目录内打开一个命令行窗口，运行make命令就可以编译。

---
## Develop Tools
* Make+avr8编译器：
  * 编译器下载：avr8-gnu-toolchain
  * 编译环境下载：Tools For Win From WinAVR，解压后把make.exe所在的目录路径加入PATH环境变量中。
  * 安装方法：下载编译器和编译环境后运行解压这两个文件到指定目录(如：D:\)，为系统的环境变量“PATH”添加：D:\avr8-gnu-toolchain\bin;D:\Tools_For_Win; 完成安装，可以使用make命令编译源代码了。
* Arduino兼容开发环境：
  * **FIXME...**
* 原理图下载：[Hell Watch SCH V1.0](http://pan.baidu.com/s/1i3wqFBJ)

---
## Soft ENV Notice
* 应用程序中需要注意的那些Bootloader设置过的寄存器：
  * 功耗控制：
    * PR.PRGEN = 0x5F; // Stops the clock to: USB, AES, EBI, RTC, EVSYS, DMA
    * PR.PRPA = 0x05; // Stops the clock to: DAC, AC
    * PR.PRPB = 0x07; // Stops the clock to: DAC, ADC, AC
    * PR.PRPC = 0x34; // Stop the clock to: USART0, USART1, HIRES
    * PR.PRPD = 0x6C; // Stops the clock to: TWI, USART1, SPI, HIRES
    * PR.PRPE = 0x6C; // Stops the clock to: TWI, USART1, SPI, HIRES
  * USB：
    * CLK.USBCTRL = CLK_USBSRC_RC32M_gc \| CLK_USBSEN_bm;
    * USB.CTRLA = USB_ENABLE_bm \| USB_SPEED_bm;
    * OLED接口：
    * USARTD0.BAUDCTRLA = 0x08; // BSEL=1
    * USARTD0.CTRLC = 0xC0; // Master SPI
    * USARTD0.CTRLB = 0x08; // Enable TX
  * 时钟：
    * DFLLRC32M.CTRL = DFLL_ENABLE_bm;
    * OSC.PLLCTRL = OSC_PLLSRC_RC2M_gc \| 16; // 2MHz * 16 = 32MHz
    * OSC.CTRL = OSC_RC32MEN_bm \| OSC_PLLEN_bm \| OSC_RC2MEN_bm ; // Enable PLL
    * DFLLRC2M.CTRL = DFLL_ENABLE_bm;
    * CLK.CTRL = CLK_SCLKSEL_PLL_gc; // Select PLL as system clock source

---
## 程序加密
* 加密的目地：
  * Hell Watch程序加密系统是针对不开放源码的程序设计的，开源程序的程序发布可以直接使用HEX格式。
  * Hell Watch系统放在外部SPI Flash里的程序会被加密后存放的，为非开源的程序提供一层基本的安全屏障。
* 加密系统说明：
  * 加密算法用的是128bit的AES算法，程序的加密由上位机的下载工具完成，加密后的程序通过Bootloader透传下载到SPI Flash中去，Bootloader在加载程序时读取SPI Flash的密文数据，使用指定的密钥解密后写入MCU内部的Flash。
  * 系统共设计了128个密钥，每个Hell Watch设备都带有这128个密钥，存放在SPI Flash最后一个扇区的前2KB中，这些密钥被Bootloader内置的密钥和MCU序列号所加密，所以每个Hell Watch在该区域内的数据都不一样，保证密钥的安全性。
  * 上位机的下载工具中内置一个共用的加密密钥，这个共用密钥没有安全性保证，对下载工具做逆向破解很容易可以得到该密钥，使用另外127个中的密钥可以提高程序安全性。
  * 当然所有程序安全都是建立在MCU芯片不被破解的基础上，如果MCU芯片被破解那么以上所有的安全设计就一点用处也没有了。
* 加密程序的发布：
  * 上位机下载工具可以把编译出来的明文程序加密另存为Hell Watch的“.hwp”程序文件，hwp文件包含：程序名称、说明、加密的用户程序、使用的密钥索引、程序大小、完整性校验值等信息，该文件可以作为密文程序分发使用，保证程序分发过程中的程序安全。
* 程序下载流示意图：

![hw-security]({{site.baseurl}}/images/hw-security.png)
* 许可管理
  * 许可管理为应用程序提供一种应用授权管理的方案，可以用来保证只有得到授权的设备才允许运行该应用程序，避免非法拷贝。
  * 许可管理设计是建立在加密系统安全性上，如果加密系统被破解，许可管理系统也就失去了安全屏障。
  * 许可管理由两组数据：“设备ID”和“许可ID”组成，其中“设备ID”可以通过上位机的下载工具获取，“许可ID”是开发者通过自己的换算算法对设备ID进行计算的结果，Hell Watch的下载工具提供一个把最多16个字节的许可ID存放在应用信息区中的功能。(应用信息区说明见下面的分配图)
  * 给设备授权的一种操作方式示例：
    * 应用开发者获取由授权目标用户提供的设备ID；
    * 应用开发者使用“换算算法”对用户提供的设备ID进行换算，得到许可ID；
    * 应用开发者将得到的“许可ID”和“应用程序(HWP格式)”发送给用户；
    * 用户使用上位机工具加载“应用程序”，输入“许可ID”，下载到设备中，开始使用；
    * 应用程序被加载运行后可以先读取“设备ID”，使用“换算算法”对读到了的“设备ID”做计算，然后将计算结果与应用信息区中的“许可ID”比较；
    * 根据比较结果就可以判断该设备是否得到了授权许可；
* 应用信息区数据分配图：

![hw-app-info]({{site.baseurl}}/images/hw-app-info.png)
* 下载工具提供的“设备ID”读取方法：

{% highlight c %}
char id_num[11];
char calib_offset[11] = {
    0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, //LOTNUM0~5
    0x10, //WAFNUM
    0x12, 0x13, 0x14, 0x15 //COORDX0~1, COORDY0~1
};

for(int i=0; i<11; i++) {
    if(!flash_read_calib(calib_offset[i], &id_num[i])) {
        note_message("## Fail to read device ID...");
        return;
    }
}
for(int i=0; i<11; i++) {
    printf("%02x", (uchar)id_num[i]);
}
{% endhighlight %}

