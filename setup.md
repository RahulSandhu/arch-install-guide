# Arch Linux Setup Guide

## 1. Connect to Wi-Fi and Update

```bash
nmcli device wifi connect "MyWiFi" password "mypassword"
sudo pacman -Syu
```

## 2. Improve Download Speeds

```bash
sudo pacman -S reflector
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
sudo reflector --verbose --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

## 3. Install yay (AUR Helper)

```bash
sudo pacman -S git
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

## 4. Install Window Manager and Dependencies

Install `swayfx` and related dependencies:

```bash
yay -S swayfx
sudo pacman -S swaybg swaync xdg-utils xorg-xwayland polkit kitty wofi waybar
```

Inside `.zprofile`, add the following line to start `swayfx` automatically when
logging in from `tty1`:

```bash
# If running from tty start swayfx
[ "$(tty)" = "/dev/tty1" ] && exec sway
```

After installing SwayFX, copy the default configuration file to your home
directory to modify terminal and application launcher.

```bash
mkdir -p ~/.config/sway
cp /etc/sway/config ~/.config/sway/
nvim ~/.config/sway/config
```

## 5. Enable Audio

```bash
sudo pacman -S pipewire wireplumber pipewire-pulse
systemctl --user --now enable pipewire wireplumber
```

## 6. Enable Bluetooth

```bash
sudo pacman -S bluez bluez-utils blueman bluez-libs
sudo modprobe btusb
sudo systemctl enable bluetooth
```

## 7. Install Essential Packages

### General Utilities

```bash
sudo pacman -S curl exfat-utils fuse-exfat neofetch ripgrep stow tree wget fd
fzf tmux wl-clipboard yazi dbeaver docker github-cli openconnect r audacity
firefox gimp kdenlive kodi libreoffice-fresh spotify-launcher vlc p7zip tar
timeshift ufw unrar unzip zip nodejs npm trash-cli zoxide
yay -S ttf-material-icons ttf-jetbrains-mono-nerd noto-fonts-emoji
```

### LaTeX and PDF Readers

```bash
sudo pacman -S biber okular zathura texlive-basic texlive-bibtexextra 
texlive-fontsextra texlive-fontsrecommended texlive-lang texlive-latex 
texlive-latexextra texlive-latexrecommended texlive-luatex texlive-mathscience 
texlive-xetex
```

## 8. Create Common Folders

```bash
mkdir -p ~/documents ~/downloads ~/pictures ~/projects ~/public ~/templates 
~/videos ~/projects ~/.virtualenvs ~/.autosessions/
