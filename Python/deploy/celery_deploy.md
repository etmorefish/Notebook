# Celery

## 前言

### celery是什么？

> Celery 是一个开源的分布式任务队列系统，它用于处理异步任务、消息传递和定时任务调度。Celery 提供了一个强大的框架，使你能够将长时间运行的、异步的、并行的任务与你的应用程序分离开来，以提高性能和可伸缩性。

### 架构

![](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/celery_struct.png)

Celery的架构由三部分组成，消息中间件（message broker），任务执行单元（worker）和任务执行结果存储（task result store）组成。

**消息中间件**

Celery本身不提供消息服务，但是可以方便的和第三方提供的消息中间件集成。包括，RabbitMQ, Redis等等

**任务执行单元**

Worker是Celery提供的任务执行的单元，worker并发的运行在分布式的系统节点中。

**任务结果存储**

Task result store用来存储Worker执行的任务的结果，Celery支持以不同方式存储任务的结果，包括AMQP, redis等

另外， Celery还支持不同的并发和序列化的手段

- 并发：Prefork, Eventlet, gevent, threads/single threaded
- 序列化：pickle, json, yaml, msgpack. zlib, bzip2 compression， Cryptographic message signing 等等



### Celery 的主要特点和用途包括：

1. **异步任务处理**: Celery 允许你将耗时的任务（例如发送电子邮件、图像处理、数据导入等）放入队列中，而不必等待它们完成。这可以提高应用程序的响应速度。
2. **分布式任务队列**: Celery 支持分布式架构，你可以在多个节点上执行任务，从而分担负载和提高性能。
3. **定时任务调度**: Celery 具有内置的定时任务调度功能，允许你定期执行任务，例如生成报告、数据备份等。
4. **支持多后端**: Celery 可以与多种消息中间件后端（如 RabbitMQ、Redis、Amazon SQS 等）配合使用，以满足不同的需求。
5. **可扩展性**: Celery 提供了丰富的插件系统和拓展性，允许你根据需求自定义任务队列的行为。
6. **监控和管理**: Celery 提供了监控和管理工具，以便查看任务队列的状态、性能和日志。

### 使用场景

1. **web应用**：用户在网站进行某个操作需要很长时间完成时，我们可以将这种操作交给Celery执行，直接返回给用户，等到Celery执行完成以后通知用户，大大提好网站并发及用户体验感。

2. **任务场景**：需要批量在几百台机器执行某些命令或者任务，Celery可以轻松搞定。

3. **定时任务**：向定时导数据报表、定时发送通知类似场景，Celery可以提供管理接口和丰富的API。

## 实战

### 基本使用

#### 准备

为了演示方便，这里使用redis同时作为 `amqp broker` 和 `task result store`。

- 安装redis：`$ docker run -itd --name redis-test -p 6379:6379 redis`
- 创建环境并安装对应依赖：`pip install redis celery eventlet` 

#### 项目初始化

- celery 单文件案例 tasks.py

  ```python
  import datetime
  from celery import Celery
  from celery.schedules import crontab
  
  # 连接远程 redis服务的地址和端口号
  redis_host = "127.0.0.1"
  redis_port = "6379"
  
  app = Celery(
      "tasks",
      broker=f"redis://{redis_host}:{redis_port}/3",
      backend=f"redis://{redis_host}:{redis_port}/4",
  )
  
  
  @app.task
  def add(x, y):
      return x + y
  
  
  @app.task
  def subtract(x, y):
      return x - y
  
  
  @app.task
  def multiply(x, y):
      return x * y
  
  
  @app.task
  def divide(x, y):
      return x / y
  
  
  @app.task
  def print_time():
      print(f"The time is now {datetime.datetime.now()}")
  
  
  app.conf.beat_schedule = {
      "print-every-minute": {
          "task": "tasks.print_time",
          "schedule": crontab(minute="*"),
      },
  }
  
  ```




#### 项目的运行

- 启动步骤

  - 启动 **Celery worker**

    `celery -A tasks worker --loglevel=info  -P eventlet`

  - 启动 **Celery Beat** 守护进程

    `celery -A tasks beat --loglevel=info`

- 测试异步任务 test.py

  ```python
  from tasks import add, subtract, multiply, divide
  
  result = list()
  
result.append(add.delay(6, 3).get())
  result.append(subtract.delay(6, 3).get())
  result.append(multiply.delay(6, 3).get())
  result.append(divide.delay(6, 3).get())
  
  print(f"result:{result}")
  ```
  

#### 运行结果

![](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/celery_simple01.png)

可以看到定时任务会自动触发，异步任务需要我们手动触发。

### 进阶使用

添加`flower`进行监控，

- 安装：`pip install flower`
- 启动：`celery -A tasks flower --port=5555 -P eventlet`

![](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/flower_monitor_web.png)





## Todo

- 多任务结构
- flask项目使用celery
- 远程调用
- 多机部署
- 错误重试
- 回调
- 设计工作流程 ¶



