## MSI Z590-A Pro macOS 26 Configuration

### Hardware Specifications

| **Component**         | **Details**                                                                                   |
|------------------------|-----------------------------------------------------------------------------------------------|
| **Computer Model**     | MSI Z590-A PRO                                                                               |
| **CPU**               | Intel Core i9-10900K                                                                          |
| **Memory**            | DDR4 3600 MHz, 64 GB Corsair CMK32GX4M2D3600C18                                               |
| **NVMe SSD**          | [Crucial P3 PCIe Gen3 NVMe 500GB – CT500P3SSD8](https://www.crucial.com/products/ssd/p3)      |
| **Discrete Graphics** | AMD RX 6600 Armor 8 GB                                                                        |
| **Wireless Card**     | BCM943602CS (using [`AppleBCMWLANCompanion.kext`](https://github.com/0xFireWolf/AppleBCMWLANCompanion))|
| **Thunderbolt**       | THUNDERBOLTM4 8K (Thunderbolt 4 PCIe Expansion Card)                                          |
| **SMBIOS**            | Use MacPro 7,1                                                                                |

---

### Feature Overview

#### Video and Audio

| **Feature**                                    | **Status** | **Dependency**             | **Remarks**                          |
|------------------------------------------------|------------|-----------------------------|--------------------------------------|
| Full Graphics Acceleration (QE/CI)             | ✅         | None                        | No kext needed with MacPro 7,1 SMBIOS |
| Audio Recording via 3.5mm Microphone           | ✅         | `VoodooHDA.kext`            |                                      |
| Audio Playback via 3.5mm                       | ✅         | `VoodooHDA.kext`            |                                      |
| Automatic Headphone Output Switching           | ✅         | `VoodooHDA.kext`            |                                      |

#### ACPI Customizations

- Use [`SSDTTime`](https://github.com/corpnewt/SSDTTime) to create your custom SSDTs.

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

#### Display

| **Feature**         | **Status** | **Dependency** | **Remarks**                               |
|----------------------|------------|-----------------|-------------------------------------------|
| HiDPI Support        | ✅         | None            | Enabled natively for UHD DP screens       |

---

### Reference Links

- [Dortania OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [Daliansky's OC-little Repository](https://github.com/daliansky/OC-little)

---

### Bootloader

- **Bootloader**:
  - [OpenCore](https://github.com/acidanthera/OpenCorePkg)
  - [Clover](https://github.com/CloverHackyColor/CloverBootloader)
- **Triple Booting**: Windows 11, macOS 26, and [PearOS NiceC0re 26.4](https://pearos.xyz).
  - **Bootloader Chain**: Use `rEFInd` to chainload Windows and PearOS, and select between OpenCore or Clover.

---

### Mandatory Kexts

| **Kext**                              | **Description**                                | **Link**                                                       |
|---------------------------------------|------------------------------------------------|-----------------------------------------------------------------|
| Lilu.kext                             | Kernel extension necessary for macOS support  | [Lilu](https://github.com/acidanthera/Lilu)                   |
| VirtualSMC.kext                       | Core SMC emulation                            | [VirtualSMC](https://github.com/acidanthera/VirtualSMC)        |
| SMCProcessor.kext                     | Used for CPU monitoring (Desktop builds)      | [SMCProcessor](https://github.com/acidanthera/VirtualSMC)     |
| SMCSuperIO.kext                       | Used for SuperIO monitoring                   | [SMCSuperIO](https://github.com/acidanthera/VirtualSMC)       |
| AMDRadeonSensor.kext                  | Enables AMD GPU sensors                       | [AMDRadeonSensor](https://github.com/ChefKissInc/SMCRadeonSensors) |
| RestrictedEvents.kext                 | Security improvements for macOS              | [RestrictedEvents](https://github.com/acidanthera/RestrictEvents) |
| NVMeFix.kext                          | Fixes NVMe incompatibility issues in macOS    | [NVMeFix](https://github.com/acidanthera/NVMeFix)             |
| HibernationFixup.kext                 | Enables hibernation support                   | [HibernationFixup](https://github.com/acidanthera/HibernationFixup) |
| AppleBCMWLANCompanion.kext            | Broadcom Wireless peripheral support          | [AppleBCMWLAN](https://github.com/0xFireWolf/AppleBCMWLANCompanion) |

---

### Optional Kexts

| **Kext**                              | **Description**                                | **Link**                                                       |
|---------------------------------------|------------------------------------------------|-----------------------------------------------------------------|
| AdvancedMap.kext                     | Lilu plugin providing modern maps on non-Apple Silicon hardware      | [AdvancedMap](https://github.com/notjosh/AdvancedMap)        |

---

### BIOS Settings Checklist

1. **System Agent Configuration**:
   - VT-D: Enable for Broadcom WiFi/BT; Disable for Intel AX210.
   - Above 4G Decoding: Enable.
2. **USB**: Enable XHCI Hand-off.
3. **Boot Settings**:
   - Compatibility Support Module (CSM): Disabled.
   - Secure Boot: OS Type: **Other OS**.
   - Wait for 'F1' If Error: Disabled.

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
- Developers: Slice, Kabyl, usr-sse2, jadran, Blackosx, dmazar, STLVNUB, pcj, apianti, JrCs, pene, FrodoKenny, skoczy, ycr.ru, Oscar09, xsmile, SoThOr, rehabman, Download-Fritz, nms42, Sherlocks,[...]

#### Community Acknowledgments
- **Major Hackintosh Communities**:
  - [r/Hackintosh on Reddit](https://www.reddit.com/r/hackintosh/)
  - Hackintosh Discord servers.
  - InsanelyMac and AppleLife forums.
