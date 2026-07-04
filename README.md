## MSI Z590-A Pro macOS 26.5.1 Configuration

### System Overview and Hardware Specifications

| **Component**         | **Details**                                                                                   |
|------------------------|-----------------------------------------------------------------------------------------------|
| **Computer Model**     | MSI Z590-A PRO                                                                               |
| **CPU**               | Intel Core i9-10900K                                                                          |
| **Memory**            | DDR4 3600 MHz, 64 GB Corsair CMK32GX4M2D3600C18                               |
| **NVMe SSD**          | Crucial P3 PCIe Gen3 NVMe 500GB – CT500P3SSD8      |
| **Discrete Graphics** | AMD RX 6600 Armor 8 GB                                                                        |
| **Wireless Card**     | BCM943602CS (using [`AppleBCMWLANCompanion.kext`](https://github.com/0xFireWolf/AppleBCMWLANCompanion))|                                                                        |
| **Ethernet NIC**     | I225-V 2.5G Ethernet Controller                                                                        |
| **Thunderbolt**       | THUNDERBOLTM4 8K (Thunderbolt 4 PCIe Expansion Card)                                                                        |
| **SMBIOS**            | MacPro 7,1                               |
| **macOS version**       | 26.5.1 (26.5.2 update was not good for the AppleBCMWLANCompanion kext which when enabled is not allowing me to load macOS - reported to developer in GitHub)                                                                        |                                      |

### Feature Overview

#### Video and Audio

| **Feature**                                    | **Status** | **Dependency**             | **Remarks**                          |
|------------------------------------------------|------------|-----------------------------|--------------------------------------|
| Full Graphics Acceleration (QE/CI)             | ✅         | None                        | No kext needed with MacPro 7,1 SMBIOS |
| Audio Recording via 3.5mm Microphone           | ✅         | `VoodooHDA.kext`            |                                      |
| Audio Playback via 3.5mm                       | ✅         | `VoodooHDA.kext`            |                                      |
| Automatic Headphone Output Switching           | ✅         | `VoodooHDA.kext`            |                                      |

VoodooHDA Installation

- Exclude other Audio kexts
- Set SIP disable kext or just
sudo nvram csr-active-config=0xA85
- Reboot

sudo cp -R /path_to/VoodooHDA.kext /Library/Extensions/

sudo cp -R /path_to/VoodooHDA.prefPane /Library/PreferencePanes/

- Wait while the system saids that the kext must be approved
- Go to System Settings and approve the kext.
- Reboot.

- If VoodooHDA stops loading, try executing the following commands in Terminal:

sudo chmod -Rf 755 /L*/E*

sudo chown -Rf 0:0 /L*/E*

sudo touch -f /L*/E*

sudo chmod -Rf 755 /S*/L*/E*

sudo chown -Rf 0:0 /S*/L*/E*

sudo touch -f /S*/L*/E*

sudo kextcache -Boot -U /

sudo kextutil -v /Library/Extensions/VoodooHDA.kext

sudo kextload /Library/Extensions/VoodooHDA.kext

HDAUniversal kext installation (as explained in insanelymac community thread)

1- Remove VoodooHDA, AppleAlc or other and inject bootarg alcid=xx or Device Properties. Check Instructions HERE
2- Run the .pkg installer, then open System Settings > Privacy & Security and allow the HDAUniversal kernel extension if macOS asks for permission.
Example: In my ALC897 I can use alcid=69 or defining the layout-id in Device Properties and HDAUniversal.kext, only that.
Why HDAUniversal Must Be Installed in /Library/Extensions
HDAUniversal is designed to work as a system-installed macOS audio kext, not as a simple bootloader-injected kext.
For this first release, the correct and supported installation path is:
/Library/Extensions/HDAUniversal.kext
This requirement is intentional.
Unlike small helper kexts that can usually be injected from EFI, HDAUniversal publishes real IOAudio devices, IOAudio engines, selectors, controls, volume ranges, mute controls, input/output sources, and AppleHDA-like audio endpoints. These objects are used not only by the kernel, but also by macOS user-space audio services such as CoreAudio, Sound Settings, Audio MIDI Setup, Control Center, and coreaudiod.
Installing the kext in /Library/Extensions gives macOS a proper on-disk bundle with the expected structure, metadata, permissions, cache handling, and resource visibility. This is important for stable audio registration, correct IOAudio behavior, system audio discovery, sleep/wake lifecycle, and future localization or UI-related metadata.
Loading HDAUniversal only from EFI may load the binary, but it can produce incomplete or inconsistent behavior because macOS may not treat the kext as a fully installed audio bundle. In that case, CoreAudio and the system UI may not reliably see all metadata, resources, localized names, or audio endpoint information the same way they do when the kext is installed properly in /Library/Extensions.
For this reason, EFI injection is not supported for the first public release.
Supported Installation Method

Install HDAUniversal here:
/Library/Extensions/HDAUniversal.kext
Then rebuild the kext cache / kernel collection and reboot.
Not Supported

EFI/OC/Kexts/HDAUniversal.kext
EFI/CLOVER/kexts/Other/HDAUniversal.kext
Temporary manual loading
Mixed copies in EFI and /Library/Extensions
Only one copy of HDAUniversal should be present on the system. Multiple copies can cause duplicate matching, wrong versions being loaded, broken audio registration, or inconsistent behavior after reboot or sleep/wake.
This approach keeps HDAUniversal closer to the way a real macOS audio driver is expected to live in the system and helps provide the most stable AppleHDA-like experience.

#### ACPI Customizations

- Custom SSDT/DSDT patches applied. Use [`SSDTTime`](https://github.com/corpnewt/SSDTTime) to create your custom SSDTs.

---

#### Power, Charge, Sleep and Hibernation

| **Feature**                | **Status** | **Dependency**                 | **Remarks**            |
|----------------------------|------------|---------------------------------|-------------------------|
| CPU Power Management       | ✅         | `SSDT-PLUG`                    | Or create using SSDTTime |
| S3 Sleep / Hibernation Mode| ✅         | None                           |                         |

---

#### Input & Output: USB

- **Temporary USB Setup**: Use `USBInjectAll.kext` to enable USB for setup.
  - [Read More](https://github.com/daliansky/OS-X-USB-Inject-All)
  
- **Permanent Mapping**:
  - Use `USBToolBox` (recommended) or `USBMap.kext` to generate mappings.
  
| **USB Configuration** | **Details**                                                                     |
|------------------------|---------------------------------------------------------------------------------|
| Unsupported XHCI Ports | X99: 8086:8D31 <br> 200: 8086:A2AF (macOS) <br> 300: 8086:A36D/8086:9DED        |
|                        | 400: 8086:A3AF <br> 500: 8086:43ED                                              |

#### Thunderbolt

To enable **Hot Plug**, use SSDTs created by CaseySJ (https://www.tonymacx86.com/threads/msi-z590-a-pro-thunderbolt-4-add-on-card-hotplug.330187/)

---

#### Ethernet

Add bootarg e1000=0 in NVRAM section of EFI OpenCore config file and enable AppleVTD to be able to have native ethernet support

---

#### Display

| **Feature**         | **Status** | **Dependency** | **Remarks**                               |
|----------------------|------------|-----------------|-------------------------------------------|
| HiDPI Support        | ✅         | None            | Enabled natively for UHD DP screens       |

---

### Reference Links

- [Dortania OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) - refer to Comet Lake settings section of Dortania Guide for specific settings applicable to this CPU generation
- [Daliansky's OC-little Repository](https://github.com/daliansky/OC-little)
- [OC-little Translated](https://github.com/5T33Z0/OC-Little-Translated)
- [Clover Guide](https://github.com/5T33Z0/Clover-Crate)

---

### Bootloader

- **Bootloader**:
  - [OpenCore](https://github.com/acidanthera/OpenCorePkg)
  - [Clover](https://github.com/CloverHackyColor/CloverBootloader)
- **Triple Booting**: Windows 11, macOS 26, and [PearOS NiceC0re 26.4](https://pearos.xyz).
  - **Bootloader Chain**: Use [rEFInd/rEFInd Plus fork](https://github.com/RefindPlusRepo/RefindPlus) to chainload Windows and PearOS, and select between OpenCore or Clover, following this [guide](https://chriswayg.gitbook.io/opencore-visual-beginners-guide/advanced-topics/dual-boot-options/create-refind-booter) and this [guide](https://mifmif.mydns.jp/alpha/?p=1125)

---

### Mandatory Kexts

| **Kext**                              | **Description**                                | **Link**                                                       |
|---------------------------------------|------------------------------------------------|-----------------------------------------------------------------|
| Lilu.kext                             | Kernel extension necessary for macOS support  | [Lilu](https://github.com/acidanthera/Lilu)                    |
| VirtualSMC.kext                       | Core SMC emulation                            | [VirtualSMC](https://github.com/acidanthera/VirtualSMC)        |
| SMCProcessor.kext                     | Used for CPU monitoring (Desktop builds)      | [SMCProcessor](https://github.com/acidanthera/VirtualSMC)      |
| SMCSuperIO.kext                       | Used for SuperIO monitoring                   | [SMCSuperIO](https://github.com/acidanthera/VirtualSMC)        |
| AMDRadeonSensor.kext                  | Enables AMD GPU sensors                       | [AMDRadeonSensor](https://github.com/ChefKissInc/SMCRadeonSensors) |
| RestrictedEvents.kext                 | Security improvements for macOS              | [RestrictedEvents](https://github.com/acidanthera/RestrictEvents) |
| NVMeFix.kext                          | Fixes NVMe incompatibility issues in macOS    | [NVMeFix](https://github.com/acidanthera/NVMeFix)              |
| HibernationFixup.kext                 | Enables hibernation support                   | [HibernationFixup](https://github.com/acidanthera/HibernationFixup) |
| AppleBCMWLANCompanion.kext            | Broadcom Wireless peripheral support          | [AppleBCMWLAN](https://github.com/0xFireWolf/AppleBCMWLANCompanion) |
| VoodooHDA.kext            | Kext for Intel macOS Audio codec support (required in macOS 26 if you want audio without relaxing too much SIP)          | [VoodooHDA](https://github.com/CloverHackyColor/VoodooHDA) |

---

### Optional Kexts

| **Kext**                              | **Description**                                | **Link**                                                       |
|---------------------------------------|------------------------------------------------|----------------------------------------------------------------|
| AdvancedMap.kext                     | Lilu plugin providing modern maps on non-Apple Silicon hardware      | [AdvancedMap](https://github.com/notjosh/AdvancedMap) |       
| itlwm.kext                     | Lilu  plugin enabling WiFi functionality for Intel WiFi cards. Using perez987 fork of Heliport as WiFi client      | [itlwm](https://github.com/OpenIntelWireless/itlwm) [Heliport](https://github.com/perez987/HeliPort)|       
| IntelBluetoothFirmware.kext                     | Lilu  plugin enabling Bluetooth functionality for Intel WiFi cards. Using Vinhts fork as it supports LE devices much better      | [IntelBluetoothFirmware](https://github.com/Vinhts/IntelBluetoothFirmware) |
| HDAUniversal.kext                     | Modern AppleHDA-like audio kernel extension for macOS and Hackintosh systems, created to deliver clean, stable, and natural onboard audio with a behavior closer to Apple’s native audio stack.      | [HDAUniversal](https://www.insanelymac.com/forum/topic/362932-hdauniversal-applehda-like-audio-kext-for-macos-tahoe-and-hackintosh-systems/) |


---

## Requirement

### Basic

- A macOS machine (optional): to create the macOS installer and build the EFI.
- Flash drive, 16GB or more, for the above purpose.
- [PlistEDPlus](https://github.com/ic005k/PlistEDPlus) to edit plist files on Windows.
- [ProperTree](https://github.com/corpnewt/ProperTree) to edit plist files on Windows/macOS.
- [MaciASL](https://github.com/acidanthera/MaciASL) for patching ACPI tables and editing ACPI patches.
- [HackinTool](https://github.com/headkaze/Hackintool) for diagnosis ONLY. Most of the built-in patches are outdated.


---

### BIOS Settings Checklist

1. **System Agent Configuration**:
   - VT-D: Enabled for Broadcom WiFi/BT; Disabled for Intel AX210
   - SEttings \ Advanced \ Above 4G Decoding: Enabled
   - Settings \ Advanced \ Integrated Peripherals → Network Stack: Disabled
   - Settings \ Advanced \ Integrated Peripherals → Intel Serial IO: Disabled
   - Settings \ Advanced \ USB Configuration → XHCI Hand-off: Enabled
   - Settings \ Advanced \ USB Configuration → Legacy USB Support: Auto
   - Settings \ Advanced \ Windows OS Configuration → MSI Fast Boot: Disabled
   - Settings \ Advanced \ Windows OS Configuration → Fast Boot: Disabled
   - Overclocking → Extreme Memory Profile(X.M.P): Enabled unless your build starts freezing / does not like it
   - Overclocking \ CPU Features → Intel Virtualization Tech: Enabled
2. **USB**: Enable XHCI Hand-off
3. **Boot Settings**:
   - Compatibility Support Module (CSM): Disabled
   - Secure Boot: Disabled
   - OS Type: **Other OS**
   - Wait for 'F1' If Error: Disabled

---

### Dual Boot: Windows Edits

Add the following registry entry to Windows 11 to fix clock disruptions:

```bash
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```

---

### Credits

#### OpenCore Credits
- [The HermitCrabs Lab](https://github.com/acidanthera)
- [al3xtjames](https://github.com/al3xtjames)
- [Andrey1970AppleLife](https://github.com/Andrey1970AppleLife)
- [mhaeuser (ex-Download-Fritz)](https://github.com/mhaeuser)
- [Goldfish64](https://github.com/Goldfish64)
- [MikeBeaton](https://github.com/MikeBeaton)
- [nms42](https://github.com/nms42)
- [PMheart](https://github.com/PMheart)
- [savvamitrofanov](https://github.com/savvamitrofanov)
- [usr-sse2](https://github.com/usr-sse2)
- [vit9696](https://github.com/vit9696)

#### Clover Credits
- Developers: Slice, with help of Kabyl, usr-sse2, jadran, Blackosx, dmazar, STLVNUB, pcj, apianti, JrCs, pene, FrodoKenny, skoczy, ycr.ru, Oscar09, xsmile, SoThOr, rehabman, Download-Fritz, nms42, Sherlocks, Zenit432, cecekpawon, stinga11, TheRacerMaster, solstice, Micky1979, Needy, joevt, ErmaC, vit9696, ath, savvas, syscl, goodwin_c, clovy, jief_machak, chris1111, vector_sigma, LAbyOne, Florin9doi, YBronst, Hnanoto

#### Other Credits
- [Chefkiss](https://github.com/ChefKissInc)
- [5T33Z0](https://github.com/5T33Z0)
- [daliansky](https://github.com/daliansky)
- [scytdtf](https://github.com/scytdtf)
- [MaLd0n](https://olarila.com)
- [Vinhts](https://github.com/Vinhts)
- [perez987](https://github.com/perez987)

#### Community Acknowledgments
- **Major Hackintosh Communities**:
  - [r/Hackintosh on Reddit](https://www.reddit.com/r/hackintosh/)
  - [Hackintosh Discord servers](https://discord.com/invite/u8V7N5C)
  - InsanelyMac, tonymacx86 and AppleLife forums.
