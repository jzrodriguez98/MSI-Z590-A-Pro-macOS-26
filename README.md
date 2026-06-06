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



Feature	Status	Dependency	Remarks
Non-Fuctional
没有不工作	✅		Completely normal
完全正常
Video and Audio / 音频与视频

Feature	Status	Dependency	Remarks
Full Graphics Accleration (QE/CI)
	✅	WhateverGreen.kext	
Audio Recording via 3.5mm microphone
	✅	AppleALC.kext	
Audio Playback after through 3.5mm
	✅	VoodooHDA.kext	
Automatic Headphone Output Switching
	✅	VoodooHDA.kext	
Power, Charge, Sleep and Hibernation

Feature	Status	Dependency	Remarks
CPU Power Management (SpeedShift)
CPU	✅	SSDT-PLUG	Use MacPro 7,1
S3 Sleep / Hibernation Mode 3
S3 睡眠 / Mode 3 休眠	✅		
Input & Output

USB
USB： USB2.0 x1 USB3.0 x1 USBinjectAll.kext，Readme https://github.com/daliansky/OS-X-USB-Inject-All

USB kexts:

XHCI-unsupported.kext

X99，8086:8d31
200，8086:a2af（根据macOS版本而有）
300系列芯片组XHC控制器，8086:a36d或8086:9ded
400系列芯片组XHC控制器，8086:a3af
500系列芯片组XHC控制器，8086:43ed
Display, TrackPad and Keyboard / 显示器、触摸板和键盘

Feature	Status	Dependency	Remarks
HiDPI	✅		Natively enabled on UHD DP screen external
在 UHD DP 4K 外接屏幕上原生启用
Reference / 必读参考资料

dortania's OpenCore Install Guide
daliansky/OC-little
OpenCore 

Requirement

Basic

A macOS machine (optional): to create the macOS installer and build the EFI. 
Flash drive, 16GB or more, for the above purpose. 
PlistEDPlus to edit plist files on Windows.
ProperTree to edit plist files on Windows/macOS.
MaciASL for patching ACPI tables and editing ACPI patches.
HackinTool for diagnosis ONLY. Most of the built-in patches are outdated.
Patience and time, especially if this is your first time Hackintosh-ing. 
Hardware Modification

SSD

Samusung PM981 is not supported AT ALL. Make sure to switch at least one SSD.

Wireless Card

it is recommended to use Broadcom wireless network card for the best experience (better, refer to the use of the best experience).


BIOS Settings

Settings > Advanced > System Agent (SA) Configuration > VT-D: Disabled
Settings > Advanced > System Agent (SA) Configuration > Above 4G Decoding: Enabled
Settings > Advanced > USB Configuration > XHCI Hand-off: Enabled
Settings > Boot > CSM(Compatibility Support Module) > Launch CSM: Disabled
Settings > Boot > Secure Boot > OS Type: Other OS
Settings > Boot > Boot\Boot Configuration > Wait For 'F1' If Error: Disabled
Win+Mac

Windows
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1


在启动选择界面，先选中要启动的项，然后按键盘的 Ctrl + Enter (回车键) 进入系统，下次重启后默认就选中该项了