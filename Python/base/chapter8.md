# 第八章：常用Python标准库

Python 提供了丰富的标准库，使得许多常见的编程任务变得简单高效。本章将介绍一些常用的标准库，包括时间处理、正则表达式、数据序列化、虚拟环境管理以及与操作系统交互的工具库。

## 1. 时间处理（datetime）

### 1.1 datetime 模块概述

`datetime` 模块提供了用于操作日期和时间的类和方法。它可以帮助您创建、格式化、比较时间和日期。

### 1.2 获取当前日期和时间

```python
import datetime

# 获取当前日期和时间
current_datetime = datetime.datetime.now()
print("当前日期和时间：", current_datetime)
```

### 1.3 日期的格式化

```python
# 格式化日期
formatted_date = current_datetime.strftime("%Y-%m-%d %H:%M:%S")
print("格式化后的日期：", formatted_date)
```

### 1.4 日期加减

```python
# 日期加减
one_week_later = current_datetime + datetime.timedelta(weeks=1)
print("一周后的日期：", one_week_later)
```

## 2. 正则表达式（re）

### 2.1 正则表达式简介

`re` 模块提供了强大的工具来处理字符串模式匹配。正则表达式是用于定义字符串模式的特殊语法。

### 2.2 基本使用方法

```python
import re

# 匹配邮箱地址
email_pattern = r"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b"
email = "example@example.com"
match = re.match(email_pattern, email)

if match:
    print("有效的邮箱地址")
else:
    print("无效的邮箱地址")
```

### 2.3 字符串替换

```python
# 使用正则表达式替换字符串中的数字
text = "我的电话号码是 123-456-7890"
replaced_text = re.sub(r"\d", "*", text)
print("替换后的文本：", replaced_text)
```

## 3. 序列化（json）

### 3.1 json 模块概述

`json` 模块用于在 Python 和 JSON（JavaScript Object Notation）格式之间进行转换。这对于数据存储和交换非常有用。

### 3.2 序列化和反序列化

```python
import json

# 将 Python 对象转换为 JSON 字符串
data = {"name": "Alice", "age": 25, "city": "Beijing"}
json_data = json.dumps(data)
print("JSON 字符串：", json_data)

# 将 JSON 字符串转换为 Python 对象
python_data = json.loads(json_data)
print("Python 对象：", python_data)
```

## 4. 虚拟环境管理（venv）

### 4.1 venv 概述

`venv` 模块用于创建虚拟环境，以便在项目中管理不同的依赖包版本，避免全局依赖的冲突。

### 4.2 创建虚拟环境

```bash
# 创建虚拟环境
python -m venv myenv

# 激活虚拟环境
# Windows
myenv\Scripts\activate

# macOS/Linux
source myenv/bin/activate
```

### 4.3 安装依赖包

```bash
# 安装依赖包
pip install requests
```

### 4.4 退出虚拟环境

```bash
# 退出虚拟环境
deactivate
```

## 5. 操作系统接口（os）

### 5.1 os 模块概述

`os` 模块提供了与操作系统交互的功能，例如文件和目录操作、环境变量访问等。

### 5.2 文件和目录操作

```python
import os

# 获取当前工作目录
current_directory = os.getcwd()
print("当前工作目录：", current_directory)

# 创建一个新目录
os.mkdir("new_directory")

# 切换目录
os.chdir("new_directory")
print("切换后的目录：", os.getcwd())
```

### 5.3 环境变量

```python
# 获取环境变量
path = os.getenv("PATH")
print("环境变量 PATH：", path)
```

## 6. 随机数生成（random）

### 6.1 random 模块概述

`random` 模块用于生成随机数和随机选择序列中的元素。

### 6.2 生成随机数

```python
import random

# 生成一个 0 到 1 之间的随机浮点数
random_float = random.random()
print("随机浮点数：", random_float)

# 生成一个指定范围内的随机整数
random_int = random.randint(1, 10)
print("随机整数：", random_int)
```

### 6.3 随机选择

```python
# 从列表中随机选择一个元素
choices = ["apple", "banana", "cherry"]
random_choice = random.choice(choices)
print("随机选择：", random_choice)
```

## 总结

通过学习本章内容，您将掌握如何使用 Python 的一些常用标准库来处理常见的编程任务。这些库功能强大且易于使用，将大大提升您的编程效率。
