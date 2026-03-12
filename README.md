# Arch Setup

## Install

### Install yay (AUR helper)

```bash
sudo pacman -Syu
sudo pacman -S --needed base-devel git

git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

---

### Hyprland Packages

```bash
sudo pacman -S \
  hyprland \
  hyprpaper \
  hyprlock \
  hypridle \
  waybar \
  wl-clipboard
```

---

### System / Graphics

```bash
sudo pacman -S \
  mesa \
  vulkan-icd-loader \
  vulkan-virtio \
  egl-wayland \
  xdg-desktop-portal-hyprland \
  polkit-kde-agent \
  vulkan-tools \
  vulkan-radeon \
  vulkan-utils
```

---

### Development Tools

```bash
sudo pacman -S \
  base-devel \
  git \
  neovim \
  wget \
  curl \
  unzip
```

---

### File Manager and Browser

```bash
sudo pacman -S nautilus
yay -S google-chrome
```

---

### Fonts

```bash
cd ~/Downloads

# Create temp dir
tmp=$(mktemp -d)
cd "$tmp"

# Download fonts
wget -q https://github.com/belluzj/fantasque-sans/releases/download/v1.8.0/FantasqueSansMono-Normal.zip
wget -q https://github.com/ryanoasis/nerd-fonts/releases/download/v3.4.0/JetBrainsMono.zip

# Extract
unzip -q FantasqueSansMono-Normal.zip
unzip -q JetBrainsMono.zip

# Install fonts
mkdir -p ~/.local/share/fonts

mv JetBrainsMono*.ttf ~/.local/share/fonts/
mv TTF/FantasqueSansMono*.ttf ~/.local/share/fonts/

# Refresh font cache
fc-cache -fv

# Cleanup
cd
rm -rf "$tmp"
```

---

## Configs

Wallpapers are stored in:

```
~/.wallpapers
```

Configuration files are stored in:

```
~/.config
```

Contents:

1. `chrome-flags.conf`
   Used to manage Chrome flags.

2. `hypr/`
   Contains configuration for:

   * Hyprland
   * Hyprlock
   * Hyprpaper

3. `kitty/`
   Contains Kitty terminal configuration.
