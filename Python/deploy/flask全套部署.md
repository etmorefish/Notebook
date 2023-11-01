# 	Flask + Gunicorn + supervisor

> 假设你的项目目录下面有以下文件

## 项目初始化

- 项目启动文件 - app.py

```python
from flask import Flask
from gevent.pywsgi import WSGIServer  
from views import api_bp

def create_app():
    app = Flask(__name__)
    app.register_blueprint(api_bp,url_prefix="/api")

    return app

GAPP = create_app()

if __name__ == "__main__":
    server = WSGIServer(("", 5000), GAPP)
    server.serve_forever()

```

- 视图函数 - views.py

```python
from flask import Blueprint, jsonify


api_bp = Blueprint("api", __name__)


@api_bp.route("/ping")
def ping():
    return jsonify({"ping": "PONG!"})
```

- 通过Gouncorn启动项目的配置文件 - gunicorn_conf.py

```python
import multiprocessing

bind = '0.0.0.0:5000'                			  # 绑定ip和端口号
backlog = 512                                     # 监听队列
timeout = 30                                      # 超时
worker_class = 'gevent'                           # 使用gevent模式，还可以使用sync 模式，默认的是sync模式, tornado
workers = multiprocessing.cpu_count() + 1         # 进程数
threads = 4                                       # 指定每个进程开启的线程数
loglevel = 'info'                                 # 日志级别，这个日志级别指的是错误日志的级别，而访问日志的级别无法设置
access_log_format = '%(t)s %(p)s %(h)s "%(r)s" %(s)s %(L)s %(b)s %(f)s" "%(a)s"'   # 设置gunicorn访问日志格式，错误日志无法设置
worker_connections = 1000
daemon = False
pidfile = None

""" 
其每个选项的含义如下： 
h remote address 
l '-' 
u currently '-', may be user name in future releases 
t date of the request 
r status line (e.g. ``GET / HTTP/1.1``) 
s status 
b response length or '-' 
f referer 
a user agent 
T request time in seconds 
D request time in microseconds 
L request time in decimal seconds 
p process ID 
"""

chdir = '/home/ubuntu/demo'                  # gunicorn要切换到的目的工作目录
accesslog = "/home/ubuntu/demo/gunicorn_hr_access.log"    # 访问日志文件
errorlog = "/home/ubuntu/demo/gunicorn_hr_error.log"      # 错误日志文件
```



## 项目的运行：

- 在项目根目录下新建虚拟环境：`python -m venv .venv`

- 进入虚拟环境：`source .venv/bin/activate `

- 安装依赖：`pip install flask gunicorn gevent ` 

- `cd`到项目目录，此时就可以运行命令后台启动程序：`nohup python -m gunicorn -c gunicorn_conf.py app:GAPP > /dev/null 2>&1 &`

  > 如果使用的是conda新建的虚拟环境 `nohup /home/ubuntu/miniconda3/envs/demo_venv/bin/python -m gunicorn -c gunicorn_conf.py app:GAPP > /dev/null 2>&1 &`  
  >
  > **!!! 不推荐使用`conda`创建`python`虚拟环境，部署`python`程序.**

  >  gevent： 可以实现异步请求处理以及改进的并发性
  >
  >  gunicorn: **多线程异步接口**

- 此时我们通过 `ps -ef |grep gunicorn_conf`就可以看到我们多个python程序的进程



## 查看结果

```shell
(.venv) ➜  demo nohup python -m gunicorn -c gunicorn_conf.py app:GAPP > /dev/null 2>&1 &
[1] 1412223
(.venv) ➜  demo ps -ef |grep gunicorn_conf
ubuntu   1412223 1382571  0 13:11 pts/21   00:00:00 python -m gunicorn -c gunicorn_conf.py app:GAPP
ubuntu   1412226 1412223  0 13:11 pts/21   00:00:00 python -m gunicorn -c gunicorn_conf.py app:GAPP
ubuntu   1412227 1412223  0 13:11 pts/21   00:00:00 python -m gunicorn -c gunicorn_conf.py app:GAPP
ubuntu   1412228 1412223  0 13:11 pts/21   00:00:00 python -m gunicorn -c gunicorn_conf.py app:GAPP
ubuntu   1412310 1382571  0 13:11 pts/21   00:00:00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn --exclude-dir=.idea --exclude-dir=.tox gunicorn_conf
(.venv) ➜  demo curl 127.0.0.1:5000/api/ping
{"ping":"PONG!"}
```

可以看到我们部署好程序后可以看到系统里面有四个进程，并且访问`127.0.0.1:5000/api/ping`直接返回了接口的数据。



## 项目的停止和重启

![运行截图](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/flask_gunicorn_deploy.png)

### stop

停止服务需要我们手动杀掉进程的`PID`,虽然这里我们可以看到四个`pid`,但是我们只需要杀掉第一个主进程`pid = 1412223`就好了，使用 `kill -9 1412223`来停止服务。

此时我们访问`127.0.0.1:5000/api/ping`, 系统会拒绝请求

### restart

重启项目，我们继续运行之前的命令就可以了。

### 到此为止，我们的flask部署到生产基本就结束了

​		流程顺利，但是唯一让人不爽的就是 每次重启服务都要敲很多命令，能不能一键启动，一键停止，还可以查看日志的那种。有，那就是`Supervisor`.



# Supervisor

> 是一个用于进程控制和管理的客户端/服务器系统。它允许您监控、启动、停止和重启后台进程，以确保它们持续运行并能够应对失败或崩溃。

## 安装

`pip install supervisor`

## 配置文件

- 创建配置文件的默认路径,用于后面存放各种python的server, `sudo mkdir /etc/supervisord.d`

- 生成一个配置文件 `echo_supervisord_conf > supervisord.conf`

- 修改并移动`supervisord.conf`配置到`/etc/supervisord.conf`

  ```shell
  [unix_http_server]
  file=/tmp/supervisor.sock   ; the path to the socket file
  
  ;守护进程，可在 web 上访问
  [inet_http_server]         ; inet (TCP) server disabled by default
  port=0.0.0.0:9001        ; ip_address:port specifier, *:port for all iface
  username=user              ; default is no username (open server)
  password=123456               ; default is no password (open server)
  
  [supervisord]
  logfile=/tmp/supervisord.log ; main log file; default $CWD/supervisord.log
  logfile_maxbytes=50MB        ; max main logfile bytes b4 rotation; default 50MB
  logfile_backups=10           ; # of main logfile backups; 0 means none, default 10
  loglevel=info                ; log level; default info; others: debug,warn,trace
  pidfile=/tmp/supervisord.pid ; supervisord pidfile; default supervisord.pid
  nodaemon=false               ; start in foreground if true; default false
  silent=false                 ; no logs to stdout if true; default false
  minfds=1024                  ; min. avail startup file descriptors; default 1024
  minprocs=200                 ; min. avail process descriptors;default 200
  
  [rpcinterface:supervisor]
  supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface
  
  [include]
  files = /etc/supervisord.d/*.conf
  ```

  > 以上是我的配置文件，主要是修改`inet_http_server`和`include`两部分
  >
  > include -> files 这个参数就是我们待会要监控的服务的配置文件

- 添加需要监控的服务的配置文件 -> /etc/supervisord.d/demo.conf

  ```shell
  [program:demo]
  ;command=bash -c "source /home/ubuntu/demo/.venv/bin/activate v002_venv &&  gunicorn -c gunicorn_jwt_conf.py run_jwt_prd:GAPP"       ; 被监控的进程路径,如果你使用的是conda环境，请用这种
  
  command=bash -c "source .venv/bin/activate && gunicorn -c gunicorn_conf.py app:GAPP "					 ; 被监控的进程路径
  directory=/home/ubuntu/demo    ; 执行前要不要先cd到目录去，一般不用
  priority=1                  ; 数字越高，优先级越高
  numprocs=1                  ; 启动几个进程
  autostart=true              ; 随着supervisord的启动而启动
  autorestart=true            ; 自动重启 当然要选上了
  startretries=5              ; 启动失败时的最多重试次数
  exitcodes=0                 ; 正常退出代码（是说退出代码是这个时就不再重启了吗？待确定）
  stopsignal=KILL             ; 用来杀死进程的信号
  stopwaitsecs=10             ; 发送SIGKILL前的等待时间
  stderr_logfile=/home/ubuntu/demo/log/demo.err.log
  stdout_logfile=/home/ubuntu/demo/log/demo.out.log   
  ```

## 启动

- 完成配置文件后，执行以下脚本启动（**supervisord -c** 启动指定路径的配置）：   `/home/ubuntu/demo/.venv/bin/supervisord -c /etc/supervisord.conf`

  - 启动成功后访问我们前面设置的`ip + port`,就可以看到管理面板，可以在管理面板上启动/停止服务，以及查看日志。

    ![](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/supervisor.png)

- 查看状态： `/home/ubuntu/demo/.venv/bin/supervisorctl -c /etc/supervisord.conf status`

- 重载配置文件:  `/home/ubuntu/demo/.venv/bin/supervisorctl -c /etc/supervisord.conf reload`

- 关闭服务：`/home/ubuntu/demo/.venv/bin/supervisorctl -c /etc/supervisord.conf shutdown`