# Paramiko - SSHv2 的 Python 实现

Paramiko 是一个用于 Python 的模块，提供了对 SSH2 协议的支持。SSH（Secure Shell）是一种网络协议，用于在不安全的网络中安全地进行系统管理和文件传输。Paramiko 使得在 Python 中使用 SSH 进行远程连接、执行命令和传输文件变得非常方便。

## Paramiko 的主要功能

1. 远程连接：可以使用 SSH 连接到远程服务器。
2. 执行命令：在远程服务器上执行命令，并获取输出。
3. 文件传输：通过 SFTP（SSH File Transfer Protocol）上传和下载文件。
4. 密钥认证：支持使用密码或私钥文件进行身份验证。

## 使用 Paramiko 的场景

- 系统管理：远程管理服务器，执行命令，进行系统维护和监控。
- 自动化脚本：编写自动化脚本，自动执行日常任务。
- 文件传输：在本地和远程服务器之间安全地传输文件。

## 安装

使用 pip 安装 Paramiko：

```
pip install paramiko
```

## 基本用法

使用 Paramiko 进行 SSH 通信的基本步骤包括：

1. 创建 SSH 客户端对象
2. 连接到 SSH 服务器
3. 执行命令或上传下载文件
4. 关闭连接

## demo
```python
import paramiko

"""
在本地生成 SSH 密钥对
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa

将公钥复制到远程服务器
ssh-copy-id -i ~/.ssh/id_rsa.pub your_username@remote_id
这将把本地生成的公钥 id_rsa.pub 添加到远程服务器的 ~/.ssh/authorized_keys 文件中。
"""

# 1、创建 SSH 客户端对象
ssh = paramiko.SSHClient()

# 自动添加远程服务器的 SSH 密钥（不建议在生产环境中使用）
ssh.set_ubuntusing_host_key_policy(paramiko.AutoAddPolicy())

# 1.1、使用密码连接到远程服务器
ssh.connect(hostname='192.168.x.xxx', username='ubuntu', password='123456')

# 1.2、使用私钥文件连接到远程服务器 -pkey
# private_key = paramiko.RSAKey.from_private_key_file('/home/ubuntu/.ssh/id_rsa')
# ssh.connect(hostname='192.168.x.xxx', username='ubuntu', pkey=private_key)

# 1.3、使用私钥文件连接到远程服务器 -key_filename
ssh.connect(hostname='192.168.x.xxx', username='ubuntu', key_filename='/home/ubuntu/.ssh/id_rsa')

# # 2、执行命令
# stdin, stdout, stderr = ssh.exec_command('ls -l')

# # 获取命令输出
# print(stdout.read().decode())

# 3、创建 SFTP 客户端对象
sftp = ssh.open_sftp()

# # 上传文件
# sftp.put('/tmp/info55.txt', '/tmp/info55.txt')

# # 下载文件
# sftp.get('/tmp/info235.txt', '/tmp/info235.txt')

filepath = "/tmp/"
filelist = sftp.listdir(filepath)
for file in filelist:
    if file.startswith("info"):
        pass
print(filelist)

# 关闭 SFTP 会话
sftp.close()


# 关闭连接
ssh.close()



```