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

Install i3 and related dependencies:

```bash
sudo pacman -S xorg-server xorg-xinit i3-wm i3-gaps xdg-utils feh kitty rofi
polybar picom polkit
```

Inside `~/.xinitrc`, add the following line to start i3 automatically when
logging in from tty1:

```bash
# If running from tty start i3
[ "$(tty)" = "/dev/tty1" ] && exec i3
```

After installing i3, copy the default configuration file to your home directory
to modify terminal and application launcher:

```bash
mkdir -p ~/.config/i3
cp /etc/i3/config ~/.config/i3/
nvim ~/.config/i3/config
```

## 5. Enable Audio

```bash
sudo pacman -S pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber
alsa-utils
systemctl --user --now enable pipewire wireplumber
```

## 6. Enable Bluetooth

```bash
sudo pacman -S bluez bluez-utils bluez-libs blueman
sudo modprobe btusb
sudo systemctl enable bluetooth
```

## 7. Install Essential Packages

### General Utilities

```bash
sudo pacman -S audacity curl dbeaver docker exfat-utils fd fzf firefox
fuse-exfat gimp github-cli imagemagick kdenlive kodi libreoffice-fresh lua
luajit luarocks neofetch nodejs npm noto-fonts-emoji openconnect p7zip r
ripgrep rlwrap sed jq ssh avahi spotify-launcher stow tar timeshift tmux
trash-cli tree ufw unrar unzip vlc wget xclip yazi zip zoxide playerctl
brightnessctl
luarocks install magick --local
yay -S noto-fonts-emoji ttf-material-icons ttf-jetbrains-mono-nerd 
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
