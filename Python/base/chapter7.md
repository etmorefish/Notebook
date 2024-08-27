# 第七章：异常处理

本章将介绍异常处理的基本概念、常见的异常类型以及如何自定义异常。通过学习这些内容，您将能够编写更健壮的代码，有效地处理运行时可能发生的错误。

## 1. 异常的概念与处理方法

### 1.1 什么是异常？

异常（Exception）是指在程序运行过程中出现的错误或意外情况，这些情况可能导致程序无法正常执行。例如，试图打开一个不存在的文件，或是除以零，这些操作都会引发异常。

### 1.2 如何处理异常？

在 Python 中，使用 `try` 和 `except` 语句可以捕获和处理异常，从而避免程序因异常而崩溃。

**示例：**

```python
try:
    # 可能发生异常的代码
    result = 10 / 0
except ZeroDivisionError as e:
    # 处理异常的代码
    print(f"捕获异常：{e}")
```

在这个例子中，`try` 块中的代码会尝试执行，如果出现 `ZeroDivisionError` 异常，会进入 `except` 块进行处理。程序不会崩溃，而是输出捕获的异常信息。

### 1.3 `finally` 语句

`finally` 语句块中的代码无论是否发生异常都会执行，通常用于清理资源（如关闭文件、释放锁等）。

**示例：**

```python
try:
    file = open("example.txt", "r")
    content = file.read()
except FileNotFoundError as e:
    print(f"文件未找到：{e}")
finally:
    if 'file' in locals():
        file.close()  # 确保文件被关闭
```

在这个例子中，无论是否出现 `FileNotFoundError` 异常，`finally` 块中的代码都会执行，确保文件被正确关闭。

### 1.4 `else` 语句

`else` 语句块中的代码只有在没有发生异常时才会执行。

**示例：**

```python
try:
    result = 10 / 2
except ZeroDivisionError:
    print("除数不能为零")
else:
    print(f"计算结果是：{result}")
```

在这个例子中，`else` 块中的代码只有在 `try` 块中没有发生异常时才会执行。

## 2. 常见的异常类型

在 Python 中，有许多内置的异常类型，每种异常类型都对应着特定的错误情况。以下是一些常见的异常类型：

- `IndexError`：当尝试访问列表或元组中不存在的索引时，会引发该异常。
  
  **示例：**
  
  ```python
  my_list = [1, 2, 3]
  print(my_list[5])  # 引发 IndexError
  ```

- `KeyError`：当尝试访问字典中不存在的键时，会引发该异常。
  
  **示例：**
  
  ```python
  my_dict = {"name": "Alice"}
  print(my_dict["age"])  # 引发 KeyError
  ```

- `ValueError`：当传递给函数或操作的参数类型正确但值不合适时，会引发该异常。
  
  **示例：**
  
  ```python
  number = int("abc")  # 引发 ValueError
  ```

- `TypeError`：当在不支持的类型之间进行操作时，会引发该异常。
  
  **示例：**
  
  ```python
  result = "5" + 5  # 引发 TypeError
  ```

- `FileNotFoundError`：当尝试打开一个不存在的文件时，会引发该异常。
  
  **示例：**
  
  ```python
  file = open("non_existent_file.txt", "r")  # 引发 FileNotFoundError
  ```

## 3. 自定义异常

在某些情况下，内置的异常类型可能不足以描述某些特定的错误。此时，您可以通过继承 `Exception` 类来自定义异常。

### 3.1 自定义异常的步骤

- 创建一个新的类，并继承 `Exception` 类。
- 可以添加自定义的初始化方法或其他方法，以实现特定的功能。

**示例：**

```python
class NegativeNumberError(Exception):
    """自定义异常：负数错误"""
    def __init__(self, value):
        self.value = value
        super().__init__(f"不允许的负数: {value}")

# 使用自定义异常
def check_positive(number):
    if number < 0:
        raise NegativeNumberError(number)
    return number

try:
    check_positive(-5)
except NegativeNumberError as e:
    print(e)  # 输出：不允许的负数: -5
```

在这个例子中，`NegativeNumberError` 是一个自定义异常类，用于处理输入的数值为负数的情况。当 `check_positive()` 函数检测到负数时，会引发该自定义异常。

### 总结

本章介绍了异常处理的基本概念、常见的异常类型以及如何自定义异常。通过这些知识，您可以更好地处理程序运行中的意外情况，确保代码的健壮性和可维护性。
