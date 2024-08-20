# 第2章：控制流

控制流决定了代码的执行路径，Python提供了多种控制流结构来帮助我们实现不同的逻辑。我们将在本章中学习条件语句、循环语句、模式匹配以及常见的循环控制语句。

## 条件语句

条件语句用于根据条件的真假决定程序的执行路径。在Python中，条件语句主要使用 `if`、`elif` 和 `else` 关键字来实现。

### 基本语法

```python
if 条件判断1:
    # 当条件判断1为True时执行的代码块
    执行语句1
elif 条件判断2:
    # 当条件判断1为False且条件判断2为True时执行的代码块
    执行语句2
else:
    # 当条件判断1和条件判断2都为False时执行的代码块
    执行语句3
```

注意不要少写了冒号`:`和 语句块的缩进。

### 示例

```python
temperature = 30

if temperature > 35:
    print("It's too hot!")
elif temperature > 25:
    print("It's warm.")
else:
    print("It's cool.")
```

在上述示例中，程序根据 `temperature` 的值选择不同的输出。

## 循环语句

循环语句用于重复执行一段代码，直到满足某个条件。在Python中，常用的循环语句有 `for` 和 `while`。

### `for` 循环

`for` 循环用于遍历序列（如列表、字符串或范围）。基本语法如下：

```python
for 变量 in 序列:
    # 执行的代码块
    执行语句
```

#### 示例

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(fruit)
```

在上述示例中，`for` 循环遍历了 `fruits` 列表中的每一个元素，并打印出来。

### `while` 循环

`while` 循环在条件为真时重复执行代码块。基本语法如下：

```python
while condition:
    # 执行的代码块
    statement
```

#### 示例

```python
count = 0

while count < 5:
    print(count)
    count += 1
```

在上述示例中，`while` 循环将 `count` 从0开始逐步增加，直到 `count` 不小于5为止。

## 模式匹配

模式匹配是Python 3.10引入的一项新特性，它可以更方便地对复杂数据结构进行解构和匹配。使用 `match` 语句来实现模式匹配。

### 基本语法

```python
match value:
    case pattern1:
        # 当value与pattern1匹配时执行的代码块
        statement1
    case pattern2:
        # 当value与pattern2匹配时执行的代码块
        statement2
    case _:
        # 如果没有匹配到任何模式时执行的代码块
        statement3
```

### 示例

```python
def describe(value):
    match value:
        case 1:
            return "One"
        case 2:
            return "Two"
        case 3:
            return "Three"
        case _:
            return "Not 1, 2, or 3"

print(describe(2))  # 输出: Two
print(describe(4))  # 输出: Not 1, 2, or 3
```

在上述示例中，`match` 语句用来根据 `value` 的不同值返回不同的描述。

## 常见的循环控制

在循环中，我们可以使用 `break`、`continue` 和 `pass` 控制循环的执行流程。

### `break` 语句

`break` 语句用于终止整个循环，跳出循环体。

#### 示例

```python
for number in range(10):
    if number == 5:
        break
    print(number)
```

在上述示例中，当 `number` 等于5时，`break` 语句终止了 `for` 循环，循环停止执行。

### `continue` 语句

`continue` 语句用于跳过当前循环的剩余部分，直接进入下一次循环。

#### 示例

```python
for number in range(10):
    if number % 2 == 0:
        continue
    print(number)
```

在上述示例中，`continue` 语句跳过了所有偶数，只打印奇数。

### `pass` 语句

`pass` 语句是一个占位符，用于表示没有实现的代码块。它不会执行任何操作。

#### 示例

```python
for number in range(10):
    if number % 2 == 0:
        pass  # 暂时不执行任何操作
    else:
        print(number)
```

在上述示例中，`pass` 语句在偶数条件下占位，不执行任何操作，而 `print` 语句则打印所有奇数。

## 小结

本章介绍了Python中的主要控制流结构，包括条件语句、循环语句、模式匹配以及常见的循环控制语句。掌握这些控制流结构能帮助你编写功能复杂、逻辑清晰的程序。

## 课后练习

### 练习1：条件语句

1. **编写一个Python脚本**，根据用户输入的分数，输出等级评价（例如：A、B、C、D、F）。使用 `if`、`elif` 和 `else` 语句。
   
   ```python
   score = int(input("Enter your score: "))
   if score >= 90:
       print("Grade: A")
   elif score >= 80:
       print("Grade: B")
   elif score >= 70:
       print("Grade: C")
   elif score >= 60:
       print("Grade: D")
   else:
       print("Grade: F")
   ```

### 练习2：循环语句

1. **编写一个Python脚本**，使用 `for` 循环打印从1到10的平方值。
   
   ```python
   for i in range(1, 11):
       print(f"{i} squared is {i**2}")
   ```

2. **编写一个Python脚本**，使用 `while` 循环计算从1到100的总和，并输出结果。
   
   ```python
   total = 0
   number = 1
   while number <= 100:
       total += number
       number += 1
   print("Total:", total)
   ```

### 练习3：模式匹配

1. **编写一个Python函数**，使用 `match` 语句根据传入的季节（如 `"spring"`, `"summer"`, `"fall"`, `"winter"`) 返回对应的月份列表。
   
   ```python
   def get_months(season):
       match season:
           case "spring":
               return ["March", "April", "May"]
           case "summer":
               return ["June", "July", "August"]
           case "fall":
               return ["September", "October", "November"]
           case "winter":
               return ["December", "January", "February"]
           case _:
               return "Invalid season"
   
   print(get_months("summer"))  # 输出: ['June', 'July', 'August']
   ```

### 练习4：循环控制

> 内置函数 [`range()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#range "range") 用于生成等差数列

1. **编写一个Python脚本**，使用 `break` 语句找到并打印第一个能被7整除的数字，范围从1到50。
   
   ```python
   for number in range(1, 51):
       if number % 7 == 0:
           print("First number divisible by 7:", number)
           break
   ```

2. **编写一个Python脚本**，使用 `continue` 语句打印1到20之间的所有偶数。
   
   ```python
   for number in range(1, 21):
       if number % 2 != 0:
           continue
       print(number)
   ```
   
   
