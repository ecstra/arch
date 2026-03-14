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
  unzip
```

### Install yay (AUR helper)

```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

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
  vulkan-mesa-layers \
  libva-mesa-driver \
  nwg-look
```

### File Manager and Browser

```bash
sudo pacman -S nautilus
yay -S google-chrome
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
sudo pacman -S code
```

Python, Pip & UV:

```zsh
sudo pacman -Syu python
sudo pacman -S python-pip
curl -LsSf https://astral.sh/uv/install.sh | sh
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
