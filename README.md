# MSI-Z590-A-Pro-macOS-26
MSI Z590-A Pro macOS 26 Configuration

Hardware Specifications

Specifications and 	Details

Computer Model: MSI Z590-A PRO

CPU	Intel Core i9-10900K

Memory:	DDR4 3600 Mhz. 64 GB Corsair CMK32GX4M2D3600C18 

NVMe SSD	Crucial P3 PCIe Gen3 NVMe 500GB – CT500P3SSD8

Discrete Graphics:	AMD RX 6600 Armor 8 GB

Wireless Card: 	BCM943602CS using AppleBCMWLANCompanion.kext

SMBIOS: Use MacPro 7,1

Feature	Status	Dependency	Remarks
Non-Fuctional
	✅		Completely normal

Video and Audio 

Feature	Status	Dependency	Remarks
Full Graphics Accleration (QE/CI)

✅	No kext needed if using MacPro 7,1 SMBIOS

Audio Recording via 3.5mm microphone
✅	VoodooHDA.kext	

Audio Playback after through 3.5mm
✅	VoodooHDA.kext	

Automatic Headphone Output Switching
✅	VoodooHDA.kext	

Use SSDTTime to create your custom SSDTs

Power, Charge, Sleep and Hibernation:

Use CPUFriend kexts for advanced CPU Power Management
CPU Power Management (SpeedShift)

CPU	✅	SSDT-PLUG	or Use SSDTTime to create your custom SSDT Plug kext

S3 Sleep / Hibernation Mode 3

S3  / Mode 3 	✅		

Input & Output

USB
USB： USB2.0 x1 USB3.0 x1 USBinjectAll.kext to temporarily enable USB to set up，Readme https://github.com/daliansky/OS-X-USB-Inject-All

USB kexts:

Use USBToolBox (Windows preferably) or USBMap.kext to generate your USB mapping

XHCI-unsupported.kext

X99，8086:8d31

200，8086:a2af（macOS)

300 XHC，8086:a36d 8086:9ded

400 XHC，8086:a3af

500 XHC，8086:43ed

Display and Keyboard

Feature	Status	Dependency	Remarks

HiDPI	✅		Natively enabled on UHD DP screen external

UHD DP 4K

Reference

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

HackinTool for diagnosis ONLY. 

Patience and time, especially if this is your first time Hackintosh-ing. 

Hardware Modification

SSD

Samusung PM981 is not supported AT ALL. Make sure to switch at least one SSD.

Wireless Card

it is recommended to use Broadcom wireless network card for the best experience (better, refer to the use of the best experience).
Alternatively, use Intel AX210 with itlwm kext + Heliport with AppleVTD disabled in BIOS

BIOS Settings

Settings > Advanced > System Agent (SA) Configuration > VT-D: Enabled if using Broadcom wifi+BT card / Disabled if using Intel AX210
Settings > Advanced > System Agent (SA) Configuration > Above 4G Decoding: Enabled
Settings > Advanced > USB Configuration > XHCI Hand-off: Enabled
Settings > Boot > CSM(Compatibility Support Module) > Launch CSM: Disabled
Settings > Boot > Secure Boot > OS Type: Other OS
Settings > Boot > Boot\Boot Configuration > Wait For 'F1' If Error: Disabled
Win+Mac

Windows
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1


Ctrl + Enter 
