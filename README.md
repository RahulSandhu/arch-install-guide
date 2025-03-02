# Arch Linux Installation & Setup Guide

This repository provides a comprehensive guide for installing and setting up
Arch Linux on your system. It is divided into two main parts:

- **Installation Guide (`install.md`)**  
Detailed step-by-step instructions for installing Arch Linux, covering
everything from drive preparation and partitioning to installing the base
system and configuring the bootloader.

- **Setup Guide (`setup.md`)**  
Post-installation instructions including connecting to Wi-Fi, updating
packages, installing essential utilities and window managers (such as Sway), as
well as configuring audio, Bluetooth, and other system components.

## Overview

This guide covers the following major sections:

1. **Preparation & Installation (install.md):**
   - **Console Setup:** Set keyboard layout and console font.
   - **Disk Preparation:** Wipe drives, partition (EFI, swap, root, and home),
   and format partitions.
   - **Base Installation:** Mount partitions, install the base system with
   `pacstrap`, and generate `fstab`.
   - **System Configuration:** Chroot into the new system, set hostname,
   locale, timezone, and enable essential services like NetworkManager.
   - **Bootloader Setup:** Install and configure GRUB along with GPU drivers
   and microcode updates.

2. **Post-Installation Setup (setup.md):**
   - **Network & Updates:** Connect to Wi-Fi using NetworkManager or `nmcli`,
   update system packages, and improve download speeds using Reflector.
   - **AUR Helper & Software Installation:** Install the AUR helper `yay` and
   additional software including a window manager (`sway`), essential
   utilities, audio, and Bluetooth support.
   - **Desktop Environment Customization:** Configure desktop environment
   settings and create common folders for user data.

## How to Use This Guide

1. **During Installation:**  
Follow the instructions in [`install.md`](./install.md) as you prepare your
hardware, partition your drives, and install Arch Linux.

2. **After Installation:**  
Once the base system is installed, refer to [`setup.md`](./setup.md) for
post-installation configurations to enhance your system setup.

## Prerequisites

- A bootable USB drive with the Arch Linux ISO.
- A stable internet connection.
- Familiarity with the Linux command line.
- A backup of important data (installation involves disk partitioning which can
lead to data loss if not handled carefully).

## Disclaimer

**Warning:** Installing Arch Linux requires careful planning and execution.
This guide is provided for educational purposes only. The author is not
responsible for any data loss or system issues that may occur. Always refer to
the [Arch Wiki](https://wiki.archlinux.org/) for the most current and detailed
instructions.

## License

This project is licensed under the [MIT License](LICENSE)

---
