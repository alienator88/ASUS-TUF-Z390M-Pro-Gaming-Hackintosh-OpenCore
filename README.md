# Asus TUF Z390M-Pro Gaming (Wifi) - OpenCore Configuration

<img align="right" src="https://user-images.githubusercontent.com/6263626/229322674-e7fa5918-dcff-4b24-8a16-43fbebd98bcf.png" alt="macOS Ventura" width="425">

[![macOS](https://img.shields.io/badge/macOS-Ventura-brightgreen.svg)](https://developer.apple.com/documentation/macos-release-notes)
[![OpenCore](https://img.shields.io/badge/OpenCore-0.9.0-blue)](https://github.com/acidanthera/OpenCorePkg)
[![License](https://img.shields.io/badge/license-MIT-purple)](/LICENSE)

<p align="center">
   <strong>Status: Maintained</strong>
   <br />
   <strong>Version: </strong>1.0
   <br />
   <a href="https://github.com/alienator88/ASUS-TUF-Z390M-Pro-Gaming-Hackintosh/releases"><strong>Download now »</strong></a>
   <br />
   <a href="https://github.com/valnoxy/t480-oc/issues">Report Bug</a>
   ·
   <a href="https://github.com/valnoxy/t480-oc/blob/main/CHANGELOG.md">View Changelog</a>
   ·
   <a href="https://www.youtube.com/watch?v=thYDWyJuUq4">YouTube Video</a>
  </p>
</p>
</br>
<br>
&nbsp;
&nbsp;

## Introduction
NOTE: There's another branch in this repo that was done for Mojave with Clover. I've since updated this repo with my current OpenCore/Ventura config

<details>
<summary><strong>💻 My Hardware</strong></summary>
<br>
These are the hardware components I use. This OpenCore configuration  <strong> should still work </strong> with your device, even if the components are not equal.

> **Note** Check the model of your WiFi & Bluetooth card. Intel cards should be compatible with AirportItlwm. If your card is from another manufacturer, please check if your card supports macOS.

| Category  | Component                            |
| --------- | ------------------------------------ |
| CPU       | Intel Core i7-9700K                  |
| Memory    | 32GB DDR4 3200(PC4 25600)            |
| Mobo      | Asus Z390M-Pro Gaming (Wi-Fi) LGA1151|
| GPU       | AMD Radeon RX 590                    |
| SSD       | Intel SSDSC2BW240A4                  |
| WiFi & BT | Intel Wireless-AC 9560               |
| Ethernet  | Intel I219-V                         |

</details>  

</details>

&nbsp;

## ⚠️ Disclaimer
This guide is only for the Asus Z390M-Pro Gaming/Wifi motherboard. I am NOT responsible for any harm you cause to your device. This guide is provided "as-is" and all steps taken are done at your own risk.

## Installation

<details>  
<summary><strong>📝 Requirements</strong></summary>
</br>

You must have the following items:
- Lenovo ThinkPad T480 (Obviously 😁).
- Access to a working Windows machine with Python installed.
- A pendrive with more than 4 GB (Remember that during the preparation we will format the flash drive to create the installation media).
- an Internet connection (recommended via Ethernet).
- 1-2 hours of your time.

</details>

<details>  
<summary><strong>⚙️ Preperation</strong></summary>
</br>

### Create the install media

First of all, you will need the install media of macOS. I will use [macrecovery](https://github.com/acidanthera/OpenCorePkg) to download and create the macOS Install media.

With macrecovery, the process is as follows:
- Download [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg) as a ZIP.
- Extract the OpenCorePkg-master.zip file.
- Open ```cmd.exe``` with Administrator privileges and change the directory to OpenCorePkg-master\Utilities\macrecovery.
- Enter the following command to download macOS:
```
# Big Sur (11)
python macrecovery.py -b Mac-42FD25EABCABB274 -m 00000000000000000 download

# Monterey (12)
python macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 download

# Ventura (13)
python macrecovery.py -b Mac-7BA5B2D9E42DDD94 download
```
- After the download succeeded, type ```diskpart``` and wait until you see ```DISKPART>```

- Plug-in your pendrive and type ```list disk``` to see your disk id.

- Select your pendrive by typing ```select disk <diskid>```

- Now we are gonna clean the pendrive and convert it to GPT. First, type ```clean``` and then ```convert gpt```.

>  **Note**: If an error occurred, try to convert again by typing ```convert gpt```.

- After the pendrive is clean and converted, we will create a new partition where we can put our files on. First, type ```create partition primary```, then select the new partition with ```select partition 1``` and format it ```format fs=fat32 quick```.

- Finally, mount your pendrive by typing ```assign```

- Now, close the Command Prompt and create the folder ```com.apple.recovery.boot``` on the pendrive. Copy ```OpenCorePkg-master\Utilities\macrecovery\BaseSystem.dmg``` and ```Basesystem.chunklist``` into that folder.

>  **Note**: If you can't find BaseSystem.dmg, use RecoveryImage.dmg and RecoveryImage.chunklist instead.

After the install media was created, we need to make the USB drive bootable.

### Configure and install OpenCore
Download the EFI folder from this repo, you will find the latest files under the release tab or just download the repo as it is. Move the folder to the root of your pendrive (e.g. J:\) and rename the folder to ```EFI```.

#### GenSMBIOS
We need a script, called [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS), to create fake serial number, UUID and MLB numbers. **This step is essential to have working iMessage, so do not skip it!**

The process is the following:

- Download GenSMBIOS as a ZIP, then extract it.
- Start GenSMBIOS.bat and use option ```1``` to download MacSerial.
- Choose option ```2```, to select the path of the config.plist file. It will be located in ```EFI -> OC``` folder.
- Choose option ```3```, and enter ```MacBookPro15,2``` as the machine type.
- Press ```Q``` to quit. Your config now should contain the requied serials.

#### Enter the proper ROM value
After adding serials to your config.plist, you have to add the computer's MAC address to the config.plist file. **This step is also essential to have a working iMessage, so do not skip it.** We need a Plist editior, to write the MAC address into the config.plist file. I used [ProperTree](https://github.com/corpnewt/ProperTree), since it works on Windows too. You have to change the MAC address value in the config.plist at

```PlatformInfo -> Generic -> ROM```

Delete the generic ```112233445566``` value, and enter your MAC address into the field, without any colons. Save the Plist file, and it is now ready to be written out to the EFI partition of your install media.

#### Default keyboard layout and language
The default keyboard layout and language is ```German```. To change the language, edit the value of ```NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> prev-lang:kbd``` to the value of your language. If your value contains an underscore "```_```", replace it with a hyphen "```-```". The value for English would be ```en-US:0```. You can find a list of all language values [here](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt).

##### ACPI patches
Please enable / disable the following patches depending on what is installed in your device.

| SSDT              | Affected device            | Description                                                |
| ----------------- | -------------------------- | ---------------------------------------------------------- |
| SSDT-ARPT.aml     | Broadcom cards             | Disable Broadcom card during sleep                         |
| SSDT-OFFTB.aml    | Thunderbolt                | Disable Thunderbolt                                        |
| SSDT-OFFGDGPU.aml | NVIDIA GeForce MX 150      | Disable NVIDIA GPU (necessary if installed)                |

### Install OpenCore
After you've finished with the neccesary tweaks, you have to copy the EFI folder to the EFI partition of your pendrive.

</details>

<details>  
<summary><strong>🚚 Installation</strong></summary>
</br>

### Prepare BIOS
The bios must be properly configured prior to installing macOS.
In Security menu, set the following settings:

-  `Security > Security Chip`: must be **Disabled**
-  `Memory Protection > Execution Prevention`: must be **Enabled**
-  `Virtualization > Intel Virtualization Technology`: must be **Enabled**
-  `Virtualization > Intel VT-d Feature`: must be **Enabled**
-  `Anti-Theft > Computrace -> Current Setting`: must be **Disabled**
-  `Secure Boot > Secure Boot`: must be **Disabled**
-  `Intel SGX -> Intel SGX Control`: must be **Disabled**
-  `Device Guard`: must be **Disabled**

In Startup menu, set the following options:

-  `UEFI/Legacy Boot`: **UEFI Only**
-  `CSM Support`: **No**

In Thunderbolt menu, set the following options:

-  `Thunderbolt BIOS Assist Mode`: **UEFI Only**
-  `Wake by Thunderbolt(TM) 3`: **No**
-  `Security Level`: **No**
-  `Support in Pre Boot Environment > Thunderbolt(TM) device`: **No**

Now you can go through the install.

### Install macOS
1. Boot from USB, press ```SPACE``` and select the USB drive inside of OpenCore ```"NO NAME (DMG)" or similar```.
>  **Note:** The first boot may take up to 20 minutes.
2. Wait for the macOS Utilities screen.
3. Select Disk Utility, select your disk and click erase. Give a name and choose **APFS** with **GUID Partition Map**.
4. After erasing, go back and select **Reinstall macOS** and follow the steps on your screen. The installation make take up to **2 hours**.
>  **Note:** Your PC will restart multiple times. Just boot from USB and select your disk inside of OpenCore. (named macOS Installer or the disk name).
5. Once you see the `Region selection` screen, you are good to proceed.
6. Create your user accound and everything else.

</details>

<details>  
<summary><strong>♻️ Upgrade macOS / Switch EFI</strong></summary>
</br>

If you plan to upgrade your macOS (or updating the EFI / switching to HeliPort), you'll need a different OpenCore configuation (EFI). Please follow these steps:

> Note: Download the desired macOS version in the Settings before following these steps, if you are connected via WiFi.

1. Download the newest release & [ProperTree](https://github.com/corpnewt/ProperTree) and extract it.
2. Start ProperTree and load the ```Config.plist``` on your EFI partition. (File -> Open)
> Note: You can mount your EFI partition by pressing ```ALT + SPACE```, typing Terminal and enter the following command: ```sudo diskutil mountDisk disk0s1```.
3. Now also load the new configuration file from the repo for the desired macOS installation (or HeliPort config). 
4. You should now have 2 ProperTree-windows open on your screen.
5. Go in both windows to ```Root -> PlatformInfo -> Generic```. Transfer ```MLB, ROM, SystemProductName, SystemSerialNumber and SystemUUID``` to the new config. 
6. Save the new config (File -> Save) and close both windows.
7. Now delete your existing EFI folder from the EFI partition and copy the new one to it. (Make sure that the Directorys ```Boot and OC``` are in ```EFI```).

If you want to upgrade macOS, download the desired macOS version in the Settings app and perform the upgrade like on a real Mac.

</details>

&nbsp;

## Post-install (optional)

<details>  
<summary><strong>💾 Install OpenCore to Hard drive</strong></summary>
</br>

1. Press `ALT + SPACE` and open terminal. Type `sudo diskutil mountDisk disk0s1` (where disk0s1 corresponds to the EFI partition of the main disk)
2. Open Finder and copy the EFI folder of your USB device to the main disk's EFI partition.
3. Unplug the USB device and reboot your laptop. Now you can boot macOS without your USB device.

</details>

<details>  
<summary><strong>✏️ Create a offline install media (Optional)</strong></summary>
</br>

In case of reinstalling macOS, a offline install media can save some time. You also don't need an Ethernet connection for the installation.
To create a offline install media, you need the following stuff: 

- macOS Installer from the App Store.
- A 16 GB pendrive (Keep in mind, during the preperation we will format the disk to create the install media).

Press `ALT + SPACE` and open Disk utility. Select your USB device and click erase. Name it `MyUSB` and choose **Mac OS Extended** with **GUID Partition Map**. After erasing the USB device, close Disk utility.

Now press `ALT + SPACE` and open terminal. Type the following command:

Big Sur:
```sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyUSB --downloadassets```

Monterey:
```sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyUSB --downloadassets```

After creating the install media, copy your EFI folder to the EFI partition of your USB device.


</details>

&nbsp;

## Status

<details>  
<summary><strong>✅ What's working</strong></summary>
</br>
 
- [X] Intel WiFi & Bluetooth (thanks to [itlwn](https://github.com/OpenIntelWireless/itlwm))
- [X] Brightness / Volume Control
- [X] Battery Information
- [X] Audio (Audio Jack & Speaker)
- [X] USB Ports & Built-in Camera
- [X] Graphics Acceleration
- [X] Trackpoint / Touchpad
- [X] Power management / Sleep
- [X] FaceTime / iMessage (iServices)
- [X] HDMI
- [X] Automatic OS updates
- [X] Handoff / Universal Clipboard
- [X] Sidecar (Cable) / AirPlay to Mac
- [X] SIP / FireVault 2
- [X] USB-C

</details>

<details>  
<summary><strong>⚠️ What's not working</strong></summary>
</br>

- [ ] Safari DRM ```Use Chromium powered Browser or Firefox to watch Amazon Prime Video, Netflix, Disney+ and others```
- [ ] AirDrop & Continuity
- [ ] Fingerprint Reader (Disabled with NoTouchID kext)
- [ ] Thunderbolt 3
- [ ] Sidecar Wireless
- [ ] Apple Watch Unlock

</details>

<details>  
<summary><strong>🔄 Not tested</strong></summary>
</br>

- [ ] WWAN
- [ ] Dualbooting Windows / Linux (with OpenCore)

</details>

&nbsp;

## ⭐️ Feedback
Did you find any bugs or just have some questions? Feel free to provide your feedback using the Discussions tab.

&nbsp;

## 📜 License

This repo is licensed under the [MIT License](https://github.com/valnoxy/t480-oc/blob/main/LICENSE).

OpenCore is licensed under the [BSD 3-Clause License](https://github.com/acidanthera/OpenCorePkg/blob/master/LICENSE.txt).

<hr>
<h6 align="center">© 2018 - 2022 valnoxy. All Rights Reserved. 
<br>
By Jonas Günner &lt;jonas@exploitox.de&gt;</h6>
<p align="center">
	<a href="https://github.com/valnoxy/t480-oc/blob/main/LICENSE"><img src="https://img.shields.io/static/v1.svg?style=for-the-badge&label=License&message=MIT&logoColor=d9e0ee&colorA=363a4f&colorB=b7bdf8"/></a>
</p>








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

 


