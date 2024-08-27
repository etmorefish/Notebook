# 第五章：文件操作

在编程中，文件操作是一个非常常见的任务。Python 提供了强大的工具来读取、写入和管理文件。在本章中，我们将学习如何使用 Python 进行文件操作。

## 1. 文件的读取与写入

### 1.1 文件读取

要读取文件，Python 提供了 `open()` 函数。使用该函数可以打开文件，然后通过文件对象读取文件内容。常见的读取方法包括 `read()`、`readline()` 和 `readlines()`。

**示例：**

```python
# 读取整个文件内容
file = open('example.txt', 'r')  # 'r' 表示只读模式
content = file.read()
print(content)
file.close()  # 关闭文件
```

- `read()`：一次性读取整个文件内容。
- `readline()`：每次读取一行内容。
- `readlines()`：将文件中的每一行作为列表元素，返回一个列表。

### 1.2 文件写入

同样，使用 `open()` 函数并指定写入模式 `'w'` 或 `'a'` 可以将内容写入文件中。

**示例：**

```python
# 写入到文件
file = open('example.txt', 'w')  # 'w' 表示写入模式，覆盖原有内容
file.write('Hello, Python!')
file.close()
```

### 1.3 文件读写模式

以下是常用的文件读写模式，以表格形式展示：

| 模式     | 说明                                             |
| ------ | ---------------------------------------------- |
| `'r'`  | 以只读模式打开文件（文件必须存在）。                             |
| `'w'`  | 以写入模式打开文件，若文件已存在则覆盖，若文件不存在则创建新文件。              |
| `'a'`  | 以追加模式打开文件，写入的数据会添加到文件末尾。若文件不存在则创建新文件。          |
| `'r+'` | 以读写模式打开文件（文件必须存在）。                             |
| `'w+'` | 以读写模式打开文件，若文件已存在则覆盖，若文件不存在则创建新文件。              |
| `'a+'` | 以读写模式打开文件，写入的数据会添加到文件末尾。若文件不存在则创建新文件。          |
| `'b'`  | 以二进制模式打开文件，与其他模式配合使用（如 `'rb'` 表示以二进制只读模式打开文件）。 |

## 2. 文件的路径管理与错误处理

### 2.1 文件路径管理

在处理文件时，正确管理文件路径非常重要。可以使用相对路径或绝对路径来指定文件的位置。Python 的 `os` 和 `pathlib` 模块提供了丰富的工具来管理文件路径。

**示例：**

```python
import os

# 获取当前工作目录
current_dir = os.getcwd()
print(current_dir)

# 连接路径
file_path = os.path.join(current_dir, 'example.txt')
print(file_path)
```

### 2.2 错误处理

在进行文件操作时，可能会遇到一些常见的错误，例如文件不存在、路径错误、没有读写权限等。为了让程序更健壮，建议在文件操作时进行错误处理。Python 提供了 `try-except` 结构，用于捕获和处理可能发生的异常。

以下是常见的文件操作错误及其处理方法：

- `FileNotFoundError`：文件未找到，通常在尝试读取一个不存在的文件时出现。
- `PermissionError`：权限错误，通常在尝试打开一个没有读写权限的文件时出现。
- `IsADirectoryError`：尝试打开一个目录而非文件时出现。
- `IOError`：输入输出错误，通常是文件读写失败时出现的通用错误。

**示例：**

```python
try:
    file = open('nonexistent.txt', 'r')
except FileNotFoundError:
    print("文件未找到，请检查文件路径！")
except PermissionError:
    print("权限不足，无法打开文件！")
except IsADirectoryError:
    print("错误：尝试打开的是一个目录，而不是文件！")
except IOError as e:
    print(f"发生了IO错误：{e}")
finally:
    print("文件操作结束。")
```

在上述代码中：

- `try` 块用于编写可能抛出异常的代码。
- `except` 块用于捕获特定类型的异常，并根据异常类型执行相应的处理逻辑。
- `finally` 块中的代码无论是否发生异常都会执行，通常用于清理操作，如关闭文件资源。

通过合理的错误处理，可以确保程序在出现问题时不会崩溃，并且能够提供有意义的错误信息给用户。

## 3. 使用 `with` 管理文件资源

使用 `with` 语句可以简化文件操作，并确保在文件操作完成后自动关闭文件资源。这样可以避免忘记关闭文件导致的资源泄漏。

**示例：**

```python
# 使用 with 语句读取文件
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)
# 文件在 with 语句块结束时会自动关闭

# 使用 with 语句写入文件
with open('example.txt', 'w') as file:
    file.write('This is managed by with!')
# 文件在 with 语句块结束时也会自动关闭
```

### 总结

本章介绍了如何在 Python 中进行文件操作，包括文件的读取与写入、路径管理与错误处理、以及使用 `with` 语句管理文件资源。通过这些知识，学习者可以在实际编程中有效处理文件，并确保程序的健壮性。