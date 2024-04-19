# kubectl 常用命令

`kubectl` 是 Kubernetes 的命令行工具，用于与 Kubernetes 集群进行交互。下面列举了一些最常用的 `kubectl` 命令，这些命令涵盖了从查询资源到部署和管理应用程序的各种操作：

1. **查看和获取资源信息**：
   - `kubectl get pods`：列出所有 pod。
   - `kubectl get all`：列出当前命名空间下的所有资源。
   - `kubectl get deployment`：列出所有部署。
   - `kubectl get service`：列出所有服务。
   - `kubectl describe pod [pod-name]`：显示特定 pod 的详细信息。

2. **创建和删除资源**：
   - `kubectl create -f [file.yaml]`：根据 YAML 配置文件创建资源。
   - `kubectl delete -f [file.yaml]`：删除 YAML 文件中定义的资源。
   - `kubectl delete pod [pod-name]`：直接删除一个 pod。

3. **修改和更新资源**：
   - `kubectl apply -f [file.yaml]`：应用 YAML 文件中的配置更新现有资源。
   - `kubectl edit pod [pod-name]`：编辑 pod 的配置。

4. **资源日志和调试**：
   - `kubectl logs [pod-name]`：获取 pod 的日志。
   - `kubectl exec -it [pod-name] -- /bin/bash`：在运行中的容器里执行交互式命令行（例如 Bash）。

5. **命名空间操作**：
   - `kubectl get namespaces`：列出所有命名空间。
   - `kubectl create namespace [name]`：创建新的命名空间。
   - `kubectl config set-context --current --namespace=[namespace]`：切换到指定的命名空间。

6. **配置和管理集群**：
   - `kubectl config view`：查看 kubectl 配置信息。
   - `kubectl cluster-info`：显示集群的信息。
   - `kubectl top node`：显示节点的资源使用情况。

7. **标签和选择器**：
   - `kubectl label pods [pod-name] [label-key]=[label-value]`：给 pod 添加新的标签。
   - `kubectl get pods --selector=[label-key]=[label-value]`：选择具有特定标签的 pod。

8. **扩展应用**：
   - `kubectl scale deployment [deployment-name] --replicas=[num]`：调整部署的副本数。

这些命令基本涵盖了日常使用 Kubernetes 进行资源管理和故障排查的大部分需求。熟练使用这些命令可以大大提高管理和维护 Kubernetes 集群的效率。

# kubectl 常用命令缩写

在使用 `kubectl` 命令时，确实可以使用一些缩写来简化命令，使其更加简洁易打。这些缩写主要适用于资源类型，如下所示：

1. **缩写常用的资源类型**：
   - `po` for `pods`
   - `rs` for `replicasets`
   - `deploy` for `deployments`
   - `svc` for `services`
   - `ns` for `namespaces`
   - `sa` for `serviceaccounts`
   - `cm` for `configmaps`
   - `ing` for `ingresses`
   - `pvc` for `persistentvolumeclaims`

2. **命令行选项的缩写**：
   - `-n` for `--namespace`
   - `--all-namespaces` 可以缩写为 `-A`
   - `-o` for `--output`
   - `--kubeconfig` 可以有时候简写（取决于具体用途）

### 示例使用缩写的命令

下面是一些使用这些缩写的例子，可以让你更快地输入命令：

- 列出所有 pods：
  ```bash
  kubectl get po
  ```

- 查看特定命名空间的服务：
  ```bash
  kubectl get svc -n mynamespace
  ```

- 删除某个命名空间中的 deployment：
  ```bash
  kubectl delete deploy my-deployment -n mynamespace
  ```

- 查看所有命名空间的资源：
  ```bash
  kubectl get pods -A
  ```

使用这些缩写可以大大加快命令行操作的速度，特别是在日常管理 Kubernetes 资源时。不过，值得注意的是，虽然这些缩写在命令行操作中非常便捷，但在编写脚本或文档时，使用完整的资源名称可以提高可读性和维护性。