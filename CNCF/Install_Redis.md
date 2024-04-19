# K8s 安装 redis

> 背景需求：我希望在指定的命名空间(sp1)里面安装一个带有密码的redis，并且希望数据持久化。


部署 Redis 到 Kubernetes (k8s) 需要涵盖多个步骤，包括创建配置文件、密码设置、持久化存储等。要在特定的命名空间（例如 `sp1`）内进行所有操作，你需要在每个 Kubernetes 资源配置中指定 `namespace` 字段，或在命令行操作时使用 `-n sp1` 参数来指定命名空间。这样做可以确保你的所有资源都被创建在同一个命名空间中。下面是如何在特定命名空间 `sp1` 中进行所有操作的步骤。

> 相关文件树
redis
├── redis.conf
├── redis-configmap.yaml
├── redis-deployment.yaml
├── redis-pvc.yaml
└── redis-service.yaml

### 步骤 1: 准备 Redis 配置文件
首先，创建一个 Redis 配置文件 redis.conf，可以在你的本地机器上编辑这个文件：

```conf
# redis.conf
requirepass 123456  # 设置密码
```
保存这个配置文件，我们将在后续步骤中使用它。

### 步骤 2: 创建命名空间

首先，如果命名空间 `sp1` 还没有创建，你需要先创建它：

```yaml
# sp1-namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: sp1
```

使用以下命令来创建命名空间：

```bash
kubectl create -f sp1-namespace.yaml
```

对于每个配置文件（ConfigMap、PVC、Deployment、Service），你需要在 `metadata` 部分加上 `namespace: sp1`。
部署 Redis 到 Kubernetes (k8s) 需要涵盖多个步骤，包括创建配置文件、密码设置、持久化存储等。下面我将逐步引导你完成这些步骤。


### 步骤 3: 创建 Kubernetes ConfigMap

使用刚才创建的 `redis.conf` 文件创建一个 ConfigMap，这样 Redis 实例就可以使用这个配置文件了：

```yaml
# redis-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: redis-configmap
  namespace: sp1
data:
  redis.conf: |
    requirepass 123456
```

使用下面的命令来创建 ConfigMap：

```bash
kubectl create -f redis-configmap.yaml
```

或者使用 kubectl 从文件直接创建 ConfigMap：如果你能够从命令行访问该文件，可以直接使用 kubectl create configmap 命令创建 ConfigMap：
```bash
kubectl create configmap redis-configmap --from-file=redis.conf=/home/mis/app/sp1/redi
```

### 步骤 3.1: 创建 Secret
由于需要密码访问，所以需要创建一个 Secret 来存储密码。
如果没有能会出现如下报错：

```bash
Warning  Failed     2m14s (x8 over 3m44s)  kubelet            Error: secret "redis-secret" not found
```
这意味着在尝试从 redis-secret 获取 REDIS_PASSWORD 环境变量时失败了，因为这个 Secret 不存在。为了解决这个问题，你需要创建一个名为 redis-secret 的 Secret，并确保它包含了名为 password 的键，其值为你的 Redis 密码。

可以使用以下命令来创建这个 Secret：

```bash
kubectl create secret generic redis-secret --from-literal=password='123456' -n sp1
```
这条命令创建了一个名为 redis-secret 的 Secret，它包含一个键 password，其值为 123456，并且确保它被创建在 sp1 命名空间中。

### 3.2 创建 Secret YAML 文件（另一种方式）
或者你可以创建一个 YAML 文件（比如 redis-secret.yaml）来定义 Secret：
首先，你需要将你的密码转换为 base64 编码。你可以在命令行中使用 echo 和 base64 命令来做到这一点：`echo -n '123456' | base64`  -> MTIzNDU2

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: redis-secret
  namespace: sp1
type: Opaque
data:
  password: MTIzNDU2
```

### 步骤 4: 创建持久卷

接下来，我们需要为 Redis 数据创建一个持久化卷。这里我们使用 PersistentVolumeClaim (PVC) 来请求存储资源。

```yaml
# redis-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: redis-pvc
  namespace: sp1
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

创建 PVC：

```bash
kubectl create -f redis-pvc.yaml
```

### 步骤 5: 部署 Redis

使用 Deployment 或 StatefulSet 来部署 Redis。这里，我们使用 Deployment，并确保配置了上述的 ConfigMap 和 PVC。

```yaml
# redis-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  namespace: sp1
spec:
  selector:
    matchLabels:
      app: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:latest
        ports:
        - containerPort: 6379
        volumeMounts:
        - name: config
          mountPath: /usr/local/etc/redis/redis.conf
          subPath: redis.conf
        - name: data
          mountPath: /data
        env:
        - name: REDIS_PASSWORD
          valueFrom:
            secretKeyRef:
              name: redis-secret
              key: password
      volumes:
      - name: config
        configMap:
          name: redis-configmap
      - name: data
        persistentVolumeClaim:
          claimName: redis-pvc
```

创建部署：

```bash
kubectl create -f redis-deployment.yaml
```

### 步骤 6: 创建服务

最后，创建一个 Kubernetes 服务 (Service) 来暴露 Redis 实例：

```yaml
# redis-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-service
  namespace: sp1
spec:
  type: ClusterIP
  ports:
  - port: 6379
    targetPort: 6379
  selector:
    app: redis
```

创建服务：

```bash
kubectl create -f redis-service.yaml
```

### 验证部署

部署完成后，你可以使用以下命令来检查 Redis 的状态：

```bash
kubectl get pods
kubectl logs <redis-pod-name>
```

以上步骤应该能帮你在 Kubernetes 上部署一个配置了密码、配置文件和数据持久化的 Redis 实例。如果有任何问题，可以进一步调试或查看日志以确定问题所在。

---
# 查看redis相关的部署情况

要查看在 Kubernetes 上部署的 Redis 实例的相关配置，你可以通过多种方法获取配置详情，包括配置文件、环境变量、使用的 ConfigMap、Secrets 等。这里将详细介绍几种查看配置的方法。

### 1. 查看 Deployment 配置

查看 Redis Deployment 的配置可以帮助你了解如何配置容器，包括环境变量、卷挂载等。

```bash
kubectl get deployment redis -n sp1 -o yaml
```

这个命令将输出 Redis Deployment 的 YAML 配置，你可以从中看到配置的环境变量、挂载的卷、使用的镜像等详细信息。

### 2. 查看 ConfigMap

如果你的 Redis 使用 ConfigMap 来配置，可以查看 ConfigMap 的内容来了解配置文件的实际内容。

```bash
kubectl get configmap redis-configmap -n sp1 -o yaml
```

这个命令会显示 ConfigMap 中的所有数据，你可以查看 `redis.conf` 文件的内容是如何配置的。

### 3. 查看 Secrets

如果你的 Redis 配置中使用了 Kubernetes Secrets（例如，存储密码），可以通过以下命令查看：

```bash
kubectl get secret redis-secret -n sp1 -o yaml
```

注意，出于安全考虑，Secrets 中的数据将是 base64 编码的。如果需要查看实际内容，你可以对其进行解码：

```bash
echo 'MTIzNDU2' | base64 --decode    # 123456
```

将 `'base64-encoded-data'` 替换为你从 Secrets 获取的实际编码数据。

### 4. 查看 Pod 的运行状态和环境变量

查看具体运行中的 Pod 的详情，可以帮助你确认环境变量和挂载的卷。

```bash
kubectl describe pod -l app=redis -n sp1
```

这个命令会列出匹配标签 `app=redis` 的所有 Pod 的详细信息，包括环境变量、卷挂载信息等。

### 5. 查看日志

查看 Redis Pod 的日志可以帮助你理解实例的运行情况，包括任何配置错误或启动警告。

```bash
# 替换 <pod-name> 为实际的 Pod 名称
kubectl logs <pod-name> -n sp1
```

通过查看日志，你可以确认 Redis 是否成功启动，并根据配置运行。

使用这些命令和方法，你可以全面了解你的 Redis 部署在 Kubernetes 中的配置和状态。这对于故障诊断和验证配置非常有用。

---
## Q:如何扩缩容？
在 Kubernetes 中扩缩容 Redis 实例主要取决于你的部署方式和 Redis 的配置。Redis 作为一个数据存储服务，其扩缩容不同于无状态应用的简单水平扩展。下面是一些关键考虑和可行的方法：

### 1. 单实例 Redis 的扩缩容

如果你在 Kubernetes 上运行的是单节点 Redis（不是集群模式），你只能通过调整资源限制（CPU 和内存）来进行"垂直扩缩容"。这涉及到修改 Redis Deployment 的资源请求和限制，然后应用这些更改。例如：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  namespace: sp1
spec:
  template:
    spec:
      containers:
      - name: redis
        image: redis:latest
        resources:
          requests:
            cpu: "500m"
            memory: "1Gi"
          limits:
            cpu: "1"
            memory: "2Gi"
```

在这个例子中，你可以根据需要增加或减少 CPU 和内存资源。

### 2. Redis 集群的扩缩容

对于 Redis 集群，你可以通过增加或减少集群中的节点数来进行水平扩缩容。这通常涉及到修改一个 StatefulSet 或自定义资源（如果使用 Operator 管理）的副本数。例如，如果 Redis 集群由一个 StatefulSet 管理，你可以这样修改副本数：

```bash
kubectl scale statefulset redis-cluster --replicas=6 -n sp1
```

这个命令将 Redis 集群的节点数扩展到 6 个副本。

### 3. 考虑持久化和数据一致性

扩缩容操作需要考虑数据的持久化和一致性。对于任何涉及数据存储的服务，确保在扩缩容操作中处理好数据的迁移和备份是非常重要的。对于 Redis，使用持久卷和适当的数据备份策略可以帮助保护数据不丢失。

### 4. 自动化扩缩容

在 Kubernetes 环境中，还可以设置基于特定指标（如 CPU 使用率、内存使用量或其他自定义指标）的自动扩缩容。这通常通过 Horizontal Pod Autoscaler (HPA) 实现：

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: redis-hpa
  namespace: sp1
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: redis
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 75
```

这个例子中，HPA 将根据 CPU 利用率来自动调整 Redis Pod 的数量，范围从 1 到 10 个副本。

### 结论

扩缩容 Redis 在 Kubernetes 上是可能的，但方法取决于你的部署类型（单实例或集群）。在实施任何扩缩容操作时，特别是涉及到集群和数据一致性的时候，必须非常小心。