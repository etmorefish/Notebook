# Python 数据结构

Python 提供了几种内置的数据结构，用于存储和操作数据。最常用的四种数据结构是列表（List）、元组（Tuple）、字典（Dictionary）和集合（Set）。它们各自具有不同的特性和用途，能够满足不同的编程需求。

## 1. 列表（List）

列表是一种有序且可变的数据结构，可以存储任意类型的元素。列表中的元素可以通过索引访问，索引从 0 开始。列表支持多种操作，例如添加、删除、修改元素等。

### 列表的定义与基本操作

```python
# 定义一个列表
fruits = ["Apple", "Banana", "Orange"]

# 获取list元素的个数
print(len(fruits)) # output: 3

# 访问列表中的元素
print(fruits[0])  # 输出: Apple
print(fruits[-1]) # 输出: Orange，负索引表示从末尾开始计数

print(fruits[3])  #  IndexError: list index out of range
# 当索引超出了范围时，Python会报一个IndexError错误，所以，要确保索引不要越界，记得最后一个元素的索引是len(fruits) - 1。

# 添加元素
fruits.append("Peach")

# 在指定位置插入元素
fruits.insert(1, "Lemon")  # 在索引 1 处插入 Lemon

# 修改元素
fruits[1] = "Mango"

# 删除元素
del fruits[2]  # 删除索引 2 处的元素
fruits.remove("Apple")  # 删除指定值的元素

# 弹出元素（删除并返回）
last_fruit = fruits.pop()  # 弹出最后一个元素

# 列表切片
slice_fruits = fruits[1:3]  # 获取索引 1 到 2 的元素，索引 3 处不包括
```

### 列表的高级操作

```python
# 列表合并
list1 = [1, 2, 3]
list2 = [4, 5, 6]
merged_list = list1 + list2

# 列表的重复
repeated_list = list1 * 3  # [1, 2, 3, 1, 2, 3, 1, 2, 3]

# 列表推导式
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### 列表的常用方法

```python
# 统计元素出现次数
count = fruits.count("Banana")

# 查找元素索引
index = fruits.index("Orange")

# 反转列表
fruits.reverse()

# 列表排序
numbers = [3, 1, 4, 1, 5]
numbers.sort()  # 升序排序

# 自定义排序
numbers.sort(reverse=True)  # 降序排序
```

### 列表的复杂度分析

- **访问**：O(1)
- **搜索**：O(n)
- **插入**：O(n) (最坏情况)
- **删除**：O(n) (最坏情况)

### 列表的使用场景

- **需要动态大小**：当你需要一个动态调整大小的数据结构时，列表是最合适的选择。
- **需要频繁添加或删除元素**：列表允许你在末尾高效地添加或删除元素。
- **需要保持元素顺序**：列表保持元素的插入顺序。

## 2. 元组（Tuple）

元组是一种有序且不可变的数据结构。一旦定义，元组中的元素就无法被修改。元组通常用于存储一组相关的数据，例如函数返回的多个值。

### 元组的定义与基本操作

```python
# 定义一个元组
t= (10, 20)
t1= ()  # 定义一个空元组
t2= (42,)  # 定义包含一个元素的元组, 如果直接(42),则表示42这个数，因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是42。

# 访问元组中的元素
print(t[0])  # 输出: 10

# 元组切片
slice_tuple = t[0:1]  # 获取元组的一部分

# 元组拆包
x, y = t # x = 10, y = 20
```

### 解释元组的不可变特性

<iframe width="400" height="300" frameborder="0" src="https://pythontutor.com/iframe-embed.html#code=t%20%3D%20%28'a',%20'b',%20%5B'A',%20'B'%5D%29%0At%5B2%5D%5B0%5D%20%3D%20'X'%0At%5B2%5D%5B1%5D%20%3D%20'Y'&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=3&heapPrimitives=nevernest&origin=opt-frontend.js&py=311&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe>

表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

### 元组的特点

- **有序性**：元组中的元素是有序的，可以通过索引访问。
- **不可变性**：元组是不可变的，一旦创建，内容不能修改。
- **重复性**：元组中可以包含重复的元素。

### 元组的高级操作

```python
# 元组拼接
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)
combined_tuple = tuple1 + tuple2  # (1, 2, 3, 4, 5, 6)

# 元组重复
repeated_tuple = tuple1 * 3  # (1, 2, 3, 1, 2, 3, 1, 2, 3)
```

### 元组的使用场景

- **不可变数据**：当你需要一组数据并且希望它们在整个程序生命周期内不被修改时，元组是理想的选择。
- **多值返回**：函数可以通过返回元组来同时返回多个值。
- **作为字典键**：元组是不可变的，因此可以作为字典的键。

## 3. 字典（Dictionary）

字典是一种无序的、可变的键值对集合。每个键（Key）都是唯一的，用于标识其对应的值（Value）。字典是 Python 中非常强大的数据结构，适合快速查找和修改数据。

### 字典的定义与基本操作

```python
# 定义一个字典
person = {"name": "John", "age": 25, "city": "Shanghai"}

# 访问字典中的值
print(person["company"])  # KeyError: 'company',如果key不存在，dict就会报错
print(person["name"])  # 输出: John

# 添加或修改键值对
person["career"] = "programmer"
person["age"] = 26  # 修改已有键的值

# 删除键值对
del person["city"]

# 获取字典的键和值
keys = person.keys()
values = person.values()

# 检查键是否存在
if "name" in person:
    print("find key 'name'")
```

### 字典的高级操作

```python
# 字典合并
additional_info = {"company": "ABC"}
person.update(additional_info)  # 合并字典

# 字典推导式
squares_dict = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# 获取值并提供默认值
person.get("age")  # 如果不存在，返回 None
age = person.get("age", 30)  # 如果不存在键 age'，返回默认值 30
```

### 字典的复杂度分析

- **访问**：O(1)
- **插入**：O(1)
- **删除**：O(1)

### 字典的使用场景

- **快速查找**：字典非常适合需要快速查找数据的场景，例如根据用户名查找用户信息。
- **键值对存储**：当你需要存储关联数据时，字典是最佳选择，例如商品的价格表。

## 4. 集合（Set）

set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。集合是一种无序的、不可重复的元素集合。集合通常用于去除重复元素或进行数学集合操作（如交集、并集等）。

### 集合的定义与基本操作

```python
# 定义一个集合
unique_numbers = {1, 2, 3, 4, 4}  # 重复元素会被自动去除

# 添加元素
unique_numbers.add(5)

# 删除元素
unique_numbers.remove(2)  # 如果元素不存在，将引发 KeyError
unique_numbers.discard(10)  # 如果元素不存在，不会引发错误

# 集合操作
intersection = {1, 2, 3} & {2, 3, 4}  # 交集 {2, 3}
union = {1, 2, 3} | {2, 3, 4}  # 并集 {1, 2, 3, 4}
difference = {1, 2, 3} - {2, 3, 4}  # 差集 {1}
```

### 集合的高级操作

```python
# 集合推导式
squares_set = {x**2 for x in range(10)}  # {0, 1, 4, 9, 16, 25, 36, 49, 64, 81}

# 冻结集合（不可变集合）
frozen_set = frozenset([1, 2, 3, 4])
```

### 集合的复杂度分析

- **访问**：O(1)

- **插入**：O(1)
- **删除**：O(1)

### 集合的使用场景

- **去除重复元素**：当你需要一个无重复元素的列表时，集合是最优选择。
- **集合运算**：当你需要执行交集、并集、差集等数学集合操作时，集合非常适合。

## 总结

Python 提供了多种数据结构，每种数据结构都有其独特的用途。了解它们的特性和操作方式，能够帮助你在编程时做出最佳选择，提升代码的性能和可读性。
