# Asus TUF Z390M-Pro Gaming (Wifi) - OpenCore Configuration

<img align="right" src="https://user-images.githubusercontent.com/6263626/229322674-e7fa5918-dcff-4b24-8a16-43fbebd98bcf.png" alt="macOS Ventura" width="425">

[![macOS](https://img.shields.io/badge/macOS-Ventura-brightgreen.svg)](https://developer.apple.com/documentation/macos-release-notes)
[![OpenCore](https://img.shields.io/badge/OpenCore-0.9.0-blue)](https://github.com/acidanthera/OpenCorePkg)
[![License](https://img.shields.io/badge/license-MIT-purple)](/LICENSE)

<p align="center">
   <strong>Status: </strong>Maintained
   <br />
   <strong>Repo Version: </strong>v1.0
   <br />
   <strong>MacOS Version: </strong>Ventura 13.3
   <br />
   <a href="https://github.com/alienator88/ASUS-TUF-Z390M-Pro-Gaming-Hackintosh/releases"><strong>Download</strong></a>
   ·
   <a href="https://github.com/alienator88/ASUS-TUF-Z390M-Pro-Gaming-Hackintosh/blob/ventura/CHANGELOG.md">Changelog</a>
  </p>
</p>
</br>
<br>
&nbsp;
&nbsp;

## Introduction
**NOTE**: There's another branch in this repo that was done for Mojave with Clover a few years back. 
I've since updated this repo with my current OpenCore/Ventura config

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

&nbsp;

## Installation

<details>  
<summary><strong>📝 Requirements</strong></summary>
</br>

You must have the following items:
- The motherboard listed in this repo (Obviously 😁).
- Access to a working Windows machine with latest [Python](https://www.python.org/downloads/) installed.
- A usb drive with more than 4 GB (Remember that during the preparation we will format the flash drive to create the installation media).
- An Internet connection (recommended via Ethernet).
- 1-2 hours of your time.

</details>

<details>  
<summary><strong>⚙️ Preparation</strong></summary>
</br>

### Create the install media

First of all, you will need the install media of macOS. I will use macrecovery to download and create the macOS Install media.

With macrecovery, the process is as follows:
- Download [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg) as a ZIP.
- Extract the OpenCorePkg-master.zip file.
- Open ```cmd.exe``` with Administrator privileges and change the directory to OpenCorePkg-master\Utilities\macrecovery.
- Enter the following command to download macOS:
```
# Ventura (13)
python macrecovery.py -b Mac-7BA5B2D9E42DDD94 download
```
- After the download finishes, type ```diskpart``` and wait until you see ```DISKPART>```

- Insert your usb drive and type ```list disk``` to see your disk id.

- Select your usb drive by typing ```select disk <diskid>```

- Now we are going to clean the usb drive and convert it to GPT. First, type ```clean``` and then ```convert gpt```.

>  **Note**: If an error occurred, try to convert again by typing ```convert gpt```.

- After the usb drive is clean and converted, we will create a new partition where we can put our files on. First, type ```create partition primary```, then select the new partition with ```select partition 1``` and format it ```format fs=fat32 quick```.

- Finally, mount your usb drive by typing ```assign```

- Now, close the Command Prompt and copy ```OpenCorePkg-master\Utilities\macrecovery\com.apple.recovery.boot``` folder into the root of the usb drive.

After the install media is created, we need to make the USB drive bootable.

### Configure and install OpenCore
Download the EFI folder from this repo, you will find the latest files under the release tab or just download the repo as is. Move the EFI folder to the root of your usb drive (e.g. E:\EFI).
> At this point you should have 2 folders on your usb drive (EFI and com.apple.recovery.boot)

#### GenSMBIOS
Now we need to create a fake serial number, UUID and MLB numbers with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS). **This step is essential to have working iMessage, don't skip it!**

The process is the following:

- Download GenSMBIOS as a ZIP, then extract it.
- Start GenSMBIOS.bat and use option ```1``` to download MacSerial.
- Choose option ```2```, to select the path of the config.plist file on the usb drive. It will be located at ```EFI -> OC -> config.plist```.
- Choose option ```3```, and enter ```iMac19,1``` as the machine type.
- Press ```Q``` to quit. Your config now should contain the required serials.

#### Enter the proper ROM value
After adding serials to your config.plist, you have to add the computer's MAC address. **This step is also essential to have a working iMessage, so do not skip it.** We need a Plist editor to write the MAC address into the config.plist file. I use [ProperTree](https://github.com/corpnewt/ProperTree), since it works on Windows too. You have to change the MAC address value in the config.plist at: ```PlatformInfo -> Generic -> ROM```

Delete the current value and enter your MAC address into the field, without any colons. 
Example:
> You can get your MAC address with ```ipconfig /all``` in Command Prompt, grab one from your ethernet or wifi adapter. Mine for example is ```04:d4:c4:01:aa:96```, so I typed the value as ```04d4c401aa96```.

#### Default keyboard layout and language
The default keyboard layout and language is ```English```. To change the language, edit the value of ```NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> prev-lang:kbd``` to the value of your language. If your value contains an underscore "```_```", replace it with a hyphen "```-```". The value for English is ```en-US:0```. You can find a list of all language values [here](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt).

Save the config.plist file.

### Prepare BIOS
The BIOS must be properly configured prior to installing macOS.
Set the following settings:

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

Now you can go through the install.

</details>


<details>  
<summary><strong>🚚 Installation</strong></summary>
</br>

### Install macOS
1. Boot the computer and press F8 to show the boot device picker. Select the usb we have configured with OpenCore and choose ```"NO NAME (DMG)" or similar```. Might have a different name for yours.
>  **Note:** The first boot may take up to 20 minutes.
2. Wait for the macOS Utilities screen.
3. Select Disk Utility, select your disk and click erase. Give a name and choose **APFS** with **GUID Partition Map**.
4. After erasing, go back and select **Reinstall macOS** and follow the steps on your screen. The installation may take up to **2 hours**.
>  **Note:** Your PC will restart multiple times. Just boot from the usb drive each time and select your disk inside of OpenCore. (```named macOS Installer or the disk name```).
5. Once you see the `Region Selection` screen, you are in the home stretch.
6. Create your user account and set everything else up.

</details>

&nbsp;

## Post-Install

<details>  
<summary><strong>💾 Install OpenCore to Hard drive</strong></summary>
</br>

1. Mount your main disk EFI partition with [MountEFI](https://github.com/corpnewt/MountEFI).
2. Open Finder and copy the EFI folder from your usb device to the main disk's EFI partition.
3. Unplug the usb device and reboot your computer. Now you can boot macOS without the usb device.

</details>

<details>  
<summary><strong>✏️ Create offline install media (Optional)</strong></summary>
</br>

In case of reinstalling macOS, the offline install media can save some time. You also don't need an Ethernet connection for the installation.
To create the offline install media, you need the following: 

- macOS Installer from the App Store.
- A 16 GB usb drive (Keep in mind, during the preparation we will format the disk to create the install media).

Open Disk Utility. Select your USB device and click erase. Name it `macOSInstaller` and choose **Mac OS Extended** with **GUID Partition Map**. After erasing the usb device, close Disk Utility.

Now open Terminal. Type the following command:

Ventura:
```sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/macOSInstaller --downloadassets```

This will take a while to write to the usb. After creating the install media, mount the EFI partitions from your main disk and this new usb install media with [MountEFI](https://github.com/corpnewt/MountEFI). Copy your working EFI folder to the EFI partition of this install media.

</details>

<details>  
<summary><strong>♻️ Upgrade macOS</strong></summary>
</br>

If you plan to upgrade your macOS, follow these steps:

1. Download the desired macOS version or security updates from inside the Settings app.
2. Perform the upgrade as normal like a real mac.

</details>

&nbsp;

## Status

<details>  
<summary><strong>✅ What's Working</strong></summary>
</br>
 
- [X] Intel WiFi (thanks to [airportitlwn](https://github.com/OpenIntelWireless/itlwm))
- [X] Audio (Audio Jack & Speaker)
- [X] USB Ports
- [X] AMD Radeon RX 590
- [X] Power management / Sleep
- [X] FaceTime / iMessage (iServices)
- [X] DisplayPort / HDMI
- [X] Automatic OS updates
- [X] SIP / FireVault 2
- [X] USB-C

</details>

<details>  
<summary><strong>⚠️ What's Not Working</strong></summary>
</br>

- [ ] Intel Bluetooth (Causes a few of the items in the Not Tested section to most likely not work also)
- [ ] Safari DRM ```Use Chromium powered Browser or Firefox to watch Amazon Prime Video, Netflix, Disney+ and others```


</details>

<details>  
<summary><strong>🔄 Not Tested</strong></summary>
</br>

- [ ] AirDrop & Continuity
- [ ] Handoff / Universal Clipboard
- [ ] Sidecar Wireless
- [ ] Apple Watch Unlock

</details>

&nbsp;

## 📜 License

This repo is licensed under the [MIT License](https://github.com/alienator88/ASUS-TUF-Z390M-Pro-Gaming-Hackintosh/blob/Ventura/LICENSE).

OpenCore is licensed under the [BSD 3-Clause License](https://github.com/acidanthera/OpenCorePkg/blob/master/LICENSE.txt).













 


