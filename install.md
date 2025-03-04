# Arch Linux Installation Guide

## 1. Wipe NVMe Drives

Wipe `/dev/nvme0n1`:

```sh
gdisk /dev/nvme0n1
# Press x (Extra functionality menu)
# Press z (Zap GPT and MBR)
# Confirm with y twice
```

Wipe `/dev/nvme1n1`:

```sh
gdisk /dev/nvme1n1
# Press x (Extra functionality menu)
# Press z (Zap GPT and MBR)
# Confirm with y twice
```

Reboot your system before proceeding with the Arch installation:

```sh
reboot now
```

## 2. Connect to the Internet

```sh
iwctl
device list
station wlan0 get-networks
station wlan0 connect <SSID> --passphrase <PASSWORD>
```

## 3. Update System Packages

```sh
pacman -Sy
```

## 4. Partitioning NVMe SSD

### 4.1 Partition /dev/nvme0n1 (EFI, SWAP, ROOT & HOME)

```sh
gdisk /dev/nvme0n1
# EFI (1GB)
n → Enter → Enter → +1G → Enter
t → choose partition 1 → EF00
# SWAP (4GB)
n → Enter → Enter → +4G → Enter
t → choose partition 2 → 8200
# ROOT (32GB)
n → Enter → Enter → +32G → Enter
t → choose partition 3 → 8300
# HOME (Rest of Disk)
n → Enter → Enter → Enter (use rest of disk)
t → choose partition 4 → 8300
# Write changes
w → Enter
```

## 5. Format Partitions

```sh
mkfs.fat -F32 /dev/nvme0n1p1
mkswap /dev/nvme0n1p2
swapon /dev/nvme0n1p2
mkfs.ext4 /dev/nvme0n1p3
mkfs.ext4 /dev/nvme0n1p4
```

## 6. Mount Partitions

```sh
mount /dev/nvme0n1p3 /mnt
mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p1 /mnt/boot/efi
mkdir -p /mnt/home
mount /dev/nvme0n1p4 /mnt/home
```

## 7. Install Arch Linux

```sh
pacstrap -K /mnt base linux linux-firmware base-devel neovim
```

## 8. Generate fstab

```sh
genfstab -U /mnt >> /mnt/etc/fstab
```

## 9. Change Root into New System

```sh
arch-chroot /mnt
```

## 10. Set Hostname

Edit `/etc/hostname`:

```sh
nvim /etc/hostname
# Add 'archlinux' as the hostname
```

## 11. Set Locale and Timezone

Edit `/etc/locale.gen`:

```sh
nvim /etc/locale.gen
# Uncomment "en_US.UTF-8 UTF-8" or your preferred locale
```

Generate locales:

```sh
locale-gen
```

Edit `/etc/locale.conf`:

```sh
nvim /etc/locale.conf
# Add the following line:
LANG=en_US.UTF-8
```

Set timezone:

```sh
ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
hwclock --systohc
```

## 12. Install and Enable NetworkManager

```sh
pacman -S networkmanager
systemctl enable NetworkManager
```

## 13. Set Root Password

```sh
passwd
```

## 14. Install and Configure Zsh

```sh
pacman -S zsh
```

## 15. Create New User with Sudo Privileges

```sh
useradd -m -G wheel -s /bin/zsh rahul
passwd rahul
export EDITOR=nvim
visudo
# Uncomment "%wheel ALL=(ALL:ALL) ALL"
```

## 16. Enable SSD Trim Support

```sh
systemctl enable fstrim.timer
```

## 17. Install GRUB Bootloader

```sh
pacman -S grub efibootmgr os-prober dosfstools mtools
```

## 18. Install GRUB to EFI

```sh
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
```

## 19. Install AMD Microcode

```sh
pacman -S amd-ucode
```

## 20. Generate Boot Configuration

```sh
grub-mkconfig -o /boot/grub/grub.cfg
```

## 21. Leave Installation

```sh
exit
umount -R /mnt
shutdown now
```
