# Dotfiles

These dotfiles should work on [Arch](https://archlinux.org/download).

# Setup

1. Install the base applications, tools and utilities.

```bash
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay-bin.git && cd yay-bin && makepkg -si

curl -f https://zed.dev/install.sh | sh

pacman -S git zip unzip rofi wayland waybar hyprland wl-clipboard zsh bluez bluez-utils blueman pulseaudio hyprpaper sddm ghostty firefox noto-fonts-cjk noto-fonts-emoji noto-fonts NetworkManager telegram-desktop discord fastfetch wlr-randr docker flameshot grim nautilus qt6-svg qt6-declarative qt5-quickcontrols2 code

mkdir -p ~/Pictures/Screenshots
yay -S spotify
curl -fsSL https://bun.sh/install | bash
usermod -aG docker $USER

# Set zsh as default editor (make sure its allowed in /etc/shells).
chsh -s $(which zsh)
```

2. Enable some services.

```bash
systemctl enable sddm
systemctl enable docker
systemctl enable --now NetworkManager
systemctl enable --now bluetooth
```

3. Enable BBR.

```bash
echo "net.core.default_qdisc=fq_codel" >> /etc/sysctl.d/bbr.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.d/bbr.conf
modprobe tcp_bbr
sysctl -p /etc/sysctl.d/bbr.conf
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control

# Check if it's loaded.
lsmod | grep bbr
```

4. Install oh-my-zsh.

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

5. Pull and apply these dotfiles with chezmoi.

```bash
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply kvizyx

# Copy SDDM config and themes
cp ~/.local/share/chezmoi/sddm/sddm.conf /etc/sddm.conf
cp -r ~/.local/share/chezmoi/sddm/themes /usr/share/sddm/themes
```

6. Reboot.
7. Download [JetBrains Mono](https://www.jetbrains.com/lp/mono) fonts.

```bash
# Unpack fonts after download.
tar -xvzf ~/Downloads/JetBrainsMono-* -C /usr/share/fonts
fc-cache -f -v
```
