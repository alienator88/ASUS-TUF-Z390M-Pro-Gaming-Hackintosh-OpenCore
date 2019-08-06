# ASUS-TUF-Z390M-Pro-Gaming-Hackintosh

>I needed to add all this information together for this board as it took me a couple days of scavenging for drivers, kexts, patches and bios settings.  


### Fixes

1. ACPI patch to fix "AMFILoadTrustedKeysFromNVRam: no nvram variable" during installer boot.
2. Driver to fix Phase 1 of installer stuck at 2 minutes remaining.
3. Driver to fix Phase 2 of installer stuck at 13 minutes remaining.

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
[EmuVariableUefi-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[FSInject-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[OsxAptioFixDrv-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI) (AptioMemoryFix-64 does not work well during install for this board)  
[HFSPlus-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  
[SMCHelper-64.efi](https://github.com/athlonreg/Clover_Build/tree/master/drivers64UEFI)  

# Kexts
[AppleALC](https://github.com/acidanthera/AppleALC/releases)   
[FakeSMC](https://github.com/RehabMan/OS-X-FakeSMC-kozlek/releases) (VirtualSMC does not work well with this board yet)   
[IntelMausiEthernet](https://github.com/acidanthera/IntelMausi/releases)   
[Lilu](https://github.com/acidanthera/Lilu/releases)   
[USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)   
[WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)   

# Config.plist

Find the file attached above.  
NOTE: I removed the serial number from the SMBIOS section. Generate your own in Clover Configurator.
