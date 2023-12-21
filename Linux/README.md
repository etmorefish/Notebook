# Linux
> Linux 备忘

## 代理

- unix: `export http_proxy=http://localhost:7890 && export https_proxy=http://localhost:7890`
- windows: `$Env:http_proxy="http://localhost:7890";$Env:https_proxy="http://localhost:7890"`


## 生产力工具快捷键
### Tmux


### [LazyVim](/Linux/Nvim.md)

nvm - 快速切换Nodejs版本



命令

```
 ps aux | awk 'NR>1 {user[$1]+=$6} END {for (i in user) print i, user[i]/1024/1024 " GB"}'
 
```

