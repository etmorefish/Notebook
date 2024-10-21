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

---

#### 1. **小节目标**

- 掌握如何在 Python 中进行文件路径的操作与管理。
- 了解常用的文件路径处理模块和方法。
- 能够正确读取和写入不同路径下的文件。

---

#### 2. **基础概念**

- **文件路径**：文件在操作系统中的位置，可以分为**绝对路径**和**相对路径**。
  - **绝对路径**：从根目录开始，表示文件的完整位置（例如：`C:/Users/maoge/Documents/file.txt`）。
  - **相对路径**：相对于当前工作目录的文件位置（例如：`./Documents/file.txt`）。

---

#### 3. **Python中的路径管理模块**

- **os 模块**
  - 提供与操作系统相关的功能，包括路径操作。
- **os.path 模块**
  - 包含处理文件路径的工具，例如路径拼接、获取文件名、目录等。
- **pathlib 模块**
  - 提供了一个更现代化的、面向对象的方式来处理路径。

---

#### 4. **os 模块中的常用方法**

- **获取当前工作目录**
  
  ```python
  import os
  print(os.getcwd())  # 获取当前工作目录
  ```

- **改变当前工作目录**
  
  ```python
  os.chdir('/path/to/directory')  # 改变工作目录
  ```

- **创建目录**
  
  ```python
  os.mkdir('new_folder')  # 创建单个目录
  os.makedirs('parent_folder/child_folder')  # 创建多级目录
  ```

- **检查路径是否存在**
  
  ```python
  os.path.exists('/path/to/file_or_directory')  # 检查路径是否存在
  ```

---

#### 5. **os.path 模块中的常用方法**

- **拼接路径**
  
  ```python
  file_path = os.path.join('folder', 'file.txt')
  print(file_path)  # folder/file.txt
  ```

- **获取文件名和扩展名**
  
  ```python
  file_name = os.path.basename('/path/to/file.txt')
  file_ext = os.path.splitext('/path/to/file.txt')[1]
  print(file_name, file_ext)  # file.txt, .txt
  ```

- **获取目录名**
  
  ```python
  dir_name = os.path.dirname('/path/to/file.txt')
  print(dir_name)  # /path/to
  ```

---

#### 6. **pathlib 模块的使用**

- **pathlib.Path 对象**
  
  ```python
  from pathlib import Path
  path = Path('/path/to/file.txt')
  print(path.name)  # file.txt
  print(path.parent)  # /path/to
  ```

- **路径拼接**
  
  ```python
  new_path = Path('folder') / 'file.txt'
  print(new_path)  # folder/file.txt
  ```

- **创建文件和目录**
  
  ```python
  new_dir = Path('new_folder')
  new_dir.mkdir(parents=True, exist_ok=True)  # 创建目录
  ```

---

#### 7. **跨平台路径处理**

- **os.path 和 pathlib 都能处理不同操作系统下的路径差异**。
  - **Windows** 使用反斜杠 (`\`) 作为路径分隔符。
  - **Linux 和 Mac** 使用正斜杠 (`/`) 作为路径分隔符。

**建议使用 `os.path.join()` 或 `pathlib.Path` 来自动处理不同平台上的路径分隔符。**

---

#### 8. **文件路径管理示例**

- **示例1：读取文件的绝对路径**
  
  ```python
  from pathlib import Path
  
  file_path = Path('/Users/maoge/Documents/example.txt')
  with open(file_path, 'r') as file:
      content = file.read()
      print(content)
  ```

- **示例2：根据相对路径创建并写入文件**
  
  ```python
  from pathlib import Path
  
  file_path = Path('output_folder') / 'output.txt'
  file_path.parent.mkdir(parents=True, exist_ok=True)
  
  with open(file_path, 'w') as file:
      file.write("This is a test file.")
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

本章介绍了如何在 Python 中进行文件操作，包括文件的读取与写入、路径管理与错误处理、以及使用 `with` 语句管理文件资源。

- **os.path** 和 **pathlib** 是管理文件路径的两大主要模块，合理使用它们可以极大简化跨平台的文件操作。
- 在实际项目中，推荐使用 `pathlib` 进行路径管理，因其代码简洁且具备更强的灵活性。

通过这些知识，学习者可以在实际编程中有效处理文件，并确保程序的健壮性。

### **课后练习**

1. 使用 `os` 模块创建一个嵌套目录（例如：`my_project/data/logs`），并检查目录是否成功创建。
2. 使用 `pathlib` 模块，编写一个程序，读取当前目录下的所有 `.txt` 文件，并将内容打印出来。
