---
title: 容器核心技术
top: true
cover: false
toc: true
mathjax: true
date: 2022-09-5 15:09:23
password:
summary: 有了虚拟机，为什么还要有容器？想要掌握容器技术要从哪里开始？Namespace,Cgroup,文件系统，网络。
tags:
- 容器
- Docker
- K8s
categories:
- Docker
---

# 容器核心技术

> Namespace做资源隔离，Cgroup做资源控制

## Docker

- 基于Linux内核的Cgroup，Namespace，以及UnionFS等技术，对进程进行封装隔离，属 于操作系统层面的虚拟化技术，由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其 为容器。
- Docker在容器的基础上，进行了进一步的封装，从文件系统、网络互联到进程隔离等等，极 大的简化了容器的创建和维护，使得 Docker 技术比虚拟机技术更为轻便、快捷。

## 为什么要用docker

- 更高效的利用系统资源 
- 更快速的启动时间 
- 一致的运行环境 
- 持续交付和部署 
- 更轻松的迁移 
- 更轻松的维护和扩展

## 虚拟机和容器运行态的对比

![image.png](https://s2.loli.net/2022/09/14/DNTibXw7xBMqsFj.png)

## 容器主要特性

- 安全性
- 隔离性
- 便携性
- 可配额

## Namespace

-  LinuxNamespace是一种LinuxKernel提供的资源隔离方案:
  - 系统可以为进程分配不同的 Namespace;
  - 并保证不同的 Namespace 资源独立分配、进程彼此隔离，即不同的 Namespace 下的进程互不干扰 。

### Linux 内核代码中 Namespace 的实现

```c
// 进程数据结构
struct task_struct 
{
	...
	/* namespaces */
	struct nsproxy *nsproxy;
	...
}

// Namespace数据结构
struct nsproxy 
{
	atomic_t count;
	struct uts_namespace *uts_ns;
	struct ipc_namespace *ipc_ns;
	struct mnt_namespace *mnt_ns;
	struct pid_namespace
	*pid_ns_for_children;
	struct net *net_ns;
}
```

### Linux 对 Namespace操作方法

- clone
   在创建新进程的系统调用时，可以通过 flags 参数指定需要新建的 Namespace 类型:

​		// CLONE_NEWCGROUP / CLONE_NEWIPC / CLONE_NEWNET / CLONE_NEWNS / CLONE_NEWPID / CLONE_NEWUSER / CLONE_NEWUTS

`int clone(int (*fn)(void *), void *child_stack, int flags, void *arg)`

-  setns
   该系统调用可以让调用进程加入某个已经存在的 Namespace 中: 

  `Int setns(int fd, int nstype)`

- unshare
   该系统调用可以将调用进程移动到新的 Namespace 下: 

  `int unshare(int flags)`

### 隔离性 – Linux Namespace

![](https://s2.loli.net/2022/09/14/LzCyg1YNK9x8VHX.png)

### 关于 namespace 的常用操作

- 查看当前系统的namespace: `lsns –t <type>`

- 查看某进程的namespace: `ls -la /proc/<pid>/ns/`

- 进入某namespace运行命令: `nsenter -t <pid> -n ip addr`





## Cgroups

- Cgroups(ControlGroups)是Linux下用于对一个或一组进程进行资源控制和监控的机制;
- 可以对诸如CPU使用时间、内存、磁盘I/O等进程所需的资源进行限制;
- 不同资源的具体管理工作由相应的Cgroup子系统(Subsystem)来实现;
- 针对不同类型的资源限制，只要将限制策略在不同的的子系统上进行关联即可;
- Cgroups在不同的系统资源管理子系统中以层级树(Hierarchy)的方式来组织管理:每个 Cgroup 都可以包含其他的子 Cgroup，因此子 Cgroup 能使用的资源除了受本 Cgroup 配置 的资源参数限制，还受到父 Cgroup 设置的资源限制 。

### Linux 内核代码中 Cgroups 的实现

```c
// 进程数据结构
struct task_struct 
{
	#ifdef CONFIG_CGROUPS 
  struct css_set __rcu *cgroups;
	struct list_head cg_list;
	#endif
}
// css_set 是 cgroup_subsys_state 对象的集合数据结构
struct css_set 
{
	/*
* Set of subsystem states, one for each subsystem. This array is * immutable after creation apart from the init_css_set during
* subsystem registration (at boot time).
*/
	struct cgroup_subsys_state *subsys[CGROUP_SUBSYS_COUNT];
}

```

### 可配额/可度量 - Control Groups (cgroups)

![image.png](https://s2.loli.net/2022/09/14/cLKanYt5Axjlp63.png)

**cgroups 实现了对资源的配额和度量。**

- **blkio**:这个子系统设置限制每个块设备的输入输出控制。例如:磁盘，光盘以及USB等等; 
-  **cpu**:这个子系统使用调度程序为cgroup任务提供CPU的访问;
-  **cpuacct**:产生cgroup任务的CPU资源报告;
-  **cpuset**:如果是多核心的CPU，这个子系统会为cgroup任务分配单独的CPU和内存;
-  **devices**:允许或拒绝cgroup任务对设备的访问;
-  **freezer**:暂停和恢复cgroup任务;
-  **memory**:设置每个cgroup的内存限制以及产生内存资源报告;
-  **net_cls**:标记每个网络包以供cgroup方便使用;
-  **ns**:名称空间子系统;
-  **pid**:进程标识子系统。

### cpuacct 子系统
 用于统计 Cgroup 及其子 Cgroup 下进程的 CPU 的使用情况。

- cpuacct.usage
   包含该 Cgroup 及其子 Cgroup 下进程使用 CPU 的时间，单位是 ns(纳秒)。

-  cpuacct.stat
   包含该 Cgroup 及其子 Cgroup 下进程使用的 CPU 时间，以及用户态和内核态的时间。

### memory 子系统

- memory.usage_in_bytes 

  cgroup下进程使用的内存，包含cgroup及其子cgroup下的进程使用的内存。

- memory.max_usage_in_bytes 

  cgroup下进程使用内存的最大值，包含子cgroup的内存使用量。

- memory.limit_in_bytes 

  设置Cgroup下进程最多能使用的内存。如果设置为-1，表示对该cgroup的内存使用不做限制。

- memory.oom_control

  设置是否在Cgroup中使用OOM(Out of Memory)Killer，默认为使用。当属于该cgroup 的进程使用的内存超过最大的限定值时，会立刻被OOM Killer处理。

## 文件系统 Union FS

- 将不同目录挂载到同一个虚拟文件系统下(unite several directories into asingle virtual filesystem)的文件系统。
- 支持为每一个成员目录(类似Git Branch)设定 readonly、readwrite 和 whiteout-able 权 限。
- 文件系统分层,对readonly权限的branch可以逻辑上进行修改(增量地,不影响readonly部 分的)。
- 通常UnionFS有两个用途,一方面可以将多个disk挂到同一个目录下,另一个更常用的就是将 一个 readonly 的 branch 和一个 writeable 的 branch 联合在一起。

## Docker 的文件系统

典型的 Linux 文件系统组成:

- Bootfs(bootfilesystem)

	- Bootloader - 引导加载 kernel，

	- Kernel - 当 kernel 被加载到内存中后 umount bootfs。

- rootfs(rootfilesystem)
	- /dev，/proc，/bin，/etc 等标准目录和文件。

	- 对于不同的 linux 发行版, bootfs 基本是一致的， 但 rootfs 会有差别。

## Docker 启动

### Linux

-  在启动后，首先将rootfs设置为readonly,进行一系列检查,然后将其切换为“readwrite” 供用户使用。

### Docker启动

- 初始化时也是将rootfs以readonly方式加载并检查，然而接下来利用unionmount的方式 将一个 readwrite 文件系统挂载在 readonly 的 rootfs 之上;
- 并且允许再次将下层的FS(filesystem)设定为readonly并且向上叠加;
- 这样一组readonly和一个writeable的结构构成一个container的运行时态,每一个FS被称 作一个 FS 层。

### 写操作

```
由于镜像具有共享特性，所以对容器可写层的操作需要依赖存储驱动提供的写时复制和用时分配
机制，以此来支持对容器可写层的修改，进而提高对存储和内存资源的利用率。
```

- 写时复制

写时复制，即 Copy-on-Write。一个镜像可以被多个容器使用，但是不需要在内存和磁盘上做多 个拷贝。在需要对镜像提供的文件进行修改时，该文件会从镜像的文件系统被复制到容器的可写 层的文件系统进行修改，而镜像里面的文件不会改变。不同容器对文件的修改都相互独立、互不 影响。

- 用时分配 

按需分配空间，而非提前分配，即当一个文件被创建出来后，才会分配空间。

### OverlayFS

OverlayFS 也是一种与 AUFS 类似的联合文件系统，同样属于文件级的存储驱动，包含了最初的 Overlay 和更新更稳定的 overlay2。

Overlay 只有两层:upper 层和 Lower 层。Lower 层代表镜像层，upper 层代表容器可写层

![image.png](https://s2.loli.net/2022/09/15/GImyhMNbaWC26EQ.png)

```sh
# demo
$ mkdir upper lower merged work
$ echo "from lower" > lower/in_lower.txt
$ echo "from upper" > upper/in_upper.txt
$ echo "from lower" > lower/in_both.txt
$ echo "from upper" > upper/in_both.txt
$ sudo mount -t overlay overlay -o lowerdir=`pwd`/lower,upperdir=`pwd`/upper,workdir=`pwd`/work `pwd`/merged
$ cat merged/in_both.txt
$ delete merged/in_both.txt $ delete merged/in_lower.txt $ delete merged/in_upper.txt
```

## 网络

### Null（--net=null）

- 把容器放入独立的网络空间但不做任何网络配置;
-  用户需要通过运行 docker network 命令来完成网络配置。

### Host

- 使用主机网络名空间，复用主机网络。

### Container

- 重用其他容器的网络。

### Bridge(--net=bridge)

- 使用 Linux 网桥和 iptables 提供容器互联，Docker 在每台主机上创建一个名叫 docker0 的网桥，通过 veth pair 来连接该主机的每一个 EndPoint。

## Docker优势

- 封装性:
	- 不需要再启动内核，所以应用扩缩容时可以秒速启动。
	- 资源利用率高，直接使用宿主机内核调度资源，性能损失小。 • 方便的 CPU、内存资源调整。
	- 能实现秒级快速回滚。
- 封装性:
	- 一键启动所有依赖服务，测试不用为搭建环境犯愁，PE 也不用 为建站复杂担心。
	- 镜像一次编译，随处使用。
	- 测试、生产环境高度一致(数据除外)。
- 隔离性:
	- 应用的运行环境和宿主机环境无关，完全由镜像控制，一台物 理机上部署多种环境的镜像测试。
	- 多个应用版本可以并存在机器上。
- 镜像增量分发:
	-	由于采用了 Union FS， 简单来说就是支持将不同的目录挂载到同 一个虚拟文件系统下，并实现一种 layer 的概念，每次发布只传输 变化的部分，节约带宽。
- 社区活跃:
	- Docker 命令简单、易用，社区十分活跃，且周边组件丰富。