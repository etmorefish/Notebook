# 第八章：常用Python标准库

## 第一节： Python 代码规范与 Python 之禅

**学习目标：**
- 了解 Python 的代码规范（PEP 8）
- 理解 Python 之禅（PEP 20）中蕴含的编程哲学

---

#### 1. PEP 8 代码规范

- **PEP 8**：Python Enhancement Proposal（Python 改进提案）中的第 8 号提案，是 Python 官方推荐的代码风格指南。
- **目的**：统一代码风格，提升可读性，使代码更具一致性。

##### 主要规范内容

1. **缩进**
   - 每行缩进使用 **4 个空格**（不要使用 Tab 键）。
   
2. **最大行宽**
   - 每行代码的长度不要超过 **79 个字符**，超过时建议换行。
   
3. **空行**
   - 函数、类定义之间使用两个空行，方法定义之间使用一个空行。
   
4. **命名规范**
   - 变量和函数：使用**小写字母和下划线**（如 `my_variable`）。
   - 常量：使用**大写字母和下划线**（如 `MAX_VALUE`）。
   - 类名：使用**首字母大写的驼峰命名法**（如 `MyClass`）。

5. **注释**
   - 注释需准确、简洁，且应在代码之上或右侧合理添加。
   
6. **空格的使用**
   - 避免在括号、方括号和大括号内添加空格。
   - 运算符前后建议保留空格，如 `a = b + c`。

**示例代码：**

```python
class MyClass:
    def __init__(self, name):
        self.name = name

    def say_hello(self):
        print("Hello, " + self.name)
```

##### 常用检查工具

- 使用工具如 `flake8`、`pylint`、 `ruff` 或 IDE 插件来检查代码是否符合 PEP 8 规范。

---

#### 2. Python 之禅（PEP 20）

- **Python 之禅**：由 Tim Peters 编写的一组指导 Python 编程哲学的 19 条准则。
- **查看方式**：在 Python 解释器中输入 `import this`。

##### 核心理念

- **简洁胜于复杂**：代码应保持简单和清晰。
- **清晰胜于晦涩**：编写易于理解的代码。
- **可读性很重要**：代码不仅是写给机器的，更是写给人看的。
- **拒绝多余**：避免不必要的功能和复杂性。

**Python 之禅示例**：

```python
import this
```

这段代码将输出 Python 之禅的全部内容，为开发者提供编程的价值观和设计哲学。

---

## 第二节： Python 标准库的定义

**学习目标：**
- 理解 Python 标准库的定义
- 掌握使用标准库的基本方式

---

#### 1. 什么是 Python 标准库？

- **定义**：Python 标准库是一组随 Python 一起发布的模块集合，涵盖了文件操作、时间处理、数据序列化等常用功能。
- **特点**：标准库模块稳定且经过优化，无需安装第三方包，直接使用。

#### 2. 使用标准库的好处

- **快速开发**：标准库提供了丰富的功能模块，减少开发时间。
- **可靠性高**：标准库模块经过广泛使用和测试，非常可靠。
- **跨平台支持**：Python 标准库大部分模块是跨平台的，支持在不同操作系统上运行。

#### 3. 标准库模块的分类

Python 标准库涵盖了广泛的领域，主要分为以下几类：

1. **系统相关**：`os`、`sys`、`platform`
2. **文件与目录操作**：`os`、`shutil`、`glob`
3. **数据处理**：`json`、`csv`、`sqlite3`
4. **网络编程**：`socket`、`http`、`urllib`
5. **时间与日期**：`datetime`、`time`
6. **数学与统计**：`math`、`statistics`、`random`

#### 4. 如何导入和使用标准库模块

- 标准库模块通过 `import` 语句导入后即可使用。例如：

```python
import os
print(os.getcwd())  # 输出当前工作目录
```

- **多模块导入**：可以一次导入多个模块，也可以导入模块中的部分功能。

**示例：**

```python
from math import sqrt, pi
print("平方根:", sqrt(16))
print("圆周率:", pi)
```

---


## 第三节：`sys` 模块

### 1. 概述
`sys` 模块是 Python 的标准库之一，用于与 Python 解释器进行交互。它提供了访问解释器的一些功能和变量，能够帮助程序员获取 Python 环境的配置信息，控制程序的退出等。理解 `sys` 模块可以让学生更清楚 Python 运行时的环境和参数。

---

### 2. 常见功能

#### **2.1 获取 Python 版本**

`sys.version` 属性可以返回 Python 的版本信息，用于检查当前环境的版本是否符合程序运行的要求。

```python
import sys

print("当前 Python 版本:", sys.version)
```

**输出示例**：
```
当前 Python 版本: 3.12.6 (v3.12.6:a4a2d2b0d85, Sep  6 2024, 16:08:03) [Clang 13.0.0 (clang-1300.0.29.30)]
```

---

#### **2.2 命令行参数**

`sys.argv` 列表存储了传递给 Python 脚本的命令行参数。第一个元素是脚本名称，后续元素是脚本运行时传递的参数。

```python
import sys

# sys.argv[0] 是脚本名，后面的是参数
print("脚本名:", sys.argv[0])
print("传递的参数:", sys.argv[1:])
```

**示例用法**：
运行以下命令：
```bash
python script.py arg1 arg2 arg3
```

输出：
```
脚本名: script.py
传递的参数: ['arg1', 'arg2', 'arg3']
```

---

#### **2.3 退出程序**

`sys.exit()` 用于退出程序并返回一个状态码。`0` 表示正常退出，非 `0` 表示异常退出。

```python
import sys

print("程序即将退出")
sys.exit(0)
print("这行不会被打印")  # 因为程序已经退出
```

---

#### **2.4 标准输入和输出**

- **sys.stdin**: 标准输入流，可以用来读取用户输入。
- **sys.stdout**: 标准输出流，可以用来向屏幕打印信息。
- **sys.stderr**: 标准错误流，用于输出错误信息。

**示例**：
```python
import sys

# 输出信息
sys.stdout.write("Hello, Standard Output!\n")

# 输出错误信息
sys.stderr.write("Error: Something went wrong!\n")
```

---

#### **2.5 修改递归深度**

默认情况下，Python 的递归深度限制为 1000。如果需要更深的递归，可以通过 `sys.setrecursionlimit()` 修改此限制。

```python
import sys

# 查看当前递归深度限制
print("当前递归深度限制:", sys.getrecursionlimit())

# 修改递归深度限制
sys.setrecursionlimit(2000)
print("修改后的递归深度限制:", sys.getrecursionlimit())
```

**注意**：增大递归深度限制可能导致内存不足或程序崩溃，使用时需谨慎。

---

#### **2.6 访问系统路径**

`sys.path` 是一个列表，包含 Python 查找模块的路径。可以动态修改 `sys.path`，以便导入特定位置的模块。

```python
import sys

# 查看当前系统路径
print("系统路径:", sys.path)

# 动态添加一个新路径
sys.path.append('/path/to/my/module')
print("修改后的系统路径:", sys.path)
```

通过 `sys` 模块的学习，能够更灵活地控制程序的运行环境、参数输入与退出逻辑。



## 第四节：os 模块

### 1. 概述
`os` 模块是 Python 的内置标准库之一，提供了与操作系统交互的功能。通过 `os` 模块，开发者可以执行文件和目录的操作、获取系统信息、处理环境变量等。`os` 模块的主要优点是它的跨平台特性，能够在不同的操作系统上运行。

### 2. 常用功能

#### 2.1 文件和目录操作

- **获取当前工作目录**：
  ```python
  import os

  current_directory = os.getcwd()
  print("当前工作目录:", current_directory)
  ```

- **改变当前工作目录**：
  ```python
  os.chdir('/path/to/directory')  # 替换为目标路径
  print("新的工作目录:", os.getcwd())
  ```

- **列出目录中的文件**：
  ```python
  files = os.listdir('.')
  print("当前目录的文件和目录:", files)
  ```

- **创建新目录**：
  ```python
  os.mkdir('new_directory')
  print("新目录创建成功")
  ```

- **删除目录**：
  ```python
  os.rmdir('new_directory')
  print("目录删除成功")
  ```

#### 2.2 文件操作

- **检查文件是否存在**：
  ```python
  file_exists = os.path.isfile('example.txt')
  print("文件是否存在:", file_exists)
  ```

- **获取文件的绝对路径**：
  ```python
  absolute_path = os.path.abspath('example.txt')
  print("文件的绝对路径:", absolute_path)
  ```

- **重命名文件**：
  ```python
  os.rename('old_name.txt', 'new_name.txt')
  print("文件重命名成功")
  ```

- **删除文件**：
  ```python
  os.remove('new_name.txt')
  print("文件删除成功")
  ```

#### 2.3 环境变量

- **获取环境变量**：
  ```python
  home_directory = os.getenv('HOME')  # 在 Windows 上可以使用 'USERPROFILE'
  print("用户主目录:", home_directory)
  ```

- **设置环境变量**：
  ```python
  os.environ['MY_VARIABLE'] = 'value'
  print("环境变量设置成功")
  ```

### 3. 实际案例

以下是一个使用 `os` 模块的简单示例，演示如何列出当前工作目录下的所有文件和子目录，并创建一个新目录：

```python
import os

# 获取当前工作目录
print("当前工作目录:", os.getcwd())

# 列出当前目录的文件和目录
print("当前目录的文件和目录:")
for item in os.listdir('.'):
    print(item)

# 创建新目录
new_directory = 'example_dir'
os.mkdir(new_directory)
print(f"目录 '{new_directory}' 创建成功")
```

### 4. 练习题

1. 编写一个程序，要求用户输入一个文件名，检查该文件是否存在于当前目录。
2. 编写一个程序，列出给定目录下的所有文件，并统计文件的数量。
3. 创建一个新目录，并在其中创建一个新文件，写入一些文本，然后读取并打印该文件的内容。



## 第五节：argparse 模块

### 1. 概述
`argparse` 模块是 Python 的标准库之一，专门用于处理命令行参数和选项。它提供了一个简单易用的接口，可以解析传递给 Python 脚本的命令行参数，并自动生成帮助信息。通过使用 `argparse`，开发者可以构建功能强大的命令行工具。

### 2. 基本用法

#### 2.1 创建解析器

首先，导入 `argparse` 模块并创建一个解析器对象：

```python
import argparse

parser = argparse.ArgumentParser(description='这是一个示例程序。')
```

#### 2.2 添加参数

可以通过 `add_argument()` 方法向解析器添加所需的参数。例如：

```python
# 添加一个位置参数
parser.add_argument('name', type=str, help='输入你的名字')

# 添加一个可选参数
parser.add_argument('-a', '--age', type=int, help='输入你的年龄', required=False)

# 添加一个布尔标志
parser.add_argument('-v', '--verbose', action='store_true', help='增加输出的详细信息')
```

#### 2.3 解析参数

通过 `parse_args()` 方法解析命令行参数：

```python
args = parser.parse_args()
```

#### 2.4 使用参数

可以通过解析后的 `args` 对象访问参数：

```python
print(f"名字: {args.name}")
if args.age:
    print(f"年龄: {args.age}")
if args.verbose:
    print("详细信息已启用")
```

### 3. 实际案例

以下是一个完整的示例，演示如何使用 `argparse` 创建一个简单的命令行程序：

```python
import argparse

# 创建解析器
parser = argparse.ArgumentParser(description='欢迎使用命令行工具！')

# 添加参数
parser.add_argument('name', type=str, help='输入你的名字')
parser.add_argument('-a', '--age', type=int, help='输入你的年龄', required=False)
parser.add_argument('-v', '--verbose', action='store_true', help='增加输出的详细信息')

# 解析参数
args = parser.parse_args()

# 使用参数
print(f"名字: {args.name}")
if args.age:
    print(f"年龄: {args.age}")
if args.verbose:
    print("详细信息已启用")
```

### 4. 使用示例

假设将上述代码保存为 `greet.py`，可以通过命令行运行：

```bash
python greet.py Alice -a 30 -v
```

**输出**：
```
名字: Alice
年龄: 30
详细信息已启用
```

### 5. 练习题

1. 编写一个命令行程序，接受一个文件名参数，读取并打印该文件的内容。
2. 扩展上述程序，添加一个可选参数，允许用户指定是否以详细模式输出（例如，输出文件的行数等信息）。
3. 创建一个计算器程序，接受两个数字和一个操作符（如加、减、乘、除）作为命令行参数，输出计算结果。



## 第六节：time 模块

### 1. 概述
`time` 模块是 Python 的内置标准库之一，主要用于处理时间相关的功能。它提供了获取当前时间、延迟执行、时间格式化等功能。`time` 模块的函数通常返回的是系统时间，单位为秒或以秒为基础的其他时间单位。

### 2. 常用功能

#### 2.1 获取当前时间

- **当前时间戳**：返回从1970年1月1日（UTC）到现在的秒数。
  ```python
  import time

  current_timestamp = time.time()
  print("当前时间戳:", current_timestamp)
  ```

- **当前本地时间**：将时间戳转换为本地时间。
  ```python
  local_time = time.localtime(current_timestamp)
  print("当前本地时间:", time.strftime('%Y-%m-%d %H:%M:%S', local_time))
  ```

#### 2.2 时间格式化

- **格式化时间字符串**：
  ```python
  formatted_time = time.strftime('%Y-%m-%d %H:%M:%S', local_time)
  print("格式化时间:", formatted_time)
  ```

- **将字符串转换为时间元组**：
  ```python
  time_string = '2024-10-26 12:00:00'
  time_tuple = time.strptime(time_string, '%Y-%m-%d %H:%M:%S')
  print("时间元组:", time_tuple)
  ```

#### 2.3 暂停程序

- **延迟执行**：使用 `sleep()` 函数让程序暂停指定的秒数。
  ```python
  print("程序暂停 3 秒...")
  time.sleep(3)
  print("继续执行")
  ```

#### 2.4 计算时间差

- **计算程序执行时间**：
  ```python
  start_time = time.time()
  
  # 执行一些操作
  for _ in range(1000000):
      pass  # 模拟操作
  
  end_time = time.time()
  elapsed_time = end_time - start_time
  print(f"程序执行时间: {elapsed_time:.6f} 秒")
  ```

### 3. 实际案例

以下是一个使用 `time` 模块的简单示例，演示如何获取当前时间、格式化时间和计算程序执行时间：

```python
import time

# 获取当前时间戳
current_timestamp = time.time()
print("当前时间戳:", current_timestamp)

# 获取本地时间
local_time = time.localtime(current_timestamp)
formatted_time = time.strftime('%Y-%m-%d %H:%M:%S', local_time)
print("当前本地时间:", formatted_time)

# 暂停程序
print("程序将在 2 秒后继续执行...")
time.sleep(2)

# 计算程序执行时间
start_time = time.time()
# 执行一些操作
time.sleep(1)  # 模拟延迟
end_time = time.time()
print(f"程序执行时间: {end_time - start_time:.6f} 秒")
```

### 4. 练习题

1. 编写一个程序，获取当前的时间戳并将其转换为格式化的日期和时间字符串。
2. 创建一个程序，计算从输入开始到结束的执行时间（例如，输入一个字符串，程序计时直到用户输入结束）。
3. 编写一个程序，模拟一个简单的计时器，用户输入秒数，程序在倒计时结束后打印消息。


---

## 第七节：datetime 模块

### 1. 概述
`datetime` 模块是 Python 的标准库之一，提供了处理日期和时间的类。它比 `time` 模块更为强大，支持更复杂的日期和时间操作，包括日期的算术运算、时区支持等。

### 2. 常用功能

#### 2.1 获取当前日期和时间

- **获取当前日期和时间**：
  ```python
  from datetime import datetime

  now = datetime.now()
  print("当前日期和时间:", now)
  ```

- **获取当前日期**：
  ```python
  today = datetime.today()
  print("今天的日期:", today.date())
  ```

#### 2.2 创建日期和时间对象

- **创建日期对象**：
  ```python
  from datetime import date

  specific_date = date(2024, 10, 26)  # 年, 月, 日
  print("指定日期:", specific_date)
  ```

- **创建时间对象**：
  ```python
  from datetime import time

  specific_time = time(14, 30, 0)  # 时, 分, 秒
  print("指定时间:", specific_time)
  ```

- **创建日期时间对象**：
  ```python
  from datetime import datetime

  specific_datetime = datetime(2024, 10, 26, 14, 30, 0)
  print("指定日期和时间:", specific_datetime)
  ```

#### 2.3 日期和时间的格式化

- **格式化日期和时间字符串**：
  ```python
  formatted_datetime = now.strftime('%Y-%m-%d %H:%M:%S')
  print("格式化的当前日期和时间:", formatted_datetime)
  ```

- **将字符串转换为日期时间对象**：
  ```python
  datetime_string = '2024-10-26 14:30:00'
  parsed_datetime = datetime.strptime(datetime_string, '%Y-%m-%d %H:%M:%S')
  print("解析后的日期时间对象:", parsed_datetime)
  ```

#### 2.4 日期和时间的算术运算

- **计算日期间隔**：
  ```python
  from datetime import timedelta

  delta = timedelta(days=10)
  future_date = now + delta
  print("10天后的日期:", future_date)
  ```

- **计算时间差**：
  ```python
  time_diff = future_date - now
  print("时间差（天）:", time_diff.days)
  ```

### 3. 实际案例

以下是一个使用 `datetime` 模块的示例，演示如何获取当前时间、格式化日期、以及计算日期间隔：

```python
from datetime import datetime, timedelta

# 获取当前日期和时间
now = datetime.now()
print("当前日期和时间:", now)

# 格式化日期和时间
formatted_datetime = now.strftime('%Y-%m-%d %H:%M:%S')
print("格式化的当前日期和时间:", formatted_datetime)

# 计算未来日期
delta = timedelta(days=30)
future_date = now + delta
print("30天后的日期:", future_date.strftime('%Y-%m-%d'))

# 计算时间差
time_diff = future_date - now
print("时间差（天）:", time_diff.days)
```

### 4. 练习题

1. 编写一个程序，获取当前日期并输出下一个月的日期。
2. 创建一个程序，要求用户输入一个日期，然后计算并输出距离今天的天数。
3. 编写一个程序，接受两个日期作为输入，计算这两个日期之间的天数差。

---
---

## 第八节：math 模块

### 1. 概述
`math` 模块是 Python 的标准库之一，提供了数学运算的函数和常量。它包含许多数学运算的功能，如三角函数、对数、幂运算等，适用于各种科学和工程计算。

### 2. 常用功能

#### 2.1 数学常量

- **π (圆周率)**：
  ```python
  import math

  pi_value = math.pi
  print("圆周率 π:", pi_value)
  ```

- **自然对数的底 e**：
  ```python
  e_value = math.e
  print("自然对数的底 e:", e_value)
  ```

#### 2.2 基本数学运算

- **取整**：
  ```python
  x = 3.14
  print("向下取整:", math.floor(x))
  print("向上取整:", math.ceil(x))
  ```

- **取绝对值**：
  ```python
  print("绝对值:", math.fabs(-5))
  ```

#### 2.3 幂和对数运算

- **幂运算**：
  ```python
  print("2 的 3 次方:", math.pow(2, 3))
  print("平方根:", math.sqrt(16))
  ```

- **对数运算**：
  ```python
  print("以 10 为底的对数:", math.log10(100))
  print("以 e 为底的自然对数:", math.log(math.e))
  ```

#### 2.4 三角函数

- **常用三角函数**：
  ```python
  angle = math.pi / 4  # 45 度
  print("sin(45°):", math.sin(angle))
  print("cos(45°):", math.cos(angle))
  print("tan(45°):", math.tan(angle))
  ```

- **反三角函数**：
  ```python
  print("arcsin(0.7071):", math.asin(0.7071))
  print("arccos(0.7071):", math.acos(0.7071))
  ```

### 3. 实际案例

以下是一个使用 `math` 模块的示例，演示如何进行基本的数学运算和使用三角函数：

```python
import math

# 获取常数
print("圆周率 π:", math.pi)
print("自然对数的底 e:", math.e)

# 幂运算
base = 2
exponent = 3
print(f"{base} 的 {exponent} 次方:", math.pow(base, exponent))

# 三角函数
angle_deg = 45
angle_rad = math.radians(angle_deg)  # 角度转弧度
print(f"sin({angle_deg}°):", math.sin(angle_rad))
print(f"cos({angle_deg}°):", math.cos(angle_rad))
print(f"tan({angle_deg}°):", math.tan(angle_rad))

# 计算平方根
number = 25
print(f"{number} 的平方根:", math.sqrt(number))
```

### 4. 练习题

1. 编写一个程序，计算并输出一个数的平方、立方和平方根。
2. 创建一个程序，输入一个角度（度），计算并输出其正弦、余弦和正切值。
3. 编写一个程序，计算给定数值的自然对数和以 10 为底的对数。

---

## 第九节：random 模块

### 1. 概述
`random` 模块是 Python 的标准库之一，提供了生成伪随机数的功能。它可以用于随机选择、打乱顺序、生成随机数等操作，非常适合需要随机性的应用程序，如游戏、模拟和抽样。

### 2. 常用功能

#### 2.1 生成随机数

- **生成随机浮点数**：
  - **范围在 [0.0, 1.0)**：
    ```python
    import random

    random_float = random.random()
    print("随机浮点数 (0.0 到 1.0):", random_float)
    ```

  - **生成指定范围的浮点数**：
    ```python
    random_uniform = random.uniform(1.0, 10.0)
    print("随机浮点数 (1.0 到 10.0):", random_uniform)
    ```

  - **生成指定范围的整数**：
    ```python
    random_int = random.randint(1, 100)  # 包含 1 和 100
    print("随机整数 (1 到 100):", random_int)
    ```

#### 2.2 随机选择

- **从序列中随机选择一个元素**：
  ```python
  choices = ['苹果', '香蕉', '橘子', '葡萄']
  random_choice = random.choice(choices)
  print("随机选择的水果:", random_choice)
  ```

- **从序列中随机选择多个元素**：
  ```python
  random_sample = random.sample(choices, 2)  # 不重复选择
  print("随机选择的水果（不重复）:", random_sample)
  ```

- **随机打乱列表的顺序**：
  ```python
  random.shuffle(choices)
  print("打乱后的水果列表:", choices)
  ```

#### 2.3 生成随机样本

- **生成指定数量的随机数**：
  ```python
  random_numbers = [random.randint(1, 100) for _ in range(5)]
  print("生成的随机整数列表:", random_numbers)
  ```

### 3. 实际案例

以下是一个使用 `random` 模块的示例，演示如何生成随机数、选择随机元素和打乱列表：

```python
import random

# 生成随机浮点数
print("随机浮点数 (0.0 到 1.0):", random.random())
print("随机浮点数 (1.0 到 10.0):", random.uniform(1.0, 10.0))

# 生成随机整数
print("随机整数 (1 到 100):", random.randint(1, 100))

# 随机选择水果
fruits = ['苹果', '香蕉', '橘子', '葡萄']
print("随机选择的水果:", random.choice(fruits))

# 打乱水果列表
random.shuffle(fruits)
print("打乱后的水果列表:", fruits)

# 生成随机数样本
random_numbers = [random.randint(1, 100) for _ in range(5)]
print("生成的随机整数列表:", random_numbers)
```

### 4. 练习题

1. 编写一个程序，随机生成 10 个 1 到 100 之间的整数，并输出它们的平均值。
2. 创建一个程序，从一个包含多个颜色的列表中随机选择 3 种颜色，并输出选择的颜色。
3. 编写一个程序，生成一个包含 10 个随机浮点数的列表，然后找到列表中的最大值和最小值。

---

## 第十节：re 模块

### 1. 概述
`re` 模块是 Python 的标准库之一，用于处理正则表达式。正则表达式是一种强大的文本处理工具，可以用于查找、替换和解析字符串数据。通过使用正则表达式，程序员可以轻松地执行复杂的文本匹配和操作。

### 2. 常用功能

#### 2.1 基本匹配

- **查找字符串**：
  ```python
  import re

  text = "今天的天气很好，明天会更好。"
  match = re.search(r"天气", text)  # 查找“天气”
  if match:
      print("找到:", match.group())
  ```

- **匹配字符串**：
  ```python
  if re.match(r"今天", text):  # 判断字符串是否以“今天”开头
      print("字符串以 '今天' 开头")
  ```

#### 2.2 查找所有匹配项

- **查找所有匹配项**：
  ```python
  results = re.findall(r"[一二三四五六七八九十]", "一二三四五六七八九十的数字")
  print("找到的数字:", results)
  ```

#### 2.3 替换字符串

- **使用 sub() 方法替换匹配项**：
  ```python
  new_text = re.sub(r"天气", "气候", text)
  print("替换后的字符串:", new_text)
  ```

#### 2.4 正则表达式元字符

- **常用元字符**：
  - `.`：匹配除换行符以外的任意字符。
  - `^`：匹配字符串的开头。
  - `$`：匹配字符串的结尾。
  - `*`：匹配前一个字符零次或多次。
  - `+`：匹配前一个字符一次或多次。
  - `?`：匹配前一个字符零次或一次。
  - `[]`：匹配括号内的任意字符。
  - `|`：逻辑或。

### 3. 实际案例

以下是一个使用 `re` 模块的示例，演示如何使用正则表达式查找和替换文本：

```python
import re

text = "我的电话号码是 123-456-7890，欢迎联系我！"

# 查找电话号码
phone_match = re.search(r"\d{3}-\d{3}-\d{4}", text)
if phone_match:
    print("找到的电话号码:", phone_match.group())

# 替换电话号码
new_text = re.sub(r"\d{3}-\d{3}-\d{4}", "XXX-XXX-XXXX", text)
print("替换后的文本:", new_text)
```

### 4. 练习题

1. 编写一个程序，从一段文本中提取所有的邮箱地址。
2. 创建一个程序，判断输入的字符串是否是有效的手机号（例如，11位数字）。
3. 编写一个程序，统计一段文本中每个单词出现的次数，并输出频率最高的前 5 个单词。

---

## 第十一节：csv 模块

### 1. 概述
`csv` 模块是 Python 的标准库之一，用于处理 CSV（Comma-Separated Values，逗号分隔值）格式的数据。CSV 是一种简单的文件格式，用于存储表格数据，广泛用于数据交换和存储。`csv` 模块提供了读取和写入 CSV 文件的便捷方法。

### 2. 常用功能

#### 2.1 读取 CSV 文件

- **使用 `csv.reader` 读取 CSV 文件**：
  ```python
  import csv

  with open('data.csv', newline='', encoding='utf-8') as csvfile:
      reader = csv.reader(csvfile)
      for row in reader:
          print(row)  # 每一行数据以列表形式输出
  ```

- **读取 CSV 文件并指定分隔符**：
  ```python
  with open('data.tsv', newline='', encoding='utf-8') as csvfile:  # 使用制表符作为分隔符
      reader = csv.reader(csvfile, delimiter='\t')
      for row in reader:
          print(row)
  ```

#### 2.2 写入 CSV 文件

- **使用 `csv.writer` 写入 CSV 文件**：
  ```python
  with open('output.csv', mode='w', newline='', encoding='utf-8') as csvfile:
      writer = csv.writer(csvfile)
      writer.writerow(['姓名', '年龄', '城市'])  # 写入表头
      writer.writerow(['张三', 28, '北京'])
      writer.writerow(['李四', 22, '上海'])
  ```

- **写入多行数据**：
  ```python
  data = [
      ['姓名', '年龄', '城市'],
      ['王五', 30, '广州'],
      ['赵六', 25, '深圳']
  ]

  with open('output.csv', mode='w', newline='', encoding='utf-8') as csvfile:
      writer = csv.writer(csvfile)
      writer.writerows(data)  # 写入多行数据
  ```

#### 2.3 读取 CSV 文件到字典

- **使用 `csv.DictReader` 读取 CSV 文件**：
  ```python
  with open('data.csv', newline='', encoding='utf-8') as csvfile:
      reader = csv.DictReader(csvfile)
      for row in reader:
          print(row['姓名'], row['年龄'])  # 通过列名访问数据
  ```

#### 2.4 写入 CSV 文件为字典

- **使用 `csv.DictWriter` 写入 CSV 文件**：
  ```python
  fieldnames = ['姓名', '年龄', '城市']
  with open('output.csv', mode='w', newline='', encoding='utf-8') as csvfile:
      writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
      writer.writeheader()  # 写入表头
      writer.writerow({'姓名': '小明', '年龄': 18, '城市': '成都'})
      writer.writerow({'姓名': '小红', '年龄': 20, '城市': '杭州'})
  ```

### 3. 实际案例

以下是一个完整的示例，演示如何读取和写入 CSV 文件：

```python
import csv

# 写入 CSV 文件
data = [
    ['姓名', '年龄', '城市'],
    ['小白', 21, '天津'],
    ['小黑', 24, '南京']
]

with open('people.csv', mode='w', newline='', encoding='utf-8') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerows(data)

# 读取 CSV 文件
with open('people.csv', newline='', encoding='utf-8') as csvfile:
    reader = csv.reader(csvfile)
    for row in reader:
        print(row)
```

### 4. 练习题

1. 编写一个程序，读取一个包含学生信息的 CSV 文件，并输出每位学生的姓名和成绩。
2. 创建一个程序，将用户输入的人员信息（姓名、年龄、城市）写入 CSV 文件，直到用户选择停止。
3. 编写一个程序，从一个 CSV 文件中读取数据，统计每个城市的人数，并将结果写入新的 CSV 文件。

---

## 第十二节：logging 模块

### 1. 概述
`logging` 模块是 Python 的标准库之一，提供了一种灵活的框架，用于记录程序运行时的日志信息。通过使用 `logging`，程序员可以轻松地追踪程序执行过程、调试问题、记录错误信息等，帮助提高代码的可维护性和可追踪性。

### 2. 常用功能

#### 2.1 基本用法

- **记录不同级别的日志**：
  ```python
  import logging

  logging.basicConfig(level=logging.DEBUG)  # 设置日志级别为 DEBUG

  logging.debug("这是调试信息")
  logging.info("这是普通信息")
  logging.warning("这是警告信息")
  logging.error("这是错误信息")
  logging.critical("这是严重错误信息")
  ```

#### 2.2 日志格式化

- **自定义日志格式**：
  ```python
  logging.basicConfig(
      level=logging.DEBUG,
      format='%(asctime)s - %(levelname)s - %(message)s'  # 自定义格式
  )

  logging.info("这是一条格式化信息")
  ```

#### 2.3 将日志输出到文件

- **将日志写入文件**：
  ```python
  logging.basicConfig(
      filename='app.log',  # 指定日志文件名
      filemode='a',  # 以追加模式写入
      level=logging.DEBUG,
      format='%(asctime)s - %(levelname)s - %(message)s'
  )

  logging.error("这是写入文件的错误信息")
  ```

#### 2.4 创建自定义日志器

- **创建自定义日志器**：
  ```python
  logger = logging.getLogger('my_logger')  # 创建自定义日志器
  logger.setLevel(logging.DEBUG)

  # 创建控制台处理器
  console_handler = logging.StreamHandler()
  console_handler.setLevel(logging.INFO)

  # 创建文件处理器
  file_handler = logging.FileHandler('my_log.log')
  file_handler.setLevel(logging.DEBUG)

  # 设置格式
  formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
  console_handler.setFormatter(formatter)
  file_handler.setFormatter(formatter)

  # 添加处理器到日志器
  logger.addHandler(console_handler)
  logger.addHandler(file_handler)

  logger.debug("调试信息")
  logger.info("普通信息")
  logger.warning("警告信息")
  ```

### 3. 实际案例

以下是一个使用 `logging` 模块的示例，演示如何在程序中记录日志信息：

```python
import logging

# 配置日志
logging.basicConfig(
    filename='application.log',
    level=logging.DEBUG,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

def divide(a, b):
    try:
        result = a / b
        logging.info(f"成功除法: {a} / {b} = {result}")
        return result
    except ZeroDivisionError:
        logging.error("除数为零错误", exc_info=True)

divide(10, 2)  # 记录成功信息
divide(10, 0)  # 记录错误信息
```

### 4. 练习题

1. 编写一个程序，使用 `logging` 模块记录程序执行的各个阶段，包括开始、完成和错误信息。
2. 创建一个程序，在读取文件时记录每次读取的行数和内容。
3. 修改上面的程序，将日志信息同时输出到控制台和文件。

---

## 第十三节：urllib 模块

### 1. 概述
`urllib` 模块是 Python 的标准库之一，提供了一组用于处理 URL 操作的功能，包括 URL 编码、获取网页内容、处理 HTTP 请求等。`urllib` 是一个非常强大的模块，适用于网络数据的获取和处理。

### 2. 常用功能

#### 2.1 获取网页内容

- **使用 `urllib.request` 获取网页内容**：
  ```python
  import urllib.request

  url = 'http://www.example.com'
  response = urllib.request.urlopen(url)
  html = response.read()  # 读取网页内容
  print(html.decode('utf-8'))  # 输出网页内容，解码为字符串
  ```

#### 2.2 URL 编码和解码

- **URL 编码**：
  ```python
  import urllib.parse

  params = {
      'name': '张三',
      'age': 25,
      'city': '北京'
  }
  encoded_params = urllib.parse.urlencode(params)
  print(encoded_params)  # 输出: name=%E5%BC%A0%E4%B8%89&age=25&city=%E5%8C%97%E4%BA%AC
  ```

- **URL 解码**：
  ```python
  decoded_params = urllib.parse.unquote(encoded_params)
  print(decoded_params)  # 输出: name=张三&age=25&city=北京
  ```

#### 2.3 发送 HTTP 请求

- **使用 `urllib.request` 发送 GET 请求**：
  ```python
  import urllib.request

  response = urllib.request.urlopen('http://httpbin.org/get')
  print(response.read().decode('utf-8'))
  ```

- **使用 `urllib.request` 发送 POST 请求**：
  ```python
  import urllib.request
  import urllib.parse

  data = urllib.parse.urlencode({'name': '张三', 'age': 25}).encode('utf-8')
  response = urllib.request.urlopen('http://httpbin.org/post', data=data)
  print(response.read().decode('utf-8'))
  ```

#### 2.4 处理 HTTP 响应

- **检查响应状态码**：
  ```python
  response = urllib.request.urlopen('http://httpbin.org/status/404')
  print(response.status)  # 输出: 404
  ```

- **获取响应头信息**：
  ```python
  response = urllib.request.urlopen('http://httpbin.org/get')
  print(response.getheaders())  # 输出响应头信息
  ```

### 3. 实际案例

以下是一个完整的示例，演示如何使用 `urllib` 模块获取网页内容并进行简单的处理：

```python
import urllib.request

url = 'http://www.example.com'
try:
    response = urllib.request.urlopen(url)
    html = response.read().decode('utf-8')
    print(f"成功获取网页内容: {html[:100]}...")  # 仅输出前100个字符
except urllib.error.URLError as e:
    print(f"获取网页失败: {e.reason}")
```

### 4. 练习题

1. 编写一个程序，使用 `urllib` 模块获取某个网页的内容，并统计其中的单词数。
2. 创建一个程序，向某个 API 发送 POST 请求，并输出返回的 JSON 数据。
3. 使用 `urllib` 模块编写一个简单的爬虫，获取某个网站的标题（`<title>` 标签内容）。

---

## 第十四节：base64 模块

### 1. 概述
`base64` 模块是 Python 的标准库之一，提供了一种用于将二进制数据编码为 ASCII 字符串的工具。Base64 编码广泛用于数据传输，尤其是在处理图像、音频和其他媒体类型时，可以在文本协议中安全地传递二进制数据。

### 2. 常用功能

#### 2.1 Base64 编码

- **将字节数据编码为 Base64**：
  ```python
  import base64

  # 示例数据
  data = b'Hello, World!'  # 字节字符串
  encoded_data = base64.b64encode(data)  # 编码为 Base64
  print(encoded_data)  # 输出: b'SGVsbG8sIFdvcmxkIQ=='
  ```

#### 2.2 Base64 解码

- **将 Base64 编码的数据解码为字节数据**：
  ```python
  decoded_data = base64.b64decode(encoded_data)  # 解码
  print(decoded_data)  # 输出: b'Hello, World!'
  ```

#### 2.3 处理文件

- **将文件内容进行 Base64 编码**：
  ```python
  with open('example.jpg', 'rb') as image_file:
      encoded_string = base64.b64encode(image_file.read())
      print(encoded_string)
  ```

- **将 Base64 编码的数据写入文件**：
  ```python
  with open('encoded.txt', 'wb') as encoded_file:
      encoded_file.write(encoded_string)
  ```

#### 2.4 URL 安全的 Base64 编码

- **使用 URL 安全的 Base64 编码**：
  ```python
  url_safe_encoded = base64.urlsafe_b64encode(data)
  print(url_safe_encoded)  # 输出: b'SGVsbG8sIFdvcmxkIQ=='

  url_safe_decoded = base64.urlsafe_b64decode(url_safe_encoded)
  print(url_safe_decoded)  # 输出: b'Hello, World!'
  ```

### 3. 实际案例

以下是一个完整的示例，演示如何使用 `base64` 模块对文本数据进行编码和解码：

```python
import base64

# 原始数据
data = b'Hello, World!'

# 编码
encoded_data = base64.b64encode(data)
print(f"编码后的数据: {encoded_data}")

# 解码
decoded_data = base64.b64decode(encoded_data)
print(f"解码后的数据: {decoded_data.decode('utf-8')}")
```

### 4. 练习题

1. 编写一个程序，读取一个文本文件，将其内容进行 Base64 编码，并输出编码后的结果。
2. 创建一个程序，将 Base64 编码的字符串解码为原始内容，并输出解码后的结果。
3. 使用 `base64` 模块处理图片文件，将其内容编码为 Base64，并将编码后的字符串保存到文本文件中。

---

## 第十五节：venv 模块

### 1. 概述
`venv` 是 Python 的标准库模块，用于创建和管理虚拟环境。虚拟环境是一个自包含的目录，其中包含一个 Python 解释器及其相关的库和包，可以与系统级的 Python 安装相隔离。使用虚拟环境，可以为不同的项目使用不同版本的依赖库，避免包之间的冲突。

### 2. 创建虚拟环境

#### 2.1 创建虚拟环境

- **使用 `venv` 创建虚拟环境**：
  ```bash
  # 在项目目录中创建名为 venv 的虚拟环境
  python -m venv venv
  ```

#### 2.2 激活虚拟环境

- **在 Windows 上激活虚拟环境**：
  ```bash
  venv\Scripts\activate
  ```

- **在 macOS 和 Linux 上激活虚拟环境**：
  ```bash
  source venv/bin/activate
  ```

激活后，命令提示符会改变，通常会显示虚拟环境的名称。

### 3. 管理虚拟环境

#### 3.1 安装依赖库

- **在虚拟环境中安装包**：
  ```bash
  pip install <package_name>
  ```

- **例如安装 Flask**：
  ```bash
  pip install Flask
  ```

#### 3.2 查看已安装的包

- **列出已安装的包**：
  ```bash
  pip list
  ```

#### 3.3 生成依赖文件

- **生成 `requirements.txt` 文件**：
  ```bash
  pip freeze > requirements.txt
  ```

这个文件包含了当前虚拟环境中所有已安装包及其版本信息，便于共享和安装。

### 4. 使用依赖文件

#### 4.1 安装依赖

- **从 `requirements.txt` 安装依赖**：
  ```bash
  pip install -r requirements.txt
  ```

### 5. 退出虚拟环境

- **退出虚拟环境**：
  ```bash
  deactivate
  ```

### 6. 实际案例

以下是一个完整的示例，演示如何使用 `venv` 模块创建和管理虚拟环境：

```bash
# 创建虚拟环境
python -m venv myenv

# 激活虚拟环境
# Windows
myenv\Scripts\activate
# macOS/Linux
source myenv/bin/activate

# 在虚拟环境中安装 Flask
pip install Flask

# 生成依赖文件
pip freeze > requirements.txt

# 退出虚拟环境
deactivate
```

### 7. 练习题

1. 创建一个新的虚拟环境，并在其中安装 `requests` 和 `numpy` 两个库。
2. 生成一个包含当前虚拟环境中所有库的 `requirements.txt` 文件。
3. 从一个已有的 `requirements.txt` 文件中安装依赖库，并确认所有库已正确安装。

---