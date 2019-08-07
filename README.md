# ASUS-TUF-Z390M-Pro-Gaming-Hackintosh

![screenshot](https://user-images.githubusercontent.com/6263626/62662534-a12b0d00-b931-11e9-9ce4-6f3ebd1fa4f0.png)

- EmuVariable is required for this (and most other Z390 boards), as the BIOS for these boards (which are based on a newer version of AMI Aptio) are not __*yet*__ compatible with AptioMemoryFix in regards to nvram writing. So native nvram does not work under macOS currently.  
- You may use the drivers and kexts I have posted here, but it's probably a better idea to download them new from the links below to make sure you have the latest.  
- For Mojave 10.14.6 update, I had to clean install clover again on the drive and drop all the files back in. FYI just in case.


### Fixes
```
ACPI patch to fix "AMFILoadTrustedKeysFromNVRam: no nvram variable" during installer boot.
```
```
Fix Phase 1 of installer stuck at 2 minutes remaining.
```
```
Fix Phase 2 of installer stuck at 13 minutes remaining.
```

# BIOS Settings

__AI Tweaker:__
  * Ai Overclock Tuner: XMP I (just for speed)
  * ASUS MultiCore Enhancement: Auto

__Advanced:__
  * Platform Misc Configuration (lower temperature of Radeon):
    * PCI Express Native Power Management: Enabled
    * Native ASPM: Enabled
    * PCH DMI ASPM: Auto
    * ASPM: Auto
    * L1 Substates: L1.1 & L1.2
    * PEG - ASPM: Auto
  * CPU Configuration:
    * Intel (VMX) Virtualization Technology: Enabled
    * Hyper-Threading: Enabled
    * CPU-Power Management Control:
      * Intel SpeedStep: Enabled
      * Intel Speed Shift: Enabled
      * CFG Lock: Disabled
  * System Agent (SA) Configuration
    * VT-d: Disabled
    * Above 4G Decoding: Disabled
    * Graphics Configuration (Video Card only without Intel HD 630 iGPU hardware acceleration):
      * Primary Display: PEG
    * Graphics Configuration (Video Card + Intel HD 630 iGPU hardware acceleration):
      * Primary Display: PCIE
      * iGPU Multi-Monitor: Enabled
      * DVMT Pre-Allocated: 32M
      * RC6(Render Standby): Disabled 
  * PCH Configuration
    * IOAPIC 24-119 Entries: Enabled
  * PCH Storage Configuration
    * SATA Controller(s): Disabled (if you have SDD NVMe)
  * Onboard Devices Configuration
    * HD Audio: Enabled
    * USB power delivery in Soft Off state (S5): Disabled
    * Serial Port Configuration
      * Serial Port: Off
  * USB Configuration
    * Legacy USB Support: Enabled
    * XHCI Hand-off: Enabled
  * Network Stack Configuration
    * Network Stack: Disabled

__Boot:__
  * Boot Configuration
    * Fast Boot: Disabled
  * CSM (Compatibility Support Module)
    * Launch CSM: Disabled
  * Secure Boot
    * OS Type: Other OS
    

# Drivers
[ApfsDriverLoader-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[AptioInputFix-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[AudioDxe-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[DataHubDxe-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[EmuVariableUefi-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[FSInject-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[OsxAptioFixDrv-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI) (AptioMemoryFix-64 does not work well during install for this board)  
[HFSPlus-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[PartitionDxe-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[SMCHelper-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)
[UsbKbDxe-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  

# Kexts
[AppleALC](https://github.com/acidanthera/AppleALC/releases)   
[FakeSMC](https://github.com/RehabMan/OS-X-FakeSMC-kozlek/releases) (VirtualSMC does not work well with this board yet)   
[IntelMausiEthernet](https://github.com/acidanthera/IntelMausi/releases)   
[Lilu](https://github.com/acidanthera/Lilu/releases)   
[USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)   
[WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)   

# Config.plist

>NOTE: I removed my Serial Number/SmUUID/CustomUUID from the SMBIOS & System Parameters section. Generate your own in Clover Configurator. 

#### Credits
[Tonymacx86](https://www.tonymacx86.com/)   
[r/hackintosh](https://www.reddit.com/r/hackintosh/)   
[Hackintosher](https://hackintosher.com/)

