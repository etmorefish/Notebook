# Nvim - Lazynvim

## Install 

- install neovim

```shell

wget https://ghproxy.com/https://github.com/neovim/neovim/releases/download/stable/nvim-linux64.tar.gz

tar xzf nvim-linux64.tar.gz
cd nvim-linux64/
sudo mv nvim-linux64 /usr/local/
sudo ln -s /usr/local/nvim-linux64/bin/nvim /usr/local/bin/nvim
nvim --version

```

- install LazyVim

```shell
#Make a backup of your current Neovim files:

# required
mv ~/.config/nvim{,.bak}

# optional but recommended
mv ~/.local/share/nvim{,.bak}
mv ~/.local/state/nvim{,.bak}
mv ~/.cache/nvim{,.bak}

# Clone the starter

git clone https://github.com/LazyVim/starter ~/.config/nvim

# Remove the .git folder, so you can add it to your own repo later

rm -rf ~/.config/nvim/.git

# Start Neovim!

nvim
```

