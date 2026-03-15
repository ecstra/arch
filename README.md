# Arch Setup

## Screenshots

<img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/e98b7263-2485-4048-b258-622aeacac04b" />
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/686e5868-466c-48a2-8378-54df28548c07" />

## Install

### Development Tools

```bash
sudo pacman -S \
  base-devel \
  git \
  neovim \
  wget \
  curl \
  unzip \
  jq
```

### Install yay (AUR helper)

```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### Hyprland Stuff

```bash
sudo pacman -S \
  hyprland \
  hyprpaper \
  hyprlock \
  hypridle \
  waybar \
  wl-clipboard

yay -S wlogout
yay -S mission-center
```

### Theme

Theme Setter:

```
sudo pacman -S \
  nwg-look \
  xdg-desktop-portal-gtk \
  xdg-desktop-portal-hyprland
```

Cursor & Icons:

```
yay -S bibata-cursor-theme
sudo pacman -S papirus-icon-theme
```

Notification:

```
# Disable Default Notification
systemctl --user disable --now dunst

# Install Swaync
sudo pacman -S swaync
```

On Screen Display:

```
# Fix Permissions
sudo usermod -aG video $USER
sudo udevadm control --reload-rules && sudo udevadm trigger

# Install SwayOSD
sudo pacman -S swayosd
```

### System / Graphics

```bash
sudo pacman -S \
  mesa \
  vulkan-icd-loader \
  vulkan-virtio \
  egl-wayland \
  polkit-kde-agent \
  vulkan-tools \
  vulkan-radeon \
  vulkan-mesa-layers \
  libva-mesa-driver
```

### File Manager and Browser

File Manager:

```bash
sudo pacman -S nautilus
```

Browser:
```
# install pacman utilities
sudo pacman -S pacman-contrib

# install debtap from AUR (tool to convert .deb packages to Arch packages)
yay -S debtap

# initialize debtap database (downloads Debian -> Arch dependency mapping)
sudo debtap -u

# download specific Chrome version (137) which still supports Manifest V2 extensions (for uBlock)
wget https://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_137.0.7151.119-1_amd64.deb

# convert the Debian package into an Arch package (.pkg.tar.zst)
debtap google-chrome-stable_137.0.7151.119-1_amd64.deb

# install the converted package
sudo pacman -U -dd google-chrome-stable-137.0.7151.119-1-x86_64.pkg.tar.zst

# delete auto-update cron-job
sudo rm -rf /opt/google/chrome/cron
```

Edit `/etc/pacman.conf`:

```
IgnorePkg = google-chrome-stable google-chrome
```

### Fonts

```bash
sudo pacman -S \
  ttf-dejavu \
  ttf-liberation \
  noto-fonts \
  noto-fonts-emoji \
  ttf-jetbrains-mono-nerd \
  ttf-jetbrains-mono \
  ttf-fantasque-nerd

fc-cache -fv
```

### Workspace Essentials

Screenshot Tools:

```bash
sudo pacman -S \
  grim \
  slurp
```

Controls:

```bash
sudo pacman -S \
  pavucontrol \
  nm-connection-editor \
  blueman
```

Launcher:

```bash
yay -S vicinae
```

## Device Essentials

### Nvidia

Drivers:

```
# 1. Install nvidia drivers
sudo pacman -S \
  nvidia-dkms \
  nvidia-utils \
  nvidia-prime \
  linux-headers

# 2. Rebuild initramfs
sudo mkinitcpio -P

# 3. Blacklist nouveau
echo "blacklist nouveau" | sudo tee /etc/modprobe.d/blacklist-nouveau.conf
```

nvidia-laptop-power-cfg (Ampere power management):

```
git clone https://gitlab.com/asus-linux/nvidia-laptop-power-cfg.git
cd nvidia-laptop-power-cfg
makepkg -sfi
cd ..
rm -rf nvidia-laptop-power-cfg
sudo systemctl enable nvidia-suspend.service nvidia-hibernate.service nvidia-resume.service
sudo systemctl enable --now nvidia-powerd
```

Create `/etc/modprobe.d/nvidia.conf` and put:

```conf
options nvidia_drm modeset=1
options nvidia NVreg_EnableGpuFirmware=0 NVreg_EnableS0ixPowerManagement=1 NVreg_DynamicPowerManagement=0x02
```

### Asus

Firmware & UCode:

```bash
sudo pacman -S \
  linux-firmware \
  amd-ucode
```

Keys:

```bash
sudo pacman-key --recv-keys 8F654886F17D497FEFE3DB448B15A6B0E9A3FA35
sudo pacman-key --finger 8F654886F17D497FEFE3DB448B15A6B0E9A3FA35
sudo pacman-key --lsign-key 8F654886F17D497FEFE3DB448B15A6B0E9A3FA35
sudo pacman-key --finger 8F654886F17D497FEFE3DB448B15A6B0E9A3FA35
```

Edit `/etc/pacman.conf` at end of file:

```conf
[g14]
Server = https://arch.asus-linux.org
```

Update:

```bash
sudo pacman -Suy
```

Asus Control Center:

```bash
sudo pacman -S \
  asusctl \
  power-profiles-daemon \
  supergfxctl \
  rog-control-center

sudo systemctl enable --now asusd
sudo systemctl enable --now supergfxd
systemctl enable --now power-profiles-daemon.service
```

## Shell

Terminal: `kitty`  
Shell: `zsh`

### Install Zsh + Oh My Zsh

```bash
sudo pacman -S zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Set Zsh as default shell:

```bash
chsh -s $(which zsh)
```

Restart the terminal.

### Zsh Plugins

Install plugins used in the setup.

```bash
# Autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# Syntax Highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# Completions
git clone https://github.com/zsh-users/zsh-completions.git ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions

# Additional plugins (ubuntu, sudo, command-not-found, python)
git clone https://github.com/ohmyzsh/ohmyzsh.git /tmp/ohmyzsh_repo
cp -r /tmp/ohmyzsh_repo/plugins/{sudo,command-not-found,python} ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/
rm -rf /tmp/ohmyzsh_repo
```

Update plugins list in `~/.zshrc`:

```zsh
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
  zsh-completions
  command-not-found
  python
  sudo
)
```

### Powerlevel10k Theme

Install theme:

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
"${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"
```

Set theme in `~/.zshrc`:

```zsh
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Restart the terminal and run the guided configuration.

### Completion Setup

Add **before** `source $ZSH/oh-my-zsh.sh` in `~/.zshrc`:

```zsh
fpath+=${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions/src
autoload -U compinit && compinit
```

### Custom Configuration

Append to the **end of `~/.zshrc`**:

```zsh
# Keybindings
bindkey '^[[A' history-search-backward
bindkey '^[[B' history-search-forward
bindkey '^[w' kill-region

# Aliases
alias ls='ls --color'
alias cls='clear'

# History
HISTSIZE=5000
HISTFILE=~/.zsh_history
SAVEHIST=$HISTSIZE
HISTDUP=erase
setopt appendhistory
setopt sharehistory
setopt hist_ignore_space
setopt hist_ignore_all_dups
setopt hist_save_no_dups
setopt hist_ignore_dups
setopt hist_find_no_dups

# Completion styling
zstyle ':completion:*' matcher-list 'm:{a-z}={A-Za-z}'
zstyle ':completion:*' list-colors "${(s.:.)LS_COLORS}"
zstyle ':completion:*' menu no
```
### VS Code

Set **zsh** as the default integrated shell in VS Code.

### Nano Syntax Hilighting

Edit nanorc file:

```zsh
sudo nano /etc/nanorc
```

Add:

```
include "/usr/share/nano/*.nanorc"
set tabsize 4
set tabstospaces
```

## Development Tools

Code Editor:

```zsh
yay -S visual-studio-code-bin
```

Python, Pip & UV:

```zsh
sudo pacman -Syu python
sudo pacman -S python-pip
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
```

Node & Bun:

```zsh
curl -o- https://fnm.vercel.app/install | bash
curl -fsSL https://bun.sh/install | bash
source ~/.zshrc
fnm install 24
```

Rust:

```zsh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
echo '. "$HOME/.cargo/env"' >> ~/.zshrc
source ~/.zshrc
```
