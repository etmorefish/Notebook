# Linux
> Linux 备忘

## 代理

- unix: `export http_proxy=http://localhost:7890 && export https_proxy=http://localhost:7890`
- windows: `$Env:http_proxy="http://localhost:7890";$Env:https_proxy="http://localhost:7890"`


## 生产力工具快捷键
### Tmux
> Tmux 是一个终端复用器（terminal multiplexer），非常有用，属于常用的开发工具。

Tmux 窗口有大量的快捷键。所有快捷键都要通过前缀键唤起。默认的前缀键是Ctrl+b，即先按下Ctrl+b，快捷键才会生效。

- 快捷键
  - `tmux new -s <session-name>` : 新建一个会话
    第一个启动的 Tmux 窗口，编号是0，第二个窗口的编号是1，以此类推。这些窗口对应的会话，就是 0 号会话、1 号会话。
使用编号区分会话，不太直观，更好的方法是为会话起名。
  - `tmux ls`: 列出所有会话
  - `tmux attach -t <session-name>`: 进入一个会话
  - `tmux detach`: 退出当前会话
  - `tmux kill-session -t <session-name>`: 关闭一个会话
  - `tmux kill-server`: 关闭所有会话
  - `tmux switch -t <session-name>`: 切换到指定会话
  - `tmux rename-session -t <session-name> <new-session-name>`: 重命名会话
  - `tmux list-keys`: 列出所有快捷键
  - `tmux list-commands`: 列出所有命令
  - `tmux source-file <path-to-tmux-config>`: 加载Tmux配置文件
  - `Ctrl+b d`: 分离当前会话
  - `Ctrl+b s`: 列出所有会话，可以选择会话编号，按下回车键进入该会话
  - `Ctrl+b $`: 重命名当前会话
  - `Ctrl+b %`: 垂直分割当前窗口
  - `Ctrl+b "`: 水平分割当前窗口
  - `Ctrl+b q`: 显示当前窗口的编号
  - `Ctrl+b x`: 关闭当前窗口



### [LazyVim](/Linux/Nvim.md)

### nvm - 快速切换Nodejs版本



命令

```bash
# 查询Linux下个用户的内存占用
ps aux | awk 'NR>1 {user[$1]+=$6} END {for (i in user) print i, user[i]/1024/1024 " GB"}'

```

### Jupyter 插件
```shell
pip install jupyterlab-lsp
pip install jupyterlab-language-pack-zh-CN
pip install jupyterlab-execute-time
pip install jupyter-resource-usage
pip install jedi-language-server
```