# dotfiles

Personal Linux dotfiles for:

- zsh
- tmux
- Neovim
- i3
- Polybar

This repo is meant to be checked out directly into your home directory using a bare Git repo, so files like `.zshrc`, `.tmux.conf`, and `.config/...` land in the correct places.

## Contents

- `~/.zshrc`
- `~/.tmux.conf`
- `~/.config/i3`
- `~/.config/polybar`
- `~/.config/nvim`

---

## Dependencies

### Core packages

Install these first:

```bash
sudo apt update
sudo apt install -y \
  git zsh tmux i3 polybar dex xss-lock i3lock \
  network-manager-gnome xinput xrandr feh redshift \
  pulseaudio-utils brightnessctl alacritty rofi \
  flameshot xclip pavucontrol pamixer \
  zsh-syntax-highlighting zsh-autosuggestions \
  alacritty rofi
```

Remove GNOME:

```bash
sudo apt remove -y \
sudo apt purge gnome-shell gdm3 ubuntu-desktop \
sudo apt autoremove --purge
```

i3 in x:

```bash
echo 'exec i3' > ~/.xinitrc
chmod +x ~/.xinitrc
```

### Fonts

Polybar setups commonly need Nerd Fonts and icon fonts. Recommended:

- FiraCode Nerd Font
- Font Awesome
- Material Design Icons

### Neovim

This config is intended for a recent Neovim release rather than the old distro package.

Install latest Neovim manually:

```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
tar xzf nvim-linux-x86_64.tar.gz
sudo mv nvim-linux-x86_64 /opt/nvim
sudo ln -sf /opt/nvim/bin/nvim /usr/local/bin/nvim
nvim --version
```

### Useful extra tooling for Neovim

Depending on what parts of the config you use, you may also want:

```bash
sudo apt install -y make clangd lua-language-server
```

Install pyright with npm:

```bash
sudo apt install -y npm
sudo npm install -g pyright
```

Install stylua:

```bash
cargo install stylua
```

Or install it however you prefer.

---

## Installation

### 1. Back up existing dotfiles

```bash
mkdir -p ~/dotfiles-backup

mv ~/.zshrc ~/dotfiles-backup/ 2>/dev/null
mv ~/.tmux.conf ~/dotfiles-backup/ 2>/dev/null
mv ~/.config/i3 ~/dotfiles-backup/ 2>/dev/null
mv ~/.config/polybar ~/dotfiles-backup/ 2>/dev/null
mv ~/.config/nvim ~/dotfiles-backup/ 2>/dev/null
```

### 2. Clone as a bare repo

```bash
git clone --bare https://github.com/1jepps0/dotfiles.git "$HOME/.dotfiles"
```

### 3. Check out the files into your home directory

```bash
git --git-dir="$HOME/.dotfiles" --work-tree="$HOME" checkout
```

If Git says some files would be overwritten, move or back them up first, then run checkout again.

### 4. Hide untracked home-directory files from Git status

```bash
git --git-dir="$HOME/.dotfiles" --work-tree="$HOME" config --local status.showUntrackedFiles no
```

### 5. Add a convenience alias

Add this to your shell config:

```bash
alias dotfiles='git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
```

Reload your shell:

```bash
source ~/.zshrc
```

---

## Usage

### Pull latest changes

```bash
dotfiles pull
```

### View tracked changes

```bash
dotfiles status
```

### Stage changes to tracked files only

```bash
dotfiles add -u
```

### Commit changes

```bash
dotfiles commit -m "Update dotfiles"
```

### Push changes

```bash
dotfiles push
```

---

## First boot / first login notes

After install, you may need to:

- set `zsh` as your default shell
- log into an i3 session
- install fonts for Polybar icons
- launch `nvim` once so plugins can install
- verify monitor, audio, and network applet behavior

Change default shell if needed:

```bash
chsh -s "$(which zsh)"
```

---

## Neovim plugin setup

Open Neovim after install:

```bash
nvim
```

Let the plugin manager install everything on first launch.

If LSP servers or formatters are missing, install them manually or through your Neovim tooling.

---

## Troubleshooting

### Git checkout says files would be overwritten
Move your existing config files out of the way and rerun checkout.

### `dotfiles` command not found
Make sure the alias is added to `~/.zshrc`, then reload your shell.

### Polybar icons look broken
Install the required Nerd Font / icon fonts and update the font names in the Polybar config if needed.

### `nvim` fails to start
Run:

```bash
nvim --version
```

Make sure `/usr/local/bin/nvim` points to the manually installed release.

### Some keybinds or scripts fail
If the config references local scripts not included in this repo, you will need to create or remove those references.

---

## Uninstall

To remove the bare repo while keeping your checked-out files:

```bash
rm -rf ~/.dotfiles
```

To fully remove configs too, delete the files from your home directory manually.

---

## License

Personal dotfiles repo. Reuse whatever is helpful.
