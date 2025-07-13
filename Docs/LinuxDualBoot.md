# 🐧 Linux Dual Boot Setup with OpenCore

If you're planning to dual or quad boot Linux alongside macOS, Windows, or other OSes with OpenCore, this guide will help you boot Linux cleanly from the OpenCore menu.

---

## ✅ Recommended Setup

* Linux installed to a **separate NVMe or SATA drive**
* UEFI boot enabled (no legacy boot)
* OpenCore already installed and working

---

## 📦 Required: OpenLinuxBoot.efi

To support native Linux booting, enable **OpenLinuxBoot.efi** in OpenCore:

### 1. Download / Enable the Driver

* Open **OpenCore Configurator**
* Go to **UEFI > Drivers**
* Add or enable `OpenLinuxBoot.efi`

If it's not present:

* Get it from the [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases) > `X64/Drivers/`
* Place it in `EFI/OC/Drivers/`

---

## 🧭 Configure Boot Entries

Linux distros often place their bootloader in:

```
/EFI/ubuntu/shimx64.efi
/EFI/debian/grubx64.efi
```

OpenLinuxBoot will auto-detect these **if you use UEFI installation**.

> ⚠️ If OpenLinuxBoot fails to show Linux, use `bless` or `efibootmgr` inside Linux to register EFI entries.

---

## 💡 Tips

* Keep Linux on its **own EFI** if possible to avoid conflicts.
* To reduce clutter in OpenCore boot menu, enable **Hide Auxiliary** in the config and use hotkey `Esc` to toggle.
* If using GRUB, ensure it is correctly installed to the EFI partition.

---

## ✅ Final Steps

After you enable `OpenLinuxBoot.efi`:

* Reboot
* OpenCore will auto-detect supported Linux installations and show them in the boot menu

You can now cleanly boot Ubuntu, Debian, Arch, Fedora, or any UEFI-based Linux install 🎉

---

Happy Booting! 🚀
