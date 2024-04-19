# 在本地 Shell 设置 SSH 端口转发

## 问题背景
 由于 minikube dashboard 默认在 localhost 上提供服务，用户在外部浏览器无法访问，所有需要设置端口转发才能从你的本地机器访问它。

## 解决过程
### 步骤 1：使用 SSH 命令连接到 Debian 虚拟机

你可以使用以下命令来建立一个 SSH 连接，并设置端口转发。这里的命令将本地端口 9999 转发到虚拟机的 Minikube Dashboard 端口（37297，这个端口可能会变，根据你之前执行 minikube dashboard --url 时获取的实际端口来替换）。

```bash
ssh -L 9999:localhost:37297 your_username@192.168.5.55
```
请替换 your_username 为你的用户名，并确保你知道虚拟机的登录密码，因为连接时可能会需要输入密码。

### 步骤 2: 在浏览器中访问 Kubernetes Dashboard
一旦 SSH 连接建立，你可以在本地机器的浏览器中输入以下 URL 来访问 Kubernetes Dashboard：

```ruby
http://127.0.0.1:9999/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```
这将使你能够通过本地端口 9999 访问运行在 Debian 虚拟机上的 Kubernetes Dashboard。

使用 PowerShell 的好处是你不需要安装任何额外的软件就可以完成端口转发，只要你的 Windows 10 机器支持 SSH 即可。

## 拓展：如何代理多个端口
你可以在同一个 SSH 连接中代理多个端口。在 SSH 中设置端口转发时，你可以为每个需要访问的服务指定一个本地端口，并将其映射到远程服务器上的相应端口。

如果你正在使用 Windows PowerShell 进行 SSH 连接，下面是如何同时设置多个端口转发的示例：

### 示例命令

假设你需要同时代理两个端口，一个是 Kubernetes Dashboard 端口，另一个是某个其他服务的端口，你可以这样做：

```bash
ssh -L 9999:localhost:37297 -L 8888:localhost:8080 your_username@192.168.5.55
```

在这个例子中：
- `9999:localhost:37297` 是将本地的 9999 端口转发到虚拟机的 37297 端口（假设这是 Kubernetes Dashboard）。
- `8888:localhost:8080` 是将本地的 8888 端口转发到虚拟机的 8080 端口（假设这是另一个需要访问的服务）。
- `your_username` 是你的用户名。
- `192.168.5.55` 是你的 Debian 虚拟机的 IP 地址。

### 注意事项

1. **确保端口号不冲突**：每个转发的端口都应该映射到一个唯一的本地端口号，以避免冲突。
2. **管理 SSH 会话**：当 SSH 会话保持活动状态时，端口转发才会继续工作。如果 SSH 会话中断，端口转发也会停止。
3. **安全考虑**：确保你了解哪些端口是开放的，特别是在使用公共网络时，以防止潜在的安全风险。

使用这种方法，你可以灵活地从本地机器访问部署在远程服务器上的多个服务。如果你有其他问题或需要进一步的帮助，随时告诉我！

> 如果你需要为多个端口设置 SSH 端口转发，并希望有一种更灵活、更易于管理的方法，特别是当端口数量较多时，你可以考虑使用脚本或配置文件来简化和自动化这个过程。

### 使用 SSH 配置文件

你可以在你的 SSH 客户端配置文件（通常是 `~/.ssh/config`）中预先设置端口转发。这样，你只需要在连接时指定配置的名称，所有的端口转发规则都会自动应用。

例如，你可以在配置文件中添加如下配置：

```config
Host minikube-vm
    HostName 192.168.5.55
    User your_username
    LocalForward 9990 localhost:9900
    LocalForward 9991 localhost:9901
    LocalForward 9992 localhost:9902
    LocalForward 9993 localhost:9903
    LocalForward 9994 localhost:9904
    LocalForward 9995 localhost:9905
    LocalForward 9996 localhost:9906
    LocalForward 9997 localhost:9907
    LocalForward 9998 localhost:9908
    LocalForward 9999 localhost:9909
```

在这个配置中，`minikube-vm` 是你定义的一个别名，包含了10个端口转发的设置。然后，你可以简单地通过下面的命令来启动所有的端口转发：

```bash
ssh minikube-vm
```