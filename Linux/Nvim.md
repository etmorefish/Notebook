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

## 快捷键

```
ctrl + /   快速调出终端
r 文件重命名
y 复制
p 粘贴
i 查看文件信息
o 修改文件排序规则
a 新建一个文件或文件夹
s 拆分屏幕
d 删除文件

f 搜索文件
g + [goto] 组合键
hjkl 方向键，左下上右

x 剪切
c 复制文件到某一个地方
v +【】可视模式
b 光标往前走一个单词的位置
m 移动文件到某i一个位置
```

