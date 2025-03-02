# Arch Linux Installation Guide

## 1. Set Console Keyboard Layout and Font (Optional)

```sh
loadkeys es
setfont ter-132n
```

## 2. Wipe NVMe Drive (Optional)

```sh
gdisk /dev/nvme0n1
# Press x (Extra functionality menu)
# Press z (Zap GPT and MBR)
# Confirm with y twice
```

Reboot your system before proceeding with the Arch installation:

```sh
reboot now
```

## 3. Connect to the Internet

```sh
iwctl
device list
station wlan0 get-networks
station wlan0 connect <SSID> 
```

## 4. Update System Packages

```sh
pacman -Sy
```

## 5. Partitioning NVMe SSD

### 5.1 Partition /dev/nvme0n1 (EFI, SWAP, ROOT & HOME)

```sh
gdisk /dev/nvme0n1
# EFI (1GB)
n → Enter → Enter → +1G → Enter
t → choose partition 1 → EF00
# SWAP (66GB)
n → Enter → Enter → +66G → Enter
t → choose partition 2 → 8200
# ROOT (40GB)
n → Enter → Enter → +40G → Enter
t → choose partition 3 → 8300
# HOME (Rest of Disk)
n → Enter → Enter → Enter (use rest of disk)
t → choose partition 4 → 8300
# Write changes
w → Enter
```

## 6. Format Partitions

```sh
mkfs.fat -F32 /dev/nvme0n1p1
mkswap /dev/nvme0n1p2
swapon /dev/nvme0n1p2
mkfs.ext4 /dev/nvme0n1p3
mkfs.ext4 /dev/nvme0n1p4
```

## 7. Mount Partitions

```sh
mount /dev/nvme0n1p3 /mnt
mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p1 /mnt/boot/efi
mkdir -p /mnt/home
mount /dev/nvme0n1p4 /mnt/home
```

## 8. Install Arch Linux (Base System)

```sh
pacstrap -K /mnt base linux linux-firmware base-devel neovim
```

## 9. Generate fstab

```sh
genfstab -U /mnt >> /mnt/etc/fstab
```

## 10. Change Root into New System

```sh
arch-chroot /mnt
```

## 11. Set Hostname

```sh
echo "archlinux" > /etc/hostname
```

## 12. Set Locale and Timezone

Edit `/etc/locale.gen`:

```sh
nvim /etc/locale.gen
# Uncomment "en_US.UTF-8 UTF-8" or your preferred locale
```

Generate locales:

```sh
locale-gen
```

Set system-wide locale:

```sh
echo "LANG=en_US.UTF-8" > /etc/locale.conf
export LANG=en_US.UTF-8
```

Set timezone:

```sh
ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
hwclock --systohc
```

## 13. Enable 32-bit Support (Multilib) & Pacman Eye Candy

Edit `/etc/pacman.conf`:

```sh
nvim /etc/pacman.conf
# Uncomment "Color" and add ILoveCandy under [options]
# Uncomment [multilib] section
```

Update package database:

```sh
pacman -Sy
```

## 14. Install and Enable NetworkManager

```sh
pacman -S networkmanager networkmanager-openvpn networkmanager-pptp networkmanager-vpnc
pacman -S wireless_tools wpa_supplicant ifplugd dialog
systemctl enable NetworkManager
```

## 15. Set Root Password

```sh
passwd
```

## 16. Install and Configure Zsh

```sh
pacman -S zsh
```

## 17. Create New User with Sudo Privileges

```sh
useradd -m -G wheel -s /bin/zsh rahul
passwd rahul
export EDITOR=nvim
visudo
# Uncomment "%wheel ALL=(ALL:ALL) ALL"
```

## 18. Configure Timesync

Edit `/etc/systemd/timesyncd.conf`:

```sh
nvim /etc/systemd/timesyncd.conf
[Time]
NTP=0.arch.pool.ntp.org 1.arch.pool.ntp.org 2.arch.pool.ntp.org 3.arch.pool.ntp.org
FallbackNTP=0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org
```

Enable service:

```sh
systemctl enable systemd-timesyncd.service
```

## 19. Enable SSD Trim Support

```sh
systemctl enable fstrim.timer
```

## 20. Install GRUB and EFI Boot Manager (Dual Boot Support)

```sh
pacman -S grub efibootmgr os-prober dosfstools mtools
```

## 21. Install GRUB to EFI

```sh
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
```

## 22. Install AMD Microcode and GPU Drivers

```sh
pacman -S amd-ucode
pacman -S mesa lib32-mesa
pacman -S vulkan-icd-loader lib32-vulkan-icd-loader
pacman -S vulkan-radeon lib32-vulkan-radeon
```

## 23. Enable OS Detection in GRUB

Edit `/etc/default/grub`:

```sh
nvim /etc/default/grub
# Uncomment or add this line:
GRUB_DISABLE_OS_PROBER=false
```

Generate GRUB configuration:

```sh
grub-mkconfig -o /boot/grub/grub.cfg
```

## 24. Exit and Shutdown

```sh
exit
umount -R /mnt
s
