# 🛠 INSTALL GUIDE – HP ZBook Fury Hackintosh (Quad Boot)

Welcome to the full guide for setting up macOS 26 Tahoe Beta on your HP ZBook Fury 15 G7 using OpenCore. This assumes you're starting from **Windows or Linux**, and want to create a **macOS USB installer manually**.

---

## 📄 Required Files & Tools

- A USB stick (at least **4GB**, **FAT32** recommended)
  - If using a USB larger than **16 GB**, format with **Rufus** (FAT32)
- [Python 3.x](https://www.python.org/) installed on your system
- `macrecovery.py` from OpenCorePkg
- This repo's `EFI` folder

---

## 📀 Download macOS Recovery via macrecovery.py

You do **not** need a full macOS install app. We'll download the recovery files directly using OpenCore's tools.

### 1. Get `macrecovery.py`

- Download OpenCorePkg: [https://github.com/acidanthera/OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)
- Navigate to: `Utilities/macrecovery/`
- Copy `macrecovery.py` to your working folder

### 2. Download Recovery for Sequoia

```bash
python3 macrecovery.py -b Mac-937A206F2EE63C01 -m 00000000000000000
```

This downloads macOS **Sequoia recovery** (macOS 15 / version 26 base).

> ⚠️ **Note**: There is no official internet recovery yet for macOS 26 Tahoe Beta. You must first install Sequoia and upgrade to Tahoe from within macOS.

---

## 💻 Format USB for OpenCore

### ✅ Best Practices:

- Use **FAT32 + GUID Partition Table** (GPT) format
- If your USB is larger than 16 GB, use [**Rufus**](https://rufus.ie) in Windows to create GPT + FAT32 manually

### On Windows (Rufus):

- Select your USB
- Partition Scheme: **GPT**
- File system: **FAT32**

### On macOS:

Use Disk Utility > Show All Devices > Erase:

- Format: **MS-DOS (FAT32)**
- Scheme: **GUID Partition Map**

---

## 🔧 Create USB Installer

1. Mount your USB drive's EFI partition (on Windows: use MountEFI; on macOS: use `diskutil`)
2. Copy your `EFI` folder from this repo into the EFI partition
3. Copy the `BaseSystem.chunklist` and `BaseSystem.dmg` downloaded from `macrecovery.py` into:
   ```
   /Com.apple.recovery.boot/
   ```
   Create the folder manually if needed.

---

## 🚀 Boot & Install

1. Boot your HP ZBook and hit `Esc` or `F9` to enter the boot menu
2. Choose your USB
3. Select **macOS Installer**
4. Use Disk Utility to erase your internal NVMe (APFS + GPT)
5. Proceed with install

---

## 🧪 Post-Install

- Copy `EFI` folder to internal EFI partition after install
- Run [**One-Key HiDPI**](https://github.com/xzhih/one-key-hidpi)
  - Use **4K + EDID patch** for full boot + GUI fix
- If you see a black screen after Apple logo loading:
  - Close the lid
  - Reopen it
  - macOS should appear and boot normally

---

## 🚫 Things to Remember

- This guide assumes you’re installing Sequoia, then upgrading to Tahoe
- Tahoe Internet Recovery **does not exist** yet
- Your USB must be **GPT + FAT32** or OpenCore will **fail to boot**

---

## 🔌 Wi-Fi Required for Recovery (MANDATORY)

❗ **Important**: Without working Wi-Fi in Recovery, macOS installation will **fail** to proceed.

- **AirportItlwm** does **not** work in Sequoia or Tahoe recovery mode.
- You **must** use `itlwm.kext` and edit its **Info.plist** to preconfigure Wi-Fi:
  1. Navigate to: `itlwm.kext/Contents/Info.plist`
  2. Go to: `IOKitPersonalities/itlwm/WiFiConfig/WiFi_1`
  3. Change the `SSID` and `Password` keys to match your Wi-Fi network
  4. Save and re-sign the kext if needed (optional but recommended)

> 🛠 You can edit with `PlistEdit Pro`, `Xcode`, or even `nano`.

✅ This ensures that Wi-Fi connects **automatically during recovery**, allowing internet-required components to function.

---

Happy Hacking! ✨

