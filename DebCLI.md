# Настройка текстового интерфейса в Debian / Ubuntu / Mint

## Общее
```sh
sudo apt install -y neovim git tmux fzf fasd vifm powerline ripgrep bat stow chafa thefuck console-terminus xfonts-terminus \
fortune cowsay lolcat sl cmatrix &&

# Font
gsettings set org.gnome.desktop.interface monospace-font-name 'Terminus Medium 8'
gconftool-2 --set /apps/gnome-terminal/profiles/Default/use_system_font --type=boolean false

# colors
ln -s `which batcat` ~/.local/bin/bat
git clone https://github.com/chriskempson/base16-shell.git ~/.config/base16-shell &&
git clone https://github.com/nosovicki/dotfiles.git ~/.dotfiles &&
cd ~/.dotfiles &&
stow -v color

```
## Neovim
```sh
# install init.vim
stow nvim
# Download vim-plug
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
# Install plugins
nvim --headless +PlugInstall +qa
nvim +UpdateRemotePlugins +qa

```

## Tmux
```sh
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm &&
cd ~/.dotfiles &&
stow tmux
~/.tmux/plugins/tpm/bindings/install_plugins
```

## Zsh
```sh
sudo apt install zsh powerline
chsh -s $(which zsh)
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git $HOME/.oh-my-zsh/custom/themes/powerlevel10k
mkdir -p ~/.fonts
cd ~/.fonts/
for x in Regular Bold Italic Bold%20Italic; do wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20$x.ttf\;done
cd -
fc-cache -f -v
gconftool-2 --set /apps/gnome-terminal/profiles/Default/font --type string "MesloLGS NF 10"
gconftool-2 --set /apps/gnome-terminal/profiles/Default/use_system_font --type=boolean false
sed -i 's/POWERLEVEL9K_DIR_BACKGROUND=.*$/POWERLEVEL9K_DIR_BACKGROUND=31/' ~/.p10k.zsh
cd ~/.oh-my-zsh/custom/plugins
git clone https://github.com/zsh-users/zsh-autosuggestions.git
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
git clone https://github.com/unixorn/fzf-zsh-plugin.git
cd -
sed -i '/^plugins/s/)/ zsh-autosuggestions zsh-syntax-highlighting fzf-zsh-plugin)/' ~/.zshrc
(cat >>~/.zshrc<<END
if [ -f \$HOME/.rc ]; then
  source \$HOME/.rc;
fi
END
)

```
## Sources
Adapted from:
- https://betterprogramming.pub/how-to-use-tmuxp-to-manage-your-tmux-session-614b6d42d6b6
- https://erikzaadi.com/2020/02/17/how-to-use-zsh-and-tmuxp-to-speed-up-your-day-to-day-workflow/
- https://github.com/hamvocke/dotfiles
- https://www.freecodecamp.org/news/tmux-in-practice-integration-with-system-clipboard-bcd72c62ff7b/
- https://man7.org/linux/man-pages/man1/tmux.1.html
- https://github.com/sharkdp/bat/issues/523
