# ERYING-i9-11900H-ES-Hackintosh
**TEST-ONLY** EFI for the Erying M-ATX motherboard with the i9 11900H/11980HK ES CPU

<img align="right" src="./resources/mobo.png"
alt="erying mobo" width="350">


[![OpenCore](https://img.shields.io/badge/OpenCore-0.9.7-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
![macOSver](https://img.shields.io/badge/macOS-Ventura-brightgreen.svg)

### ⚠️ Disclaimer:
- This EFI is meant to be for **TEST-ONLY**, I strongly do not recommend trying to use this efi for daily drive
- Use this EFI as an **example**, remember to always make your efi so you know how to fix it in case something breaks...
- This EFI will probably not be constantly updated to the latest version of opencore especially if no major changes are made
- Please read the whole readme before proceeding
- The the efi is not cleaned from garbage or useless things
- Please, Change the **[SMBIOS serial number](https://github.com/Forte500/ERYING-i9-11900H-ES-Hackintosh/tree/main#generating-smbios)** before using the EFI
##

### ⚠️ BIOS Disclaimer:
<img align="right" src="./resources/splash.jpg"
alt="10729 lightning bios boot logo" width="210">
This EFI is built based on the "Lightning BIOS", if you are still using the default bios be aware that you probably need to redo the USB Mapping process, + audio didn't worked at all for me with every codec for ALC 897.

If you decide to flash the lightning bios I **strongly recommend** to already have a bios programmer such as **CH341A**, It has happened to some people to have [**bricked their motherboard**](https://www.reddit.com/r/EryingMotherboard/comments/15f132j/well_i_bricked_my_motherboard/) during this process.

**[Lightning BIOS reddit post](https://www.reddit.com/r/EryingMotherboard/comments/12xg3n6/thoughts_on_the_more_powerful_bios/)**

### 💻 My Hardware
| Component      | Brand/info                                              |
|----------------|---------------------------------------------------------|
| **CPU**        | `Intel Core i9 11900H ES Tiger Lake-H`                  |
| **RAM**        | `16gb 2*8 Crucial ballistix 3200mhz`                    |
| **Storage 1**  | `500GB Crucial MX500`                                   |
| **Storage 2**  | `Crucial P3 Plus 1TB` (for Windows)                     |
| **iGPU**       | `Intel UHD graphics xe 32eus (vesa mode/disabled)`      |
| **dGPU**       | `XFX RX 6600 XT QICK 308`                               |
| **Audio**      | `ALC897 - layout 99 or 98`, `layout 67(custom AppleALC needed)`|
| **Ethernet**   | `Realtek Gigabit Ethernet`                              |
| **PSU**        | `Cooler Master MWE 600W White V2`                       |
| **WiFi**       | N/A (can be added)                                      |

### ✅️ What works</strong></summary>

- GPU graphics acceleration RX 6600 XT with Resizable Bar Enabled
- Audio (sort of, check below for more info)
- Fan Readings & Control
- USB ports
- Ethernet
- Safari DRM Video Playback 
  

### ❌️ What doesn't work

- Power management (WIP)
- some audio ports (check below for more info)
- Intel iGPU (no drivers)
- Sleep (probably related to Bios)
- **+ other things**

### ⚠️ Known Issues
<details>
<summary><strong>🔊 Audio</strong></summary>
  <br>
  
Apparently there is no fully working audio layout for this erying board

|          | layout 67 (custom) | layout 98 | Layout 99 |
| ------ | --- | --- | --- |
| Rear line out (green)  | ✅️ | ✅️ | ✅️ |
| Rear line in (blue)  | ✅️ | ✅️ | ❌️ |
| Rear Mic in (Pink)  | ❌️ | ❌️ | ✅️ |
| Front Headphone out  | ❌️ | ❌️ | ✅️ |
| Front Mic in  | ❌️ | ❌️ | ❌️ |

</details>

- Possible Performance issues CPU-side: random TDP drops from 70W to 35W under load 
- ~After Verbose, there's a blackscreen for 2-3 minutes when booting off amd gpu~
should have been solved with the `-wegswitchgpu` bootarg, which disable the internal GPU when an external GPU is installed

### 👨‍🔧 For those who haves an unsupported GPU:
<details>
<summary><strong></strong></summary>
  <br>
  
You can still try out MacOS without graphics acceleration by using the Intel igpu, add `-wegnoegpu` to your bootargs and remove `-wegswitchgpu`
</details>

## ⚙️ Setup
<details>
<summary><strong>🔧 BIOS Settings</strong></summary>
  <br>

**Advanced TAB**
- `SATA Configuration > SATA Mode Selection` must be set to **AHCI**
- `Graphics Configuration > VT-d`: should be **Enabled**
- `Graphics Configuration > Internal Graphics`: should be **Enabled**
- `Graphics Configuration > Primary Display`: should be set to **Auto**
- `PCI Subsystem Settings > Above 4G Decoding & Re-Size BAR Support`: must be **Both Enabled**
- `USB Configuration > XHCI Hand-off`: must be **Enabled**

**Startup TAB**
- `Fast Boot`: **Disabled**

**Security TAB**
- `Secure Boot > Secure Boot`: must be **Disabled**

</details>

<details>
<summary><strong>🗒 config.plist edits</strong></summary>
  <br>
  
- ### Default keyboard layout and language:
 *optional:* edit `prev-lang:kbd` in config.plist in order to match your keyboard layout and language (mainly relevant in recovery and installation)
  
  default is (<>) which will force the Language Picker to appear at first boot up.
  More info [here](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake.html#add-4) at the bottom of `7C436110...` etc.
  
  
- ### Generating SMBIOS:

We need a tool, called [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) from corpnewt, to generate a fake serial number, UUID and MLB for our Hackintosh.

**this step is mandatory to get working iServices, be careful not to make any mistakes**

1. Download GenSMBIOS from the link above as .ZIP, then extract it.
2. Start GenSMBIOS and select option `1` to download and install MacSerial
3. Select option `2` and open the `config.plist` located under `EFI > OC`
4. Select option `3` and enter `iMacPro1,1`, serials will be generated
5. **IMPORTANT:** reminder that you need an **invalid serial!** to check copy and paste the second part saying `Serial: XXXXX..` in [Apple's Check Coverage Page](https://checkcoverage.apple.com/), if you get a red message saying "We're sorry, we're unable to check coverage for this serial number."
 then, you're good to go! Otherwise, go back and restart from step `2` (more info [here](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#serial-number-validity))

