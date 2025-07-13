# 🛠 INSTALL GUIDE – HP ZBook Fury Hackintosh (Quad Boot)

Welcome to the full guide for setting up macOS 26 Tahoe Beta on your HP ZBook Fury 15 G7 using OpenCore. This assumes you're starting from **Windows or Linux**, and want to create a **macOS USB installer manually**.

---

## ✅ Step-by-Step Installation Overview

1. **Download EFI folder** from this GitHub repo.
2. **Download macOS Sequoia Recovery** using `macrecovery.py` (see below).
3. **Format USB drive** as FAT32 + GUID using Disk Utility or Rufus.
4. **Copy the EFI** to the USB’s EFI partition.
5. **Copy the Recovery files** (`BaseSystem.dmg` and `.chunklist`) into `/Com.apple.recovery.boot/`.
6. **Edit `itlwm.kext` Info.plist** to pre-configure Wi-Fi for recovery.
7. **Change BIOS settings** (disable Secure Boot, VT-d, HP Sure Start, etc.)
8. **Boot USB and install macOS** using recovery.
9. ⚠️ If screen goes grey/black after Apple logo: **close lid, wait 5+ seconds, reopen**.
10. **Download OpenCore Configurator**: [https://mackie100projects.altervista.org/download-opencore-configurator/](https://mackie100projects.altervista.org/download-opencore-configurator/)
11. Use it to **mount your internal disk’s EFI partition**
12. **Copy the USB EFI folder** to your internal disk EFI partition
13. **Reboot**, go to BIOS > Boot Order
14. Set the **macOS disk first** so OpenCore loads from it (macOS won't boot without OpenCore)
15. You can boot into Windows/Linux from OpenCore menu (recommended)
16. For Linux booting, see: [`Docs/LinuxDualBoot.md`](./LinuxDualBoot.md) – enable `OpenLinuxBoot.efi` in OpenCore Configurator > UEFI > Drivers tab
17. After booting macOS, **run One-Key HiDPI** to patch screen + EDID.

---

## 📄 Required Files & Tools

* A USB stick (at least **4GB**, **FAT32** recommended)

  * If using a USB larger than **16 GB**, format with **Rufus** (FAT32)
* [Python 3.x](https://www.python.org/) installed on your system
* `macrecovery.py` from OpenCorePkg
* This repo's `EFI` folder

---

## 📀 Download macOS Recovery via macrecovery.py

### 1. Get `macrecovery.py`

* Download OpenCorePkg: [https://github.com/acidanthera/OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)
* Navigate to: `Utilities/macrecovery/`
* Copy `macrecovery.py` to your working folder

### 2. Download Recovery for Sequoia

```bash
python3 macrecovery.py -b Mac-937A206F2EE63C01 -m 00000000000000000
```

This downloads macOS **Sequoia recovery** (macOS 15 / version 26 base).

> ⚠️ **Note**: There is no official internet recovery yet for macOS 26 Tahoe Beta. You must first install Sequoia and upgrade to Tahoe from within macOS.

---

## 💻 Format USB for OpenCore

* Use **FAT32 + GUID Partition Table** (GPT) format
* If your USB is larger than 16 GB, use [**Rufus**](https://rufus.ie) in Windows

On macOS:

* Use Disk Utility > Show All Devices > Erase
* Format: **MS-DOS (FAT32)**
* Scheme: **GUID Partition Map**

---

## 🔌 Wi-Fi Required for Recovery (MANDATORY)

❗ **Without working Wi-Fi in Recovery, macOS installation will fail**.

* AirportItlwm does not work in Sequoia/Tahoe recovery
* Use `itlwm.kext` and edit `Info.plist`:

  1. Go to: `itlwm.kext/Contents/Info.plist`
  2. Navigate to: `IOKitPersonalities/itlwm/WiFiConfig/WiFi_1`
  3. Enter your real `SSID` and `Password`
  4. Save the file

✅ This connects Wi-Fi automatically in recovery mode.

---

## ⚙️ BIOS Configuration (IMPORTANT)

Before installation, change these BIOS settings:

* ❌ Disable **Secure Boot**
* ❌ Disable **VT-d**
* ❌ Disable **Fast Boot**
* ❌ Disable **HP Sure Start**
* ✅ Enable **UEFI Boot Mode**
* ✅ Enable **USB Boot Support**
* ✅ Set **Graphics Mode** to `Hybrid` or `iGPU only`

Save changes and reboot.

---

## 🚀 Boot & Install

1. Boot HP ZBook → press `Esc` or `F9`
2. Select the USB boot device
3. Choose **macOS Installer**
4. Use Disk Utility to format internal disk (APFS + GPT)
5. Continue with macOS install

⚠️ If screen goes grey/black after Apple logo (backlight on):

* Close the lid
* Wait at least **5 seconds**
* Reopen lid → macOS should appear

---

## 🧪 Post-Install – One-Key HiDPI

Run [**One-Key HiDPI**](https://github.com/xzhih/one-key-hidpi):

```bash
git clone https://github.com/xzhih/one-key-hidpi
cd one-key-hidpi
chmod +x hidpi.sh
./hidpi.sh
```

Steps:

* Press **2** → HiDPI + EDID
* Press **3** → MacBook Pro spoof
* Choose native or ideal resolution (e.g. `1920x1080` for 1080p)
* Confirm and reboot

✅ This fixes both GUI scaling and boot display detection.

---

Happy Hacking! ✨
