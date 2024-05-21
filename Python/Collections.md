# collections - 容器数据类型

> [reference](https://docs.python.org/zh-cn/3/library/collections.html#module-collections)

Python的`collections`模块提供了一些额外的数据结构，这些数据结构扩展了Python内置的数据类型，为用户提供了更强大和灵活的工具。以下是`collections`模块中的主要数据结构及其用途：

- namedtuple(): 一个工厂函数，用来创建元组的子类，子类的字段是有名称的。
- deque: 类似列表的容器，但 append 和 pop 在其两端的速度都很快。
- ChainMap: 类似字典的类，用于创建包含多个映射的单个视图。
- Counter: 用于计数 hashable 对象的字典子类
- OrderedDict: 字典的子类，能记住条目被添加进去的顺序。
- defaultdict: 字典的子类，通过调用用户指定的工厂函数，为键提供默认值。
- UserDict: 封装了字典对象，简化了字典子类化
- UserList: 封装了列表对象，简化了列表子类化
- UserString: 封装了字符串对象，简化了字符串子类化

### 1. `namedtuple`
`namedtuple`是一个工厂函数，用于创建具名元组子类。具名元组类似于普通的元组，但它们的字段可以通过名称访问，而不仅仅是通过位置访问。

```python
from collections import namedtuple

# 创建一个具名元组类 Point
Point = namedtuple('Point', ['x', 'y'])

# 创建一个 Point 对象
p = Point(11, 22)

print(p.x)  # 输出：11
print(p.y)  # 输出：22
print(p)    # 输出：Point(x=11, y=22)
```

### 2. `deque`
`deque`（双端队列）是一个支持快速从两端添加和删除元素的线程安全数据类型。它是一个双端队列，可以用作栈和队列。

```python
from collections import deque

# 创建一个空的双端队列
d = deque()

# 在右端添加元素
d.append(1)
d.append(2)

# 在左端添加元素
d.appendleft(0)

print(d)  # 输出：deque([0, 1, 2])

# 从右端删除元素
d.pop()

# 从左端删除元素
d.popleft()

print(d)  # 输出：deque([1])
```

### 3. `Counter`
`Counter`是一个用于计数的字典子类。它用于计数可哈希对象的实例数。

```python
from collections import Counter

# 创建一个 Counter 对象
c = Counter('gallahad')

print(c)  # 输出：Counter({'a': 3, 'l': 2, 'g': 1, 'h': 1, 'd': 1})

# 更新计数
c.update('aab')
print(c)  # 输出：Counter({'a': 5, 'l': 2, 'g': 1, 'h': 1, 'd': 1, 'b': 1})

# 获取最常见的元素
print(c.most_common(2))  # 输出：[('a', 5), ('l', 2)]
```

### 4. `OrderedDict`
`OrderedDict`是一个字典子类，它记住了插入元素的顺序。与Python 3.7+中的内置`dict`不同，`OrderedDict`在所有Python版本中都保证这个特性。

```python
from collections import OrderedDict

# 创建一个有序字典
od = OrderedDict()

# 按顺序添加元素
od['banana'] = 3
od['apple'] = 4
od['pear'] = 1

print(od)  # 输出：OrderedDict([('banana', 3), ('apple', 4), ('pear', 1)])
```

### 5. `defaultdict`
`defaultdict`是一个字典子类，它提供了一个默认值供不存在的键使用。这个默认值由传递给`defaultdict`构造函数的工厂函数提供。

```python
from collections import defaultdict

# 创建一个 defaultdict，默认值为 int（0）
dd = defaultdict(int)

print(dd['missing'])  # 输出：0，因为 'missing' 键不存在，返回默认值

# 更新键值
dd['apple'] = 4
print(dd)  # 输出：defaultdict(<class 'int'>, {'missing': 0, 'apple': 4})
```

### 6. `ChainMap`
`ChainMap`是用于管理多个字典的容器。它将多个字典合并成一个视图，用户可以像使用一个字典一样访问这些字典中的所有键值对。

```python
from collections import ChainMap

# 创建两个字典
dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}

# 创建一个 ChainMap
cm = ChainMap(dict1, dict2)

print(cm['a'])  # 输出：1
print(cm['b'])  # 输出：2
print(cm['c'])  # 输出：4
```

### 7. `UserDict`, `UserList`, `UserString`
这些类是可变的封装器类，用于创建用户定义的数据结构，分别用于字典、列表和字符串。它们可以通过继承这些类并重写其方法来创建自定义数据结构。

```python
from collections import UserDict

class MyDict(UserDict):
    def __setitem__(self, key, value):
        if isinstance(value, int):
            super().__setitem__(key, value)
        else:
            raise ValueError("Only integers are allowed")

my_dict = MyDict()
my_dict['a'] = 10  # 正常
# my_dict['b'] = 'value'  # 引发 ValueError
```

这些数据结构扩展了Python内置的数据类型，提供了更强大和灵活的工具，适用于各种编程场景。了解并使用这些数据结构可以使你的代码更简洁、高效和易读。