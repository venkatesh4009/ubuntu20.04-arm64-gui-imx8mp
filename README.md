# Ubuntu Server 20.04 ARM64 with GUI on i.MX8MP Board

This guide helps you install Ubuntu Server 20.04 ARM64 and configure a graphical user interface (GUI) on the **phyCORE-i.MX8MP** board.

📄 **Prepared by:** Venkatesh M  
✉️ **Email:** venkatesh.m@phytecembedded.in

---

## 📦 Project Overview

This project provides step-by-step instructions to:
- Flash a base SD card image.
- Extract and install an Ubuntu Server root filesystem.
- Set up a graphical desktop environment.
- Create a user account and configure system access.
- Enable display output and network connectivity.

---

## 🛠️ Required Hardware

- ✅ phyCORE-i.MX8MP Board
- ✅ 16GB+ MicroSD Card
- ✅ USB Keyboard and Mouse
- ✅ HDMI Cable and Display
- ✅ 24V Power Adapter
- ✅ Ethernet Cable
- ✅ Ubuntu Host Machine (x86 or ARM)

---

## 🔧 Required Host Packages

```bash
sudo apt update
sudo apt install gparted minicom qemu qemu-user-static
⬇️ Download and Flash Demo SD Image
bash
Copy
Edit
wget https://download.phytec.de/Software/Linux/BSP-Yocto-i.MX8MP/BSP-Yocto-NXP-i.MX8MP-PD22.1.0/images/ampliphy-vendor-xwayland/phyboard-pollux-imx8mp-2/phytec-qt5demo-image-phyboard-pollux-imx8mp-2.sdcard

sudo dd if=phytec-qt5demo-image-phyboard-pollux-imx8mp-2.sdcard of=/dev/sdX bs=4M status=progress && sync
Replace /dev/sdX with your SD card path (e.g., /dev/sdb).

💾 Resize and Format SD Partitions (Using GParted)
Unmount /dev/sdX2

Delete existing root partition.

Create new 14GB ext4 partition named root.

Apply changes.

📂 Extract Ubuntu Server Root Filesystem
bash
Copy
Edit
sudo mount /dev/sdX2 /mnt

wget https://cloud-images.ubuntu.com/releases/focal/release/ubuntu-20.04-server-cloudimg-arm64-root.tar.xz

sudo tar -xpf ubuntu-20.04-server-cloudimg-arm64-root.tar.xz -C /mnt
🔁 Bind Essential Mounts
bash
Copy
Edit
sudo cp /etc/resolv.conf /mnt/etc/resolv.conf
sudo mount --bind /dev /mnt/dev
sudo mount --bind /sys /mnt/sys
sudo mount --bind /proc /mnt/proc
sudo mount -t devpts devpts /mnt/dev/pts
🧑‍💻 Chroot and Install Desktop GUI
bash
Copy
Edit
sudo chroot /mnt

apt update
apt install ubuntu-desktop
📍 Configuration Prompts
Region: 6 (Asia)

Time Zone: 45 (Kolkata)

Keyboard Layout: 33 (English US)

Language: 1 (English US)

🌐 Locale and Essential Tools
bash
Copy
Edit
apt install locales
locale-gen en_US.UTF-8
update-locale LANG=en_US.UTF-8

apt install sudo build-essential net-tools network-manager iputils-ping nano lightdm xserver-xorg-video-fbdev xserver-xorg-video-vesa
👤 Create User and Grant Sudo
bash
Copy
Edit
adduser phytec
usermod -aG sudo phytec
Edit sudoers file:

bash
Copy
Edit
visudo
Replace:

bash
Copy
Edit
root ALL=(ALL:ALL) ALL
With:

bash
Copy
Edit
phytec ALL=(ALL:ALL) ALL
✅ Verify and Cleanup
bash
Copy
Edit
su - phytec
sudo apt update && sudo apt upgrade
exit
Unmount:

bash
Copy
Edit
sudo umount /mnt/dev /mnt/proc /mnt/sys /mnt
sudo sync
🖥️ First Boot on Board
Insert SD card into i.MX8MP board.

Connect UART terminal (e.g., Minicom).

Login as phytec.

🧾 Configure X11 for Display
bash
Copy
Edit
sudo nano /etc/X11/xorg.conf
Paste the modeline configuration [from PDF, or see full file].

Restart GUI:

bash
Copy
Edit
sudo systemctl restart lightdm
📛 Rename Hostname
bash
Copy
Edit
sudo nano /etc/hostname
# Change to: ubuntu
🌐 Enable Network (eth1)
bash
Copy
Edit
sudo ip link set eth1 up
sudo dhclient eth1
ping google.com
📂 Folder Structure in Repository
text
Copy
Edit
.
├── Installing Ubuntu Server image 20.04 ARM64 with GUI on i.MX8MP Board.pdf
├── ubuntu-20.04-server-cloudimg-arm64-root.tar.xz
├── i.MX8MP_ubuntu_server_20.04_GUI_rootfs_tar/
├── .gitignore
Note: .gitignore excludes large tarballs from upload.

📌 License
This guide is open for educational use. Contact the author for commercial or support queries.

🧑‍💼 Author
Venkatesh M
Embedded System Engineer
📧 venkatesh.m@phytecembedded.in
