# crictl

crictl 是 CRI 兼容的容器运行时命令行接口，可以使用它来检查和调试 k8s 节点上的容器运行时和应用程序。一般来说，安装完 k8s 之后才会有该命令。

```bash
mis@k8s-node1:~$ crictl -v
crictl version v1.24.0
```

## 免root执行`crictl`
### 使用  `crictl`

```bash
mis@k8s-node1:~$ crictl images
FATA[0000] unable to determine image API version: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing dial unix /run/containerd/containerd.sock: connect: permission denied"
```
出现这个错误是因为你尝试以非特权用户运行需要特权的 `crictl` 命令。这些命令需要访问 `containerd` 的 socket 文件，通常只有 root 用户或者相应的特权组才有权限访问。

### 解决方法：

1. **使用 `sudo` 命令执行**：
   尝试在命令前加上 `sudo` 来获取必要的权限。例如：
   ```bash
   sudo crictl ps
   sudo crictl images
   ```

2. **添加用户到特权组**：
   如果你经常需要执行这类命令，你可能想要将你的用户账号添加到可以访问容器运行时的用户组。对于 `containerd`，通常这个用户组是 `docker`，尽管在某些设置中可能不同。你可以使用以下命令将你的用户添加到 `docker` 组（请根据你的环境可能需要调整组名）：
   ```bash
   sudo usermod -aG docker $USER
   ```
   修改后，你需要重新登录或重新启动，以使组成员资格生效。

3. **检查 socket 权限**：
   确保 socket 文件 `/run/containerd/containerd.sock` 的权限设置正确。通常，它应该允许 root 用户和 `docker` 组成员访问。你可以使用以下命令查看它的权限设置：
   ```bash
   ls -l /run/containerd/containerd.sock
   ```
   正确的权限设置可能类似于：
   ```text
   srw-rw---- 1 root docker 0 date time /run/containerd/containerd.sock
   ```
   如果权限不正确，你可以使用 `chown` 和 `chmod` 命令调整权限，例如：
   ```bash
   sudo chown root:docker /run/containerd/containerd.sock
   sudo chmod 660 /run/containerd/containerd.sock
   ```

使用上述方法之一，你应该能够解决权限问题，并成功运行 `crictl` 命令。

## 配置私有镜像仓库
```bash
mis@k8s-node1:~$ sudo crictl pull 192.168.5.40:8867/repository/docker-hosted/flask-demo
E0424 13:31:42.326140   47116 remote_image.go:238] "PullImage from image service failed" err="rpc error: code = Unknown desc = failed to pull and unpack image \"192.168.5.40:8867/repository/docker-hosted/flask-demo:latest\": failed to resolve reference \"192.168.5.40:8867/repository/docker-hosted/flask-demo:latest\": failed to do request: Head \"https://192.168.5.40:8867/v2/repository/docker-hosted/flask-demo/manifests/latest\": http: server gave HTTP response to HTTPS client" image="192.168.5.40:8867/repository/docker-hosted/flask-demo"
FATA[0000] pulling image: rpc error: code = Unknown desc = failed to pull and unpack image "192.168.5.40:8867/repository/docker-hosted/flask-demo:latest": failed to resolve reference "192.168.5.40:8867/repository/docker-hosted/flask-demo:latest": failed to do request: Head "https://192.168.5.40:8867/v2/repository/docker-hosted/flask-demo/manifests/latest": http: server gave HTTP response to HTTPS client
```
这个错误信息表明你在尝试使用 crictl 从私有仓库拉取 Docker 镜像时，客户端（即你的 Kubernetes 节点上的 crictl 工具）尝试通过 HTTPS 协议进行通信，但是服务器以 HTTP 协议响应。这通常发生在服务器未正确配置 SSL/TLS 或客户端错误地尝试使用 HTTPS 连接到不支持 HTTPS 的服务器。

#### 待解决的问题
- 跳过https验证
- 配置私有仓库的地址

如果是docker作为运行时，则需要配置docker的配置文件，添加私有仓库的地址。

```bash
mis@k8s-node1:~$ sudo vim /etc/docker/daemon.json
{
        "insecure-registries" : ["192.168.5.40:8866", "192.168.5.40:8867", "192.168.5.40:8868"]
}
```

然后重启docker服务：

```bash
 mis@k8s-node1:~$ sudo systemctl restart docker
```

对于`crictl`而言，他使用的是`containerd`:

编辑 /etc/containerd/config.toml 配置文件，你需要为每个不安全的仓库地址指定配置。

```bash
[plugins."io.containerd.grpc.v1.cri".registry.mirrors."192.168.5.40:8866"]
  endpoint = ["http://192.168.5.40:8866"]

[plugins."io.containerd.grpc.v1.cri".registry.configs."192.168.5.40:8866".tls]
  insecure_skip_verify = true

[plugins."io.containerd.grpc.v1.cri".registry.mirrors."192.168.5.40:8867"]
  endpoint = ["http://192.168.5.40:8867"]

[plugins."io.containerd.grpc.v1.cri".registry.configs."192.168.5.40:8867".tls]
  insecure_skip_verify = true

[plugins."io.containerd.grpc.v1.cri".registry.mirrors."192.168.5.40:8868"]
  endpoint = ["http://192.168.5.40:8868"]

[plugins."io.containerd.grpc.v1.cri".registry.configs."192.168.5.40:8868".tls]
  insecure_skip_verify = true

```

这个配置告诉 containerd 对这些指定的 IP 和端口使用 HTTP 而非 HTTPS。配置更新后，记得重启 containerd 服务：

```bash
sudo systemctl restart containerd
```

## 基本命令对比

| **操作**                  | **Docker 命令**            | **crictl 命令**                |
|--------------------------|---------------------------|-------------------------------|
| 列出正在运行的容器       | `docker ps`               | `crictl ps`                   |
| 列出所有容器             | `docker ps -a`            | `crictl ps -a`                |
| 拉取镜像                 | `docker pull <image>`     | `crictl pull <image>`         |
| 查看本地镜像             | `docker images`           | `crictl images`               |
| 运行容器                 | `docker run <image>`      | `crictl create` 和 `crictl start` 需要使用 pod 和容器配置文件 |
| 停止容器                 | `docker stop <container>` | `crictl stop <container>`     |
| 删除容器                 | `docker rm <container>`   | `crictl rm <container>`       |
| 查看容器日志             | `docker logs <container>` | `crictl logs <container>`     |
| 执行命令在容器内部       | `docker exec <container> <command>` | `crictl exec -i -t <container> <command>` |
| 检查容器详情             | `docker inspect <container>` | `crictl inspect <container>` |
| 拉取容器状态             | `docker stats <container>`| `crictl stats <container>`    |
