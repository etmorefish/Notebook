# 第六章：面向对象编程基础

本章将深入探讨面向对象编程（OOP）的核心概念，包括类和实例、访问限制、继承和多态、获取对象信息、以及实例属性和类属性。通过这些知识，您将能够更好地理解和应用 Python 的 OOP 特性。

## 1. 类和实例

### 1.1 类是什么？

类可以看作是一种蓝图或模板，定义了某类事物的共同行为和属性。比如，`Cat` 类可以描述所有猫的共同特征：名字、年龄、叫声等。

### 1.2 实例是什么？

实例是根据类创建的具体对象。每个实例都有自己独立的属性和行为。就像根据房屋的设计图（类）建造的具体房子（实例）一样，您可以根据 `Cat` 类创建多只猫的实例。

**示例：**

```python
class Cat:
    def __init__(self, name, age):
        self.name = name  # 实例属性
        self.age = age

    def meow(self):
        print(f"{self.name} is meowing!")

# 创建实例
cat1 = Cat("Kiki", 3)
cat2 = Cat("Max", 5)

# 访问实例属性和方法
print(cat1.name)  # 输出：Kiki
cat2.meow()  # 输出：Max is meowing!
```

在这个例子中，`Cat` 类定义了猫的属性 `name` 和 `age`，以及行为 `meow()`。`cat1` 和 `cat2` 是两个不同的实例，分别表示两只猫。

## 2. 访问限制

### 2.1 公有属性与方法

在 Python 中，默认情况下，类的属性和方法都是公有的，可以直接访问。例如，您可以直接通过 `cat1.name` 访问 `name` 属性。

### 2.2 私有属性与方法

有时候，我们希望类的某些属性或方法不被外部直接访问，这时就可以将它们定义为私有。私有属性和方法在命名时以双下划线 `__` 开头。

**示例：**

```python
class Cat:
    def __init__(self, name, age):
        self.name = name
        self.__age = age  # 私有属性

    def get_age(self):
        return self.__age  # 公有方法，访问私有属性

# 创建实例
cat = Cat("Kiki", 3)

# 访问私有属性
print(cat.get_age())  # 输出：3
print(cat.__age)  # 报错：AttributeError
```

在这个例子中，`__age` 是一个私有属性，外部无法直接访问，必须通过类提供的公有方法 `get_age()` 来访问。

## 3. 继承和多态

### 3.1 继承

继承是一种创建新类的方式，新类可以继承父类的属性和方法，并可以添加新的属性和方法。继承可以提高代码的复用性。

**示例：**

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def make_sound(self):
        print(f"{self.name} makes a sound")

class Cat(Animal):  # Cat 类继承自 Animal 类
    def make_sound(self):
        print(f"{self.name} meows")

# 创建实例
cat = Cat("Kiki")
cat.make_sound()  # 输出：Kiki meows
```

在这个例子中，`Cat` 类继承了 `Animal` 类，并重写了 `make_sound()` 方法。

### 3.2 多态

多态是指子类可以重写父类的方法，且对象在运行时可以调用实际类型的方法。多态允许不同类的对象以统一的接口进行操作。

**示例：**

```python
class Dog(Animal):
    def make_sound(self):
        print(f"{self.name} braks")

def animal_sound(animal):
    animal.make_sound()

# 多态示例
cat = Cat("Kiki")
dog = Dog("Timi")

animal_sound(cat)  # 输出：Kiki meows
animal_sound(dog)  # 输出：Timi braks
```

在这个例子中，无论是 `Cat` 还是 `Dog`，都可以通过 `animal_sound()` 函数调用自己的 `make_sound()` 方法，这就是多态的体现。

## 4. 获取对象信息

在编程中，我们经常需要获取对象的某些信息，比如对象的类型、对象是否具有某个属性或方法等。Python 提供了许多内置函数来获取对象的信息。

### 4.1 使用 `type()` 获取对象类型

`type()` 函数返回对象的类型。

**示例：**

```python
print(type(cat))  # 输出：<class '__main__.Cat'>
```

### 4.2 使用 `isinstance()` 判断对象是否属于某个类

`isinstance()` 函数可以用来判断一个对象是否是某个类的实例。

**示例：**

```python
print(isinstance(cat, Cat))  # 输出：True
print(isinstance(cat, Animal))  # 输出：True
```

### 4.3 使用 `hasattr()` 检查对象是否具有某个属性

`hasattr()` 函数可以检查对象是否具有指定的属性。

**示例：**

```python
print(hasattr(cat, 'name'))  # 输出：True
print(hasattr(cat, 'color'))  # 输出：False
```

## 5. 实例属性和类属性

### 5.1 实例属性

实例属性是与某个实例绑定的属性，每个实例的属性是相互独立的。实例属性通常在 `__init__` 方法中定义。

**示例：**

```python
class Cat:
    def __init__(self, name):
        self.name = name  # 实例属性
```

### 5.2 类属性

类属性是所有实例共享的属性，它属于类本身。类属性通常在类体内直接定义，而不是在 `__init__` 方法中。

**示例：**

```python
class Cat:
    species = "Feild"  # 类属性

    def __init__(self, name):
        self.name = name  # 实例属性

# 访问类属性
print(Cat.species)  # 输出：Feild

# 所有实例共享类属性
cat1 = Cat("Kiki")
cat2 = Cat("Max")
print(cat1.species)  # 输出：Feild
print(cat2.species)  # 输出：Feild
```

在这个例子中，`species` 是一个类属性，所有 `Cat` 类的实例都共享这个属性。

### 总结

本章详细介绍了面向对象编程的核心概念，包括类和实例、访问限制、继承和多态、获取对象信息、以及实例属性和类属性。通过这些知识，学习者可以深入理解并应用 OOP 的基本原则，使代码更加模块化和易于维护。

## 作业

### 1. 编写一个“学生管理系统”的类

要求：

- 创建一个名为 `Student` 的类，包含学生的姓名、学号、成绩三个属性。
- 提供 `add_grade` 方法，用于添加成绩，并更新 `成绩` 属性。
- 提供 `get_info` 方法，用于返回学生的基本信息（姓名、学号和成绩）。
- 使用继承创建一个名为 `GraduateStudent` 的类，扩展 `Student` 类，增加导师 `advisor` 属性。
- 在 `GraduateStudent` 类中重写 `get_info` 方法，除了返回姓名、学号和成绩，还要返回导师的信息。

编写代码，并实例化几个 `Student` 和 `GraduateStudent` 对象，测试各个方法的功能。

### 2. 探索类属性与实例属性

- 创建一个名为 `Car` 的类，包含一个类属性 `total_cars`，用于记录所有 `Car` 实例的数量。
- 在 `Car` 类中定义 `__init__` 方法，当实例化一个 `Car` 对象时，`total_cars` 自增1。
- 创建多个 `Car` 实例，并打印 `total_cars`，观察类属性的变化。
- 探索如何使用类属性与实例属性的区别，以及它们在实际应用中的作用。

----

> 答案示例：

```python
# 作业1：编写一个“学生管理系统”的类

class Student:
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
        self.grade = None  # 初始成绩为 None

    def add_grade(self, grade):
        self.grade = grade

    def get_info(self):
        return f"姓名: {self.name}, 学号: {self.student_id}, 成绩: {self.grade if self.grade is not None else '未录入'}"

# 继承 Student 类，创建 GraduateStudent 类
class GraduateStudent(Student):
    def __init__(self, name, student_id, advisor):
        super().__init__(name, student_id)
        self.advisor = advisor

    # 重写 get_info 方法，加入导师信息
    def get_info(self):
        base_info = super().get_info()
        return f"{base_info}, 导师: {self.advisor}"

# 实例化 Student 类
student1 = Student("张三", "2023001")
student1.add_grade(85)
print(student1.get_info())  # 输出：姓名: 张三, 学号: 2023001, 成绩: 85

# 实例化 GraduateStudent 类
grad_student1 = GraduateStudent("李四", "2023002", "王教授")
grad_student1.add_grade(90)
print(grad_student1.get_info())  # 输出：姓名: 李四, 学号: 2023002, 成绩: 90, 导师: 王教授
```

这个答案展示了如何使用 Python 面向对象编程的基本概念来构建一个简单的学生管理系统，包含了类的定义、继承以及方法的重写。

```python
# 作业2：探索类属性与实例属性

class Car:
    # 类属性
    total_cars = 0

    def __init__(self, make, model):
        self.make = make  # 实例属性
        self.model = model  # 实例属性
        Car.total_cars += 1  # 每创建一个实例，类属性自增1

# 创建多个 Car 实例
car1 = Car("Toyota", "Camry")
car2 = Car("Honda", "Accord")
car3 = Car("Tesla", "Model S")

# 打印类属性 total_cars
print(Car.total_cars)  # 输出：3

# 打印实例的 make 和 model
print(car1.make, car1.model)  # 输出：Toyota Camry
print(car2.make, car2.model)  # 输出：Honda Accord
print(car3.make, car3.model)  # 输出：Tesla Model S
```

这个答案展示了如何使用类属性和实例属性，以及它们之间的区别。`total_cars` 是一个类属性，记录了所有 `Car` 实例的数量，并且所有实例共享这个类属性。
