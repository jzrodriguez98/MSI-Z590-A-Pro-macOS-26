# MSI-Z590-A-Pro-macOS-26
MSI Z590-A Pro macOS 26 Configuration

Hardware Specifications

Specifications	Details
Computer Model	MSI Z590-A PRO
CPU	Intel Core i9-10900K
Memory	DDR4 2666 Mhz. 64 GB
NVMe SSD	Crucial P3 PCIe Gen3 NVMe 500GB – CT500P3SSD8
Discrete Graphics	AMD RX 6600
Wireless Card	BCM943602CS

Working & Not Working / 可用与不可用的功能

Non-Fuctional / 不工作

Feature	Status	Dependency	Remarks
Non-Fuctional
没有不工作	✅		Completely normal
完全正常
Video and Audio / 音频与视频

Feature	Status	Dependency	Remarks
Full Graphics Accleration (QE/CI)
图形硬件加速	✅	WhateverGreen.kext	
Audio Recording via 3.5mm microphone
通过3.5mm 麦克风录音	✅	AppleALC.kext	
Audio Playback after through 3.5mm
通过 后 3.5mm 接口播放音频	✅	AppleALC.kext	
Automatic Headphone Output Switching
当插入耳机时自动切换音频输出	✅	AppleALC.kext	
Power, Charge, Sleep and Hibernation / 电源管理、充电、睡眠、休眠

Feature	Status	Dependency	Remarks
CPU Power Management (SpeedShift)
CPU 电源管理	✅	SSDT-PLUG	Use iMac20,2
S3 Sleep / Hibernation Mode 3
S3 睡眠 / Mode 3 休眠	✅		
Input & Output

建议自行重新定制USB
如需定制USB 需准备： USB2.0U盘x1 USB3.0U盘x1 USBinjectAll.kext最新版本，请在使用前阅读Readme文档 https://github.com/daliansky/OS-X-USB-Inject-All USB定制教程（部落阁） USB定制教程（视频版）

需要基于 USB控制器 您可能需要安装额外的 kexts:

XHCI-unsupported.kext

X99系列芯片组XHC控制器，8086:8d31
200系列芯片组XHC控制器，8086:a2af（根据macOS版本而有）
300系列芯片组XHC控制器，8086:a36d或8086:9ded
400系列芯片组XHC控制器，8086:a3af
500系列芯片组XHC控制器，8086:43ed
Display, TrackPad and Keyboard / 显示器、触摸板和键盘

Feature	Status	Dependency	Remarks
HiDPI	✅		Natively enabled on UHD DP screen external
在 UHD DP 4K 外接屏幕上原生启用
Refrence / 必读参考资料

dortania's OpenCore Install Guide
daliansky/OC-little
OpenCore 简体中文参考手册 (非官方)
No seriously, PLEASE read those.

务必阅读上述参考资料

Requirement / 需求和依赖

Basic / 基本需求

A macOS machine (optional): to create the macOS installer and build the EFI. 一台已经安装好 macOS 的机器，用于制作 macOS 安装器和编译本项目
Flash drive, 16GB or more, for the above purpose. 一个容量大于等于 16 GiB 的 U 盘
PlistEDPlus to edit plist files on Windows. 编辑 plist 文件的工具 PlistEDPlus
ProperTree to edit plist files on Windows/macOS. 编辑 plist 文件的工具 ProperTree
MaciASL for patching ACPI tables and editing ACPI patches. 用于修补和编辑 ACPI 的工具 MaciASL
HackinTool for diagnosis ONLY. Most of the built-in patches are outdated. 仅用于 诊断的 HackinTool，大部分内置的补丁和工具已经过时、不再适用
Patience and time, especially if this is your first time Hackintosh-ing. 耐心和时间。如果你是第一次进行黑苹果，这尤为重要
Hardware Modification / 硬件修改

SSD

Samusung PM981 is not supported AT ALL. Make sure to switch at least one SSD.

三星 PM981 完全 无法使用，务必更换至少一块 SSD 硬盘。

Wireless Card / 无线网卡

it is recommended to use Broadcom wireless network card for the best experience (better, refer to the use of the best experience).

建议使用博通无线网卡以获得 更好 的体验（更好，指使用 体验）。

BIOS Settings / 修改 BIOS 设置

Settings > Advanced > System Agent (SA) Configuration > VT-D: Disabled
Settings > Advanced > System Agent (SA) Configuration > Above 4G Decoding: Enabled
Settings > Advanced > USB Configuration > XHCI Hand-off: Enabled
Settings > Boot > CSM(Compatibility Support Module) > Launch CSM: Disabled
Settings > Boot > Secure Boot > OS Type: Other OS
Settings > Boot > Boot\Boot Configuration > Wait For 'F1' If Error: Disabled
Win+Mac双系统解决Win系统时间时差问题

在Windows下运行
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
设置默认启动项

在启动选择界面，先选中要启动的项，然后按键盘的 Ctrl + Enter (回车键) 进入系统，下次重启后默认就选中该项了