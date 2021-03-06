# Dell-Inspiron-7590-Hackintosh-Opencore
OpenCore EFI for Dell Inspiron 759x.   _[English Version](https://github.com/Pinming/Dell-Inspiron-7590-Hackintosh-Opencore/blob/master/README.en.md)_ 

【理论上】本 EFI 支持 Dell Inspiron 7590 / 7591 全系列机型。
![](http://tva1.sinaimg.cn/large/0080xEK2ly1gbzh20adfrj312s0puk0z.jpg)

# 写在前面
* 本 EFI 仅供参考，系统目前各个可以驱动的主要硬件运行基本正常，但无线网卡尚未测试，相关完善将在近期进行。
* 本 EFI 在 @[tctien342](https://github.com/tctien342/Dell-Inspiron-7591-Hackintosh) 的 repo 基础上修改并优化，感谢！
* EFI 已集成 `WhateverGreen` 最新源码（`1.3.7`），夏普屏驱动问题已解决，理论上可以不使用二进制破解引导 10.15 各版本。感谢 @[0xFirewolf](https://github.com/0xfirewolf)！具体解决思路详见：https://github.com/acidanthera/WhateverGreen/pull/41
* `config.plist` 与 `config-1080P.plist` 的异同：前者相较于后者移除了两个值：`device-id` & `AAPL,ig-platform-id`，以保证 4K 机型在 Opencore 环境下不会出现奇怪的花屏或无法进入系统等问题。即 4K 屏幕采用  `config.plist`，1080P 屏幕采用 `config-1080P.plist` 即可。

# 声卡接口修复
在 `ComboJack` 文件夹中打开 `install.sh` 安装声卡接口守护进程，使得机器可以识别耳机接口的插拔。        
在最新版的 `ComboJack` 中（2020/2/18 之后），插入耳机将自动识别，不再出现弹窗。
感谢 @[tctien342](https://github.com/tctien342) 的贡献！
![](http://tva1.sinaimg.cn/large/0080xEK2ly1gbzgvhggtbj30tk0ewahj.jpg)

# 目前存在的 Bug
* 无线网卡 / 雷电接口尚未测试，不确定功能可用性
* 内置麦克风无法使用【无解】
* 电池的容量 (Capacity) 识别错误，应为 97Wh，但实时电量显示基本准确
* 偶有出现声卡掉驱动现象，推测是 `AppleALC` 与 `AppleHDA` 间的加载顺序问题，一时可能无法解决
* HDMI 只能输出画面，不能输出声音
* 系统初次进入默认加载 sRGB 颜色配置，对于 4K 机型，这会导致观感不佳。
> 如有需要可以自行下载 Adobe RGB 的校色文件：【[夏普 SHP14C7](http://oss.pm-z.tech/temp_files/SHP14C7_ICC.zip)】【[友达 AUO41EB](http://oss.pm-z.tech/temp_files/AUO41EB_ICC.zip)】<br>压缩包内已包含 Dell PremierColor 软件中的全部六种配置文件。<br>使用方法：解压压缩包后，将需要的 .icm 文件复制到：`~/Library/ColorSync/Profiles` 中，然后在 `系统偏好设置→显示器→颜色` 中选择相应的配置文件。

# 测试机硬件配置

## 已驱动 / 已知可驱动
**Dell Inspiron 7590** with Sharp SHP14C7 4K Display
* CPU：Intel Core i7-9750H @ 2.60 Ghz (Boost to 4.50 Ghz)
* IGPU：Intel Graphics UHD 630
* RAM：Hynix DDR4 2666Mhz / 16 GB * 2 = 32 GB RAM
* Display：Sharp SHP14C7 @ 15.6' / 4K
* SSD：WD PC SN520 NVMe WDC 512GB SSD
* Audio：Realtek ALC295（戴尔定制型号：ALC3254）（内置麦克风不能驱动）（Layout-ID = 77，选用 28 可能导致 kernel_task 占用过高而导致 CPU 高频不下）
* Micro SD Card Reader：Realtek Memory Card Reader（系统属性「读卡器」一栏无法识别，但可以正常使用）
* 【计划 / 即将更换】_WLAN + Bluetooth：Broadcom DW1820A_

## 已知不可驱动
* Nvidia Geforce GTX 1650（无解）
* Intel Wireless-AC 9560（WiFi 无解 / ~~仅蓝牙可有限度使用~~）加载 Intel 蓝牙驱动会导致启动严重拖慢，考虑后决定不再支持。待开学后换了 DW1820a 再弄蓝牙吧……
* Goodix fingerpint reader（无解）
