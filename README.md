# Dotfiles for Personalization of nvim and wezterm (macOS) or kitty(raspy)

I created these files with the help of the YouTuber **[Josean's GitHub](https://github.com/josean-dev)**,
because I liked his setup the most.

His setup is mostly adjusted for wbdevelopment, so some changings have to be done adjusted to personal coding needs.

The videos I used:

**[Wezterm setup](https://youtu.be/TTgQV21X0SQ)**

**[Nvim Setup](https://youtu.be/6pAG3BHurdM)**

Additionally several dependencies have to be installed on your system, like e.g. python, node.js etc.

GLHF :)

---

## Kitty + zsh setup (Linux / Ubuntu / Raspberry Pi OS)

### 1. Install packages

```bash
sudo apt update
sudo apt install zsh kitty git curl unzip fontconfig zoxide
```

### 2. Make zsh the default shell

```bash
chsh -s "$(which zsh)"
```

Check it (the change only applies after logging out and back in):

```bash
grep "^$USER:" /etc/passwd   # should end with /usr/bin/zsh
```

### 3. Install Oh My Zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

The installer creates a `~/.zshrc` and asks to switch the default shell, answer `y` if step 2 was skipped.

### 4. Theme (Powerlevel10k) and plugins

```bash
ZSH_CUSTOM="${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}"

git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "$ZSH_CUSTOM/themes/powerlevel10k"
git clone https://github.com/zsh-users/zsh-autosuggestions "$ZSH_CUSTOM/plugins/zsh-autosuggestions"
git clone https://github.com/zsh-users/zsh-syntax-highlighting "$ZSH_CUSTOM/plugins/zsh-syntax-highlighting"
```

Then set these lines in `~/.zshrc`:

```zsh
ZSH_THEME="powerlevel10k/powerlevel10k"
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

And add at the end of `~/.zshrc`:

```zsh
export PATH="$HOME/.local/bin:$PATH"
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
command -v zoxide >/dev/null && eval "$(zoxide init zsh)"
```

### 5. Install the font (MesloLGS Nerd Font)

```bash
mkdir -p ~/.local/share/fonts/MesloNerd
cd ~/.local/share/fonts/MesloNerd
curl -fLO https://github.com/ryanoasis/nerd-fonts/releases/latest/download/Meslo.zip
unzip -o Meslo.zip && rm Meslo.zip
fc-cache -fv
```

Check that kitty finds it:

```bash
kitty +list-fonts | grep -i "MesloLGS Nerd Font Mono"
```

### 6. Link the kitty config from this repo

```bash
mkdir -p ~/.config/kitty
ln -sf "$PWD/kitty.conf" ~/.config/kitty/kitty.conf   # run inside the dotfiles folder
```

`kitty.conf` already starts zsh directly (`shell /usr/bin/zsh`).

### 7. Source and restart

Load the new config in the running shell:

```bash
source ~/.zshrc
```

The first start of Powerlevel10k opens the configuration wizard automatically, to run it again later: `p10k configure`.

Reload kitty's config without restarting: `Ctrl+Shift+F5`.

Finally **log out and back in** (or reboot) so the default shell change from step 2 takes effect everywhere:

```bash
sudo reboot
```

After that `echo $SHELL` should print `/usr/bin/zsh`.
