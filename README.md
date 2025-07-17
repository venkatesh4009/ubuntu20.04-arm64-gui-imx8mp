# Ubuntu 20.04 ARM64 with GUI on i.MX8MP Board

This project provides a detailed step-by-step guide to install **Ubuntu Base 20.04 ARM64** with a **Graphical User Interface (GUI)** on the **phyCORE-i.MX8MP** board.


---

## 📦 Project Structure

This repository includes:
- Setup documentation (`.pdf`)
- Scripts and commands used during the process
- `.gitignore` to exclude large rootfs image files

**Note:** The large root filesystem image (`ubuntu-base-20.04.1-base-arm64.tar.gz`) is excluded from the repository to keep it lightweight.

---

## 🧰 Hardware Requirements

- ✅ phyCORE-i.MX8MP Board
- ✅ MicroSD Card (≥ 16GB)
- ✅ HDMI Display and Cable
- ✅ USB Keyboard and Mouse
- ✅ Power Adapter (24V)
- ✅ Ethernet Cable
- ✅ Host PC running Ubuntu

---

## 📥 Required Downloads

1. **Yocto Demo Image** for SD card preparation  
   👉 [Download Link](https://download.phytec.de/Software/Linux/BSP-Yocto-i.MX8MP/BSP-Yocto-NXP-i.MX8MP-PD22.1.0/images/ampliphy-vendor-xwayland/phyboard-pollux-imx8mp-2/phytec-qt5demo-image-phyboard-pollux-imx8mp-2.sdcard)

2. **Ubuntu Base Image 20.04 ARM64**  
   👉 [Download Link](https://cdimage.ubuntu.com/ubuntu-base/releases/20.04/release/ubuntu-base-20.04.1-base-arm64.tar.gz)

---

## ⚙️ Installation Steps Overview

### 1. Install Required Packages on Host PC

sudo apt update && sudo apt upgrade
sudo apt install gparted qemu qemu-user-static minicom
2. Prepare the SD Card
Use dd to write the .sdcard image to the SD card

Resize root partition using GParted

3. Mount and Extract Ubuntu Base

sudo mount /dev/sdX2 /mnt
sudo tar -xvf ubuntu-base-20.04.1-base-arm64.tar.gz -C /mnt
4. Chroot and Configure Ubuntu

sudo chroot /mnt
apt update && apt install ubuntu-desktop
5. Add a New User

adduser phytec
usermod -aG sudo phytec
6. Configure Display (X11)
Edit /etc/X11/xorg.conf and add resolution modelines and preferred driver (fbdev or vesa).

🧪 First Boot on i.MX8MP Board
Insert SD card and power on the board

Use minicom or putty via UART (e.g., /dev/ttymxc0)

Login with user phytec and the password you created

🌐 Network Setup

sudo ip link set eth1 up
sudo dhclient eth1
ping google.com
🔁 Reboot and Login
After everything is configured:


sudo reboot
Login as user phytec with GUI ready.


✉️ Contact
For support or questions, please contact:

Venkatesh M
Embedded Systems Engineer
📧 venkatesh.m@phytecembedded.in
