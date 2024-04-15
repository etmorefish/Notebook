# Redis Cluster with Docker Compose

> Docker Compose

**Usage  

**Docker 撰写 yaml 详细信息：**

参考: [https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)  


顶层：

1. `services` ：容器的配置，你可以认为它与 `docker run` 命令类似。
2. `networks` ：自定义网络管理。创建自定义网络， `services` 端可以引用此网络配置，可以同时与多个服务一起使用。
3. `volumes` ：管理卷。可以同时与多个服务一起使用。
4. `version` ：Docker compose 版本。

首先创建卷（挂载）文件：

```sh
# run this for loop to create each redis container configuration file
for port in $(seq 1 6); do
  mkdir -p ./node-${port}/conf
  touch ./node-${port}/conf/redis.conf
  cat << EOF >./node-${port}/conf/redis.conf
#端口
port 6379
#非保护模式
protected-mode no
bind 0.0.0.0
#启用集群模式
cluster-enabled yes
cluster-config-file nodes.conf
#超时时间
cluster-node-timeout 5000
#集群各节点IP地址
cluster-announce-ip 172.38.0.1${port}
#集群节点映射端口 
cluster-announce-port 6379
#集群总线端口 
cluster-announce-bus-port 16379
#开启aof持久化策略
appendonly yes
#后台运行
#daemonize yes
#集群加密
#masterauth 123456
#requirepass 123456
EOF
done


```

Write the docker-compose.yaml file:  
编写 docker-compose.yaml 文件：

```yml
version: "3"
services:
    redis-1:
        image: redis
        command: ["redis-server","/etc/redis/redis.conf"]
        volumes:
            - ./node-1/data:/data
            - ./node-1/conf/redis.conf:/etc/redis/redis.conf
        ports:
            - 6371:6379
            - 16371:16379
        networks:
            redis_network:
                ipv4_address: 172.38.0.11
    redis-2:
        image: redis
        command: ["redis-server", "/etc/redis/redis.conf"]
        volumes:
            - ./node-2/data:/data
            - ./node-2/conf/redis.conf:/etc/redis/redis.conf
        ports:
            - "6372:6379"
            - 16372:16379
        networks:
            redis_network:
                ipv4_address: 172.38.0.12
    redis-3:
        image: redis
        command: ["redis-server", "/etc/redis/redis.conf"]
        volumes:
            - ./node-3/data:/data
            - ./node-3/conf/redis.conf:/etc/redis/redis.conf
        ports:
            - "6373:6379"
            - 16373:16379
        networks:
            redis_network:
                ipv4_address: 172.38.0.13
    redis-4:
        image: redis
        command: ["redis-server", "/etc/redis/redis.conf"]
        volumes:
            - ./node-4/data:/data
            - ./node-4/conf/redis.conf:/etc/redis/redis.conf
        ports:
            - "6374:6379"
            - 16374:16379
        networks:
            redis_network:
                ipv4_address: 172.38.0.14
    redis-5:
        image: redis
        command: ["redis-server", "/etc/redis/redis.conf"]
        volumes:
            - ./node-5/data:/data
            - ./node-5/conf/redis.conf:/etc/redis/redis.conf
        ports:
            - "6375:6379"
            - 16375:16379
        networks:
            redis_network:
                ipv4_address: 172.38.0.15
    redis-6:
        image: redis
        command: ["redis-server", "/etc/redis/redis.conf"]
        volumes:
            - ./node-6/data:/data
            - ./node-6/conf/redis.conf:/etc/redis/redis.conf
        ports:
            - "6376:6379"
            - "16376:16379"
        networks:
            redis_network:
                ipv4_address: 172.38.0.16
networks:
    redis_network:
        driver: bridge
        ipam:
            config:
                - subnet: 172.38.0.0/16
                  gateway: 172.38.0.1

```

Command:  
命令：

```shell
# check running container
$ docker compose ps
CONTAINER ID   IMAGE           COMMAND                   CREATED         STATUS         PORTS                                                                                      NAMES
ba7b65b595ca   redis           "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:6372->6379/tcp, :::6372->6379/tcp, 0.0.0.0:16372->16379/tcp, :::16372->16379/tcp   redis-cluster-redis-2-1
c03b41296f77   redis           "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:6374->6379/tcp, :::6374->6379/tcp, 0.0.0.0:16374->16379/tcp, :::16374->16379/tcp   redis-cluster-redis-4-1
c5d82347505d   redis           "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:6373->6379/tcp, :::6373->6379/tcp, 0.0.0.0:16373->16379/tcp, :::16373->16379/tcp   redis-cluster-redis-3-1
20deef95158e   redis           "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:6371->6379/tcp, :::6371->6379/tcp, 0.0.0.0:16371->16379/tcp, :::16371->16379/tcp   redis-cluster-redis-1-1
8239a36f1916   redis           "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:6375->6379/tcp, :::6375->6379/tcp, 0.0.0.0:16375->16379/tcp, :::16375->16379/tcp   redis-cluster-redis-5-1
6209bac99938   redis           "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:6376->6379/tcp, :::6376->6379/tcp, 0.0.0.0:16376->16379/tcp, :::16376->16379/tcp   redis-cluster-redis-6-1

# If created successful, enter to the redis-1 to create cluster
root@20deef95158e:/data# redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.38.0.15:6379 to 172.38.0.11:6379
Adding replica 172.38.0.16:6379 to 172.38.0.12:6379
Adding replica 172.38.0.14:6379 to 172.38.0.13:6379
M: dbb3a7651c7e4f6eea79cdde030de79c49b21ab1 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
M: 7f91e6da603c3572291a373a8bdd187698c78547 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: 2201cec0f0b0fc62ffa342e48b46c41e6d15cee7 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: ca6b40656420bf0108931cf261b9b38c9b11341f 172.38.0.14:6379
   replicates 2201cec0f0b0fc62ffa342e48b46c41e6d15cee7
S: 20723026987235ed7d7376725660ff0cb879248d 172.38.0.15:6379
   replicates dbb3a7651c7e4f6eea79cdde030de79c49b21ab1
S: 3de5cd49992029ff5188ec8fddbebaadd88bf852 172.38.0.16:6379
   replicates 7f91e6da603c3572291a373a8bdd187698c78547
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 172.38.0.11:6379)
M: dbb3a7651c7e4f6eea79cdde030de79c49b21ab1 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 3de5cd49992029ff5188ec8fddbebaadd88bf852 172.38.0.16:6379
   slots: (0 slots) slave
   replicates 7f91e6da603c3572291a373a8bdd187698c78547
S: 20723026987235ed7d7376725660ff0cb879248d 172.38.0.15:6379
   slots: (0 slots) slave
   replicates dbb3a7651c7e4f6eea79cdde030de79c49b21ab1
M: 7f91e6da603c3572291a373a8bdd187698c78547 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
M: 2201cec0f0b0fc62ffa342e48b46c41e6d15cee7 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: ca6b40656420bf0108931cf261b9b38c9b11341f 172.38.0.14:6379
   slots: (0 slots) slave
   replicates 2201cec0f0b0fc62ffa342e48b46c41e6d15cee7
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.


# Check again, enter to the redis-cli with cluster
root@b59ad0535541:/data$ redis-cli -c

# check the relationship of all redis nodes
127.0.0.1:6379> CLUSTER nodes
3de5cd49992029ff5188ec8fddbebaadd88bf852 172.38.0.16:6379@16379 slave 7f91e6da603c3572291a373a8bdd187698c78547 0 1713170694456 2 connected
dbb3a7651c7e4f6eea79cdde030de79c49b21ab1 172.38.0.11:6379@16379 myself,master - 0 1713170693000 1 connected 0-5460
20723026987235ed7d7376725660ff0cb879248d 172.38.0.15:6379@16379 slave dbb3a7651c7e4f6eea79cdde030de79c49b21ab1 0 1713170693954 1 connected
7f91e6da603c3572291a373a8bdd187698c78547 172.38.0.12:6379@16379 master - 0 1713170693451 2 connected 5461-10922
2201cec0f0b0fc62ffa342e48b46c41e6d15cee7 172.38.0.13:6379@16379 master - 0 1713170695059 3 connected 10923-16383
ca6b40656420bf0108931cf261b9b38c9b11341f 172.38.0.14:6379@16379 slave 2201cec0f0b0fc62ffa342e48b46c41e6d15cee7 0 1713170693050 3 connected

```

Command: `$ redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1`  

*   `redis-cli` ：Redis 命令行客户端工具。
*   `--cluster create` ：创建 Redis 集群的命令。
*   `172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379` ：列出集群中的各个节点，包括它们的 IP 地址和端口号。这里列出了 6 个节点。
*   `--cluster-replicas 1` ：设置每个主节点的副本数为 1，即每个主节点都会有一个对应的副本节点。
 
可以根据需要提供更多或更少的节点，Redis 会自动相应地配置集群。在此示例中，Redis 推断你打算创建一个具有 3 个主节点的集群，每个主节点都有一个相应的副本节点。

**Redis Cluster Relationship Diagram:  
Redis 集群关系图：**
![](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/2024/240215-redis_cluster.png)