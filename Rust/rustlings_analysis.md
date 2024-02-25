# Rustlings Analysis

## 00_intro

### 介绍
学会使用`print!` 和 `println!`宏打印字符串到标准输出。

### 练习
- intro1.rs
- intro1.rs

## 01_variables

### 介绍
学习如何声明变量，并使用`let`关键字。
在Rust中，默认情况下变量是不变的。当变量不变时，一旦值绑定到名称，就无法更改该值。您可以通过在可变名称的前面添加mut来使其变异。

#### [变量与可变性](https://kaisery.github.io/trpl-zh-cn/ch03-01-variables-and-mutability.html)

**常量**（constants）是 Rust 中绑定到一个名称的不允许改变的值。与变量不同，常量具有以下特点：

1. **不可变性：** 常量是不可变的，不能使用 `mut` 关键字进行修改。一旦绑定了值，就不能再改变它。
2. **类型注解：** 在声明常量时，必须注明值的类型。这是因为 Rust 通过类型注解来确保程序的类型安全性。常量的类型注解是必须的，而变量的类型注解通常是可选的，因为 Rust 可以通过类型推导自动推断变量的类型。
3. **作用域：** 常量可以在任何作用域中声明，包括全局作用域。这使得常量在程序的多个部分中都能被访问，有助于提供共享的不可变值。
4. **常量表达式：** 常量必须被设置为常量表达式，而不是其他任何只能在运行时计算出的值。这意味着常量的值必须在编译时就能确定，而不能依赖于运行时计算。

### 练习

- variables1.rs
  ```rust
  fn main() {
      // x = 5;
      let x = 5;   // 使用let声明变量
      println!("x has the value {}", x);
  }
  ```

- variables2.rs
  ```rust
  fn main() {
      // let x;
      let x = 10;  // 不能直接设置一个空变量
      if x == 10 {
          println!("x is ten!");
      } else {
          println!("x is not ten!");
      }
  }
  ```

- variables3.rs
  ```rust
  fn main() {
      // let x: i32;
      let x: i32 = 0;  // 定义变量时必须赋予一个具体的值
      println!("Number {}", x);
  }
  ```

- variables4.rs
  ```rust
  fn main() {
      // let x = 3;
      let mut x = 3;	// 不能对不可变变量 x 二次赋值, 需要通过 mut 将其修改为可变变量
      println!("Number {}", x);
      x = 5; // don't change this line
      println!("Number {}", x);
  }
  ```

- variables5.rs
  ```rust
  fn main() {
      let number = "T-H-R-E-E"; // don't change this line
      println!("Spell a Number : {}", number);
      // number = 3; // don't rename this variable
      let number = 3;  // 变量默认是不可改变的（immutable），要想改变number需要对其重新赋值，即第一个变量被第二个 隐藏（Shadowing） 了
      println!("Number plus two is : {}", number + 2);
  }
  ```

- variables6.rs
  ```rust
  // const NUMBER = 3;
  const NUMBER: i32 = 3;  // 声明常量的时候需要添加上对应的类型
  fn main() {
      println!("Number {}", NUMBER);
  }
  ```


## 02_functions

### 介绍
学习如何编写函数以及 Rust 编译器如何帮助您调试错误，即使是在更复杂的代码中。

在 Rust 中，有两个主要的概念：语句（Statements）和表达式（Expressions）。

1. **语句（Statements）：**
   - 语句是执行一些操作但不返回值的指令。
   - 语句的主要目的是执行一些操作，例如声明变量、执行赋值、执行函数调用，等等。
   - 语句的执行通常不产生一个可用的值，它们主要用于执行操作的副作用。

   ```rust
   let x = 5; // 这是一个语句
   println!("Hello, World!"); // 这也是一个语句
   ```

2. **表达式（Expressions）：**
   - 表达式计算并产生一个值。
   - 表达式的主要目的是计算值，它们可以作为语句的一部分，也可以作为函数体的结尾表达式。
   - 表达式可以包括数字、运算符、函数调用等，它们都会产生一个值。

   ```rust
   let y = {
       let x = 5;
       x + 1 // 这是一个表达式
   };

   let result = if y > 5 { "greater" } else { "less or equal" }; // if 表达式
   ```

在 Rust 的函数体中，函数体由一系列的语句和一个可选的结尾表达式构成。函数体的最后一个表达式会成为函数的返回值，或者如果没有结尾表达式，则函数返回 `()`（unit 类型）。这种设计使得 Rust 函数在语法上更加一致，并且函数的返回值非常显式。

### 练习

- functions1.rs
- functions2.rs
  ```rust
  fn main() {
      call_me(3);
  }

  // fn call_me(num:) {
  fn call_me(num: i32) {   // 函数的参数需要添加对应的类型
      for i in 0..num {
          println!("Ring! Call number {}", i + 1);
      }
  }
  ```

- functions3.rs
  ```rust
  fn main() {
      // call_me();
      call_me(5);  // 调用函数的时候需要传入对应的参数
  }

  fn call_me(num: u32) {
      for i in 0..num {
          println!("Ring! Call number {}", i + 1);
      }
  }
  ```
- functions4.rs
  ```rust
  fn main() {
    let original_price = 51;
    println!("Your sale price is {}", sale_price(original_price));
  }

  // fn sale_price(price: i32) -> {
  fn sale_price(price: i32) -> i32{ // 函数的返回值需要添加对应的类型
    if is_even(price) {
        price - 10
    } else {
        price - 3
    }
  }

  fn is_even(num: i32) -> bool {
    num % 2 == 0
  }
  ```

- functions5.rs
  ```rust
  fn main() {
    let answer = square(3);
    println!("The square of 3 is {}", answer);
  }

  fn square(num: i32) -> i32 {
      // num * num;
      num * num   // rust中以分号结束的叫做表达式或者语句，表达式会返回一个值
                  // 如果没有分号，则返回最后一个表达式的值
  }
  ```

## 03_if

### 介绍
`if`是最基本的控制流结构，它允许根据条件来执行不同的代码块。

#### 在 let 语句中使用 if
`if let` 是 Rust 中的一种模式匹配表达式，用于简化对某个值进行模式匹配的代码。它的语法是：

```rust
if let Some(pattern) = some_value {
    // 如果模式匹配成功，则执行这里的代码块
} else {
    // 如果模式匹配不成功，则执行这里的代码块
}
```

`if let` 语句用于替代传统的 `match` 表达式，特别适用于处理 `Option` 类型的值，以及其他可能的模式匹配情况。主要特点如下：

1. **简化模式匹配：** `if let` 提供了一种简洁的方式来处理特定模式的匹配，避免了使用 `match` 时的冗长语法。

2. **适用于 Option 类型：** 常用于处理 `Option` 类型的值，可以更轻松地提取 `Some` 的值而忽略 `None` 的情况。

3. **可用于其他模式：** 不仅限于处理 `Option`，也可以用于任何支持模式匹配的情况，例如枚举类型、元组等。

下面是一个使用 `if let` 处理 `Option` 类型的简单示例：

```rust
fn main() {
    let some_value: Option<i32> = Some(42);

    if let Some(x) = some_value {
        println!("Got a value: {}", x);
    } else {
        println!("Got None");
    }
}
```

在这个例子中，如果 `some_value` 是 `Some` 类型，则 `x` 被绑定到内部的值，而在 `None` 情况下执行相应的代码块。

### 练习

- if1.rs
  ```rust
  pub fn bigger(a: i32, b: i32) -> i32 {
      // Complete this function to return the bigger number!
      // Do not use:
      // - another function call
      // - additional variables
      if a > b {
          a
      }else{
          b
      }
  }

  // Don't mind this for now :)
  #[cfg(test)]
  mod tests {
      use super::*;

      #[test]
      fn ten_is_bigger_than_eight() {
          assert_eq!(10, bigger(10, 8));
      }

      #[test]
      fn fortytwo_is_bigger_than_thirtytwo() {
          assert_eq!(42, bigger(32, 42));
      }

      #[test]
      fn equal_numbers() {
          assert_eq!(42, bigger(42, 42));
      }
  }
  ```

- if2.rs
  ```rust
  pub fn foo_if_fizz(fizzish: &str) -> &str {
    // TODO
      if fizzish == "fizz" {
          "foo"
      } else if fizzish == "fuzz"{
          "bar"
      }else{
          "baz"
      }
  }

  // No test changes needed!
  #[cfg(test)]
  mod tests {
      use super::*;

      #[test]
      fn foo_for_fizz() {
          assert_eq!(foo_if_fizz("fizz"), "foo")
      }

      #[test]
      fn bar_for_fuzz() {
          assert_eq!(foo_if_fizz("fuzz"), "bar")
      }

      #[test]
      fn default_to_baz() {
          assert_eq!(foo_if_fizz("literally anything"), "baz")
      }
  }
  ```

- if3.rs
  ```rust
  pub fn animal_habitat(animal: &str) -> &'static str {
      let identifier = if animal == "crab" {
          1
      } else if animal == "gopher" {
          // 2.0
          2
      } else if animal == "snake" {
          3
      } else {
          0
      };

      // DO NOT CHANGE THIS STATEMENT BELOW
      let habitat = if identifier == 1 {
          "Beach"
      } else if identifier == 2 {
          "Burrow"
      } else if identifier == 3 {
          "Desert"
      } else {
          "Unknown"
      };

      habitat
  }

  // No test changes needed.
  #[cfg(test)]
  mod tests {
      use super::*;

      #[test]
      fn gopher_lives_in_burrow() {
          assert_eq!(animal_habitat("gopher"), "Burrow")
      }

      #[test]
      fn snake_lives_in_desert() {
          assert_eq!(animal_habitat("snake"), "Desert")
      }

      #[test]
      fn crab_lives_on_beach() {
          assert_eq!(animal_habitat("crab"), "Beach")
      }

      #[test]
      fn unknown_animal() {
          assert_eq!(animal_habitat("dinosaur"), "Unknown")
      }
  }
  ```

## 04_primitive_type

### 介绍

Rust 中的数据类型分为标量（scalar）和复合（compound）两类。Rust 是静态类型语言，因此在编译时就必须确定所有变量的类型。在类型不明确的情况下，可以通过类型注解来指定类型。

#### 标量类型

标量类型代表单一值，分为以下几种：

1. **整型（Integer Types）：** 表示整数，分为有符号和无符号，不同位数。例如，`i32` 表示有符号 32 位整数，`u8` 表示无符号 8 位整数。

2. **浮点型（Floating-Point Types）：** 表示带小数点的数字，分为 `f32` 和 `f64`，分别是单精度和双精度浮点数。

3. **布尔型（Boolean Type）：** 只有两个值，`true` 和 `false`。

4. **字符型（Character Type）：** 代表单个字符，使用单引号 `'`。例如，`'a'`。

#### 复合类型

复合类型可以将多个值组合成一个类型。Rust 中有两个主要的复合类型：

1. **元组（Tuple）：** 一个固定长度的有序集合，元组中的每个位置都有一个类型。元组的长度一旦确定就不能改变。

    ```rust
    let tup: (i32, f64, u8) = (500, 6.4, 1);
    ```

2. **数组（Array）：** 一个固定长度的相同类型的元素集合。数组的长度也是固定的。

    ```rust
    let a: [i32; 5] = [1, 2, 3, 4, 5];
    ```

#### 数值运算

Rust 中的所有数字类型支持基本的数学运算，包括加法、减法、乘法、除法和取余。整数除法会向零舍入。

#### 数组元素访问

数组的元素可以通过索引访问，索引从 0 开始。越界访问数组会导致运行时错误，称为 panic。Rust 在运行时检查数组索引是否有效，确保安全的数组访问。

#### 切片类型

当你需要引用集合中的一部分而不是整个集合时，Rust 提供了切片（slice）类型。切片允许你引用集合中的一个范围，而无需复制数据。

在 Rust 中，切片是对数组或其他集合的引用，指定了一个范围。切片使用 `&` 符号，并包含两个索引，用 `..` 或 `..=` 连接，表示起始索引和终止索引。切片的类型写作 `&[T]`，其中 `T` 是元素类型。

```rust
let numbers = [1, 2, 3, 4, 5];
let slice = &numbers[1..4];  // 包含索引 1、2、3 的切片
```

上述代码中，`slice` 是对 `numbers` 数组的引用，包含索引 1 到 3 的元素。注意，切片的终止索引是开区间，即不包括终止索引指向的元素。

#### 字符串切片

字符串切片是一种特殊的切片，用于引用字符串的一部分。字符串切片的类型写作 `&str`。

```rust
let message = "Hello, World!";
let greeting = &message[0..5];  // 包含索引 0、1、2、3、4 的切片
```

#### 全范围切片

如果希望切片包含整个集合，可以使用全范围切片。全范围切片的写法是 `&collection[..]`。

```rust
let numbers = [1, 2, 3, 4, 5];
let whole_slice = &numbers[..];  // 包含整个数组的切片
```

### 切片与函数参数

切片经常用于函数参数，以便接受不同长度的集合。例如：

```rust
fn process_numbers(numbers: &[i32]) {
    for &num in numbers {
        println!("{}", num);
    }
}

let numbers = [1, 2, 3, 4, 5];
process_numbers(&numbers[1..4]);  // 传递数组的切片作为参数
```

在函数签名中，`&[i32]` 表示接受一个 `i32` 类型元素的切片作为参数。

切片是 Rust 中强大而灵活的工具，使得在不复制数据的情况下操作集合的子集成为可能。

### 练习

- primitive_types1.rs

    ```rust
    fn main() {
        // Booleans (`bool`)

        let is_morning = true;
        if is_morning {
            println!("Good morning!");
        }

        // let // Finish the rest of this line like the example! Or make it be false!
        let is_evening = true;	// bool 值
        if is_evening {
            println!("Good evening!");
        }
    }
    ```
- primitive_types2.rs
    ```rust
    fn main() {
        // Characters (`char`)

        // Note the _single_ quotes, these are different from the double quotes
        // you've been seeing around.
        let my_first_initial = 'C';
        if my_first_initial.is_alphabetic() {
            println!("Alphabetical!");
        } else if my_first_initial.is_numeric() {
            println!("Numerical!");
        } else {
            println!("Neither alphabetic nor numeric!");
        }

        let your_character = '🦀';  // 可以是任何字符或者Unicode字符
        // let // Finish this line like the example! What's your favorite character?
        // Try a letter, try a number, try a special character, try a character
        // from a different language than your own, try an emoji!
        if your_character.is_alphabetic() {
            println!("Alphabetical!");
        } else if your_character.is_numeric() {
            println!("Numerical!");
        } else {
            println!("Neither alphabetic nor numeric!");
        }
    }
    ```

- primitive_types3.rs

    ```rust
    fn main() {
        // let a = ???
        let a = [0;100];  // [0; 100] 的含义是创建一个包含 100 个元素的数组，每个元素的初始值都是 0
                        // or  vec![0;100]; 创建一个包含 100 个元素的 Vec（动态数组），并将每个元素初始化为 0 的一种便捷方法。
                        // 这里有一个重要的区别：Vec 是一个动态数组，可以在运行时改变长度。而数组的长度是在编译时确定的。
        if a.len() >= 100 {
            println!("Wow, that's a big array!");
        } else {
            println!("Meh, I eat arrays like that for breakfast.");
            panic!("Array not big enough, more elements needed")
        }
    }
    ```

- primitive_types4.rs
    ```rust
    #[test]
    fn slice_out_of_array() {
        let a = [1, 2, 3, 4, 5];

        let nice_slice = &a[1..4];

        assert_eq!([2, 3, 4], nice_slice)
    }
    ```

- primitive_types5.rs

    ```rust
    fn main() {
        let cat = ("Furry McFurson", 3.5);
        // let /* your pattern here */ = cat;
        let (name, age) = cat;  // 解构元组，将元组中的值分别绑定到变量 name 和 age
        println!("{} is {} years old.", name, age);
    }
    ```

- primitive_types6.rs

    ```rust
    #[test]
    fn indexing_tuple() {
        let numbers = (1, 2, 3);
        // Replace below ??? with the tuple indexing syntax.
        // let second = ???;
        let second = numbers.1;  // 使用元组索引语法获取第二个元素。
        assert_eq!(2, second,
            "This is not the 2nd number in the tuple!")
    }
    ```


## 05_vecs

### 介绍
Vectors（向量）是 Rust 中的一种动态数组类型，也称为动态数组或可变长度数组。Vectors 允许在运行时增加或减少其长度，这使它们非常灵活。Vectors 属于标准库中的 `Vec<T>` 类型。

以下是有关 Vectors 的基本信息：

#### 创建 Vector

创建一个空的 Vector：

```rust
let v: Vec<i32> = Vec::new();
```

使用 `vec!` 宏创建包含初始元素的 Vector：
```rust
let v = vec![1, 2, 3];
```

#### 添加元素

可以使用 `push` 方法将元素添加到 Vector 的末尾：
```rust
let mut v = Vec::new();
v.push(1);
v.push(2);
v.push(3);
```

#### 访问元素

通过索引访问 Vector 的元素：
```rust
let v = vec![1, 2, 3];
let third_element = v[2]; // 注意：索引越界会导致 panic
```

使用 `get` 方法安全地访问元素，如果索引越界返回 `None`：
```rust
let v = vec![1, 2, 3];
if let Some(third_element) = v.get(2) {
    // 处理 third_element
} else {
    // 索引越界
}
```

#### 更新元素

通过索引更新 Vector 的元素：
```rust
let mut v = vec![1, 2, 3];
v[1] = 5;
```

#### 删除元素

使用 `pop` 方法删除 Vector 的末尾元素：
```rust
let mut v = vec![1, 2, 3];
let popped_element = v.pop(); // 返回 Option<T>
```

#### 遍历 Vector

使用 `for` 循环遍历 Vector：
```rust
let v = vec![1, 2, 3];
for element in &v {
    // 处理每个元素
}
```

`iter_mut` 和 `map` 是 Rust 中用于处理集合（如数组、向量等）的两个重要方法。

### `iter_mut`

`iter_mut` 是 Iterator trait 提供的一个方法，它返回一个可变的迭代器，允许你以可变的方式迭代集合中的元素。这对于需要修改集合中的元素时非常有用。

例子：
```rust
fn main() {
    let mut numbers = vec![1, 2, 3, 4, 5];

    for num in numbers.iter_mut() {
        *num *= 2; // 将每个元素乘以 2
    }

    println!("{:?}", numbers); // 输出 [2, 4, 6, 8, 10]
}
```

在上面的例子中，`iter_mut` 返回一个可变迭代器，允许我们使用 `for` 循环遍历集合中的每个元素并修改它们。

### `map`

`map` 是一个高阶函数，它接受一个闭包作为参数，并将闭包应用于集合的每个元素，最终生成一个新的集合。

例子：
```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    //let doubled_numbers: Vec<i32> = numbers.iter().map(|&x| x * 2).collect();
    let doubled_numbers: Vec<i32> = numbers.iter().map(|x| x * 2).collect();
    
    println!("{:?}", doubled_numbers); // 输出 [2, 4, 6, 8, 10]
}

// 在这两种写法中，主要区别在于闭包参数的引用方式。让我们逐步解释：

// |&x| x * 2：这里的 &x 表示闭包参数 x 是一个引用。这是因为 iter() 方法返回的是一个迭代器，而迭代器的元素类型是引用。因此，在闭包中我们使用 &x 来匹配这个引用。

//  |x| x * 2：这里的 x 直接是值，不是引用。这是因为 iter() 方法返回的迭代器提供了对值的所有权，而不是引用。

// 在两种情况下，map 方法都可以正确工作，因为 Rust 的闭包是可以智能地处理引用和所有权的。在这里，由于我们只是进行简单的乘法操作，两者的效果是相同的，不用纠结。
```

在上面的例子中，`map` 方法将闭包 `|&x| x * 2` 应用于每个元素，生成一个新的集合 `doubled_numbers`，其中的每个元素都是原始集合中对应元素的两倍。

`map` 通常用于对集合中的每个元素执行某种操作并生成一个新的集合，而不改变原始集合。

这两个方法的结合可以在迭代集合的同时进行元素的修改或生成新的集合。

#### Vector 的所有权

Vector 的所有权可以通过将它传递给函数或方法来转移。对 Vector 进行传递和返回通常会导致所有权的转移。

Vectors 提供了在程序运行时动态管理元素的能力，是 Rust 中常用的数据结构之一。

### 练习

- vecs1.rs
    ```rust
    fn array_and_vec() -> ([i32; 4], Vec<i32>) {
        let a = [10, 20, 30, 40]; // a plain array
        // let v = // TODO: declare your vector here with the macro for vectors
        let v = vec![10, 20, 30, 40];  // 使用vec!宏创建一个vector
        (a, v)
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn test_array_and_vec_similarity() {
            let (a, v) = array_and_vec();
            assert_eq!(a, v[..]);
        }
    }
    ```

- vecs2.rs
    ```rust
    fn vec_loop(mut v: Vec<i32>) -> Vec<i32> {
        for element in v.iter_mut() {
            // TODO: Fill this up so that each element in the Vec `v` is
            // multiplied by 2.
            // ???
            *element *= 2;  // 这里使用*来解引用，因为element是一个引用
        }

        // At this point, `v` should be equal to [4, 8, 12, 16, 20].
        v
    }

    fn vec_map(v: &Vec<i32>) -> Vec<i32> {
        v.iter().map(|element| {
            // TODO: Do the same thing as above - but instead of mutating the
            // Vec, you can just return the new number!
            // ???
            element * 2  // 这里返回一个新的数字
        }).collect()
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn test_vec_loop() {
            let v: Vec<i32> = (1..).filter(|x| x % 2 == 0).take(5).collect();
            let ans = vec_loop(v.clone());

            assert_eq!(ans, v.iter().map(|x| x * 2).collect::<Vec<i32>>());
        }

        #[test]
        fn test_vec_map() {
            let v: Vec<i32> = (1..).filter(|x| x % 2 == 0).take(5).collect();
            let ans = vec_map(&v);

            assert_eq!(ans, v.iter().map(|x| x * 2).collect::<Vec<i32>>());
        }
    }
    ```

## 06_move_semantics

### 介绍

移动语义（Move Semantics）是指在编程语言中对数据的所有权（ownership）进行有效管理的一种机制。在Rust中，移动语义是一项重要的特性，与所有权系统密切相关。

总体来说，移动语义有以下几个关键点：

1. **所有权和移动：** 在Rust中，每个值都有一个唯一的所有者。当值被传递给另一个变量或函数时，所有权会从一个所有者转移到另一个所有者。这个过程称为“所有权转移”或“移动”。

    ```rust
    let v1 = vec![1, 2, 3];
    let v2 = v1;  // 这里发生了所有权转移 
    ```

2. **避免浅拷贝和深拷贝：** 在传统的编程语言中，数据的复制可能会涉及浅拷贝（shallow copy）或深拷贝（deep copy）。浅拷贝只复制指针而不是实际数据，而深拷贝会复制整个数据。在Rust中，移动语义意味着所有权的转移，避免了昂贵的深拷贝操作，同时保持内存安全性。

3. **借用和引用：** 当我们不想转移所有权时，可以使用引用（reference）来借用数据的引用而不是所有权。Rust的借用规则确保了对数据的多个引用是安全的。

    ```rust
    fn print_vector(v: &Vec<i32>) {
        // // 函数接受对向量的引用。这里可以安全地借用v中的元素
        for &num in v {
            println!("{}", num);
        }
    }

    let numbers = vec![1, 2, 3];
    print_vector(&numbers);  // 传递引用而不是所有权。
    ```

4. **Copy trait：** 基本的整数类型（如 i32、u64）、字符串和布尔类型 bool 是内置实现 Copy trait 的类型。这意味着它们在进行赋值操作时会复制值，而不是移动所有权。如果你有一个包含这些类型的结构体或元组，只要这个结构体或元组的所有字段都实现了 Copy，那么这个结构体或元组也会自动实现 Copy。如果你的结构体或元组包含不实现 Copy 的字段，那么它将不再自动实现 Copy。

    ```rust
    let x = 5;  // 整数实现了 Copy trait.
    let y = x;  // 复制值而不是移动所有权。

    let s1 = String::from("Hello");
    let s2 = s1;  // 字符串的所有权被移动。

    println!("{}, world!", s1); // 错误！s1 不再有效。
    println!("{}", s2); // 正确。

    let s3 = "Hello";
    let s4 = s3; // 字符串实现了 Copy trait，所以没有移动。
    println!("{}, world!", s4); // 正确。
    ```

移动语义在Rust中提供了高效的内存管理，并通过防止悬垂引用（dangling references）等问题，增强了程序的安全性。合理利用移动语义可以避免不必要的内存拷贝，并帮助编写更高效、更安全的代码。

### 练习

- move_semantics1.rs
    ```rust
    #[test]
    fn main() {
        let vec0 = vec![22, 44, 66];

        let vec1 = fill_vec(vec0);

        assert_eq!(vec1, vec![22, 44, 66, 88]);
    }

    fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
        // let vec = vec;
        let mut vec = vec;  // 这里 vec 拥有 vec0 的所有权

        vec.push(88);

        vec
    }
    ```
- move_semantics2.rs
    ```rust
    #[test]
    fn main() {
        let vec0 = vec![22, 44, 66];

        // let mut vec1 = fill_vec(vec0);
        let vec1 = fill_vec(vec0.clone());  // 使用 clone 方法创建新的 Vec

        assert_eq!(vec0, vec![22, 44, 66]);
        assert_eq!(vec1, vec![22, 44, 66, 88]);
    }

    fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
        let mut vec = vec;

        vec.push(88);

        vec
    }
    ```
    
- move_semantics3.rs
    ```rust
    #[test]
    fn main() {
        let vec0 = vec![22, 44, 66];

        let mut vec1 = fill_vec(vec0);

        assert_eq!(vec1, vec![22, 44, 66, 88]);
    }

    fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {   //添加 mut， 这里 vec 拥有 vec0 的可变借用
        vec.push(88);

        vec
    }
    ```
- move_semantics4.rs
    ```rust
    #[test]
    fn main() {
        let vec0 = vec![22, 44, 66];

        let mut vec1 = fill_vec(vec0);

        assert_eq!(vec1, vec![22, 44, 66, 88]);
    }

    // `fill_vec()` no longer takes `vec: Vec<i32>` as argument - don't change this!
    fn fill_vec() -> Vec<i32> {
        // Instead, let's create and fill the Vec in here - how do you do that?
        // let mut vec = vec;

        // 直接在函数内创建并填充 Vec
        let mut vec = vec![22, 44, 66];

        vec.push(88);

        vec
    }
    ```
- move_semantics5.rs
    ```rust
    #[test]
    fn main() {
        let mut x = 100;
        let y = &mut x;
        // let z = &mut x;
        *y += 100;
        let z = &mut x;    // 只需要调整顺序即可，此处考察的是 Rust 的所有权模型和借用规则
                           // Rust 中的借用规则确保在任何给定时间，要么只能有一个可变引用，要么可以有多个不可变引用，但不能同时拥有可变引用和不可变引用。
        *z += 1000;
        assert_eq!(x, 1200);
    }
    ```
- move_semantics6.rs
    ```rust
        fn main() {
        let data = "Rust is great!".to_string();

        // get_char(data);
        // string_uppercase(&data);

        get_char(&data);
        string_uppercase(data);
    }

    // Should not take ownership
    fn get_char(data: &String) -> char {    // 加上 &，此处 data 参数被声明为不可变引用，因此不能被修改
        data.chars().last().unwrap()
    }

    // Should take ownership
    fn string_uppercase(mut data: String) {    // 去掉 &
        data = data.to_uppercase();   // 去掉 &

        println!("{}", data);
    }
    ```
## 07_structs

### 介绍

[Structures](https://kaisery.github.io/trpl-zh-cn/ch05-00-structs.html)

[Method-syntax](https://kaisery.github.io/trpl-zh-cn/ch05-03-method-syntax.html)

Rust 具有三种主要的结构类型，它们分别是经典的 C 结构、元组结构和单元结构。

1. **经典的 C 结构（Classic C Structs）**：
   - 类似于 C 语言中的结构体，允许你定义包含不同数据类型字段的自定义数据结构。
   - 使用 `struct` 关键字定义结构体，通过字段名访问结构体中的成员。

   ```rust
   struct Person {
       name: String,
       age: u32,
   }

   let john = Person {
       name: String::from("John"),
       age: 30,
   };

   println!("Name: {}, Age: {}", john.name, john.age);
   ```

2. **元组结构（Tuple Structs）**：
   - 与元组类似，但是有着命名的字段，可以为字段定义具体的类型。
   - 通过在 `struct` 关键字后添加字段的类型，创建一个类似元组的结构。

   ```rust
   struct Point(i32, i32);

   let origin = Point(0, 0);

   println!("X: {}, Y: {}", origin.0, origin.1);
   ```

3. **单元结构（Unit Structs）**：
   - 也被称为零字段结构，不包含任何字段。
   - 主要用于实现特定的 traits 或表示某种特定的信息。

   ```rust
   struct EmptyStruct;

   let empty_instance = EmptyStruct;
   // No fields to access
   ```

这些结构类型在 Rust 中用于创建自定义数据类型，使得代码更加模块化、可读性更强，并提供了强类型的静态检查。通过这些结构类型，Rust 支持灵活而强大的数据建模。
### 练习

- structs1.rs
    ```rust
    struct ColorClassicStruct {
        // TODO: Something goes here
        red: u8,
        green: u8,
        blue: u8,
    }

    // struct ColorTupleStruct(/* TODO: Something goes here */);
    struct ColorTupleStruct(u8, u8, u8);

    #[derive(Debug)]
    struct UnitLikeStruct;

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn classic_c_structs() {
            // TODO: Instantiate a classic c struct!
            // let green =
            let green = ColorClassicStruct {
                red: 0,
                green: 255,
                blue: 0,
            };

            assert_eq!(green.red, 0);
            assert_eq!(green.green, 255);
            assert_eq!(green.blue, 0);
        }

        #[test]
        fn tuple_structs() {
            // TODO: Instantiate a tuple struct!
            // let green =
            let green = ColorTupleStruct(0, 255, 0);

            assert_eq!(green.0, 0);
            assert_eq!(green.1, 255);
            assert_eq!(green.2, 0);
        }

        #[test]
        fn unit_structs() {
            // TODO: Instantiate a unit-like struct!
            // let unit_like_struct =
            let unit_like_struct = UnitLikeStruct;
            let message = format!("{:?}s are fun!", unit_like_struct);

            assert_eq!(message, "UnitLikeStructs are fun!");
        }
    }
    ```

- structs2.rs
    ```rust
    #[derive(Debug)]
    struct Order {
        name: String,
        year: u32,
        made_by_phone: bool,
        made_by_mobile: bool,
        made_by_email: bool,
        item_number: u32,
        count: u32,
    }

    fn create_order_template() -> Order {
        Order {
            name: String::from("Bob"),
            year: 2019,
            made_by_phone: false,
            made_by_mobile: false,
            made_by_email: true,
            item_number: 123,
            count: 0,
        }
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        fn your_order() {
            let order_template = create_order_template();
            // TODO: Create your own order using the update syntax and template above!
            // let your_order =
            assert_eq!(your_order.name, "Hacker in Rust");
            assert_eq!(your_order.year, order_template.year);
            assert_eq!(your_order.made_by_phone, order_template.made_by_phone);
            assert_eq!(your_order.made_by_mobile, order_template.made_by_mobile);
            assert_eq!(your_order.made_by_email, order_template.made_by_email);
            assert_eq!(your_order.item_number, order_template.item_number);
            assert_eq!(your_order.count, 1);
        }
    }
    ```

- structs3.rs
    ```rust
    #[derive(Debug)]
    struct Package {
        sender_country: String,
        recipient_country: String,
        weight_in_grams: u32,
    }

    impl Package {
        fn new(sender_country: String, recipient_country: String, weight_in_grams: u32) -> Package {
            if weight_in_grams < 10 {
                // This is not how you should handle errors in Rust,
                // but we will learn about error handling later.
                panic!("Can not ship a package with weight below 10 grams.")
            } else {
                Package {
                    sender_country,
                    recipient_country,
                    weight_in_grams,
                }
            }
        }

        fn is_international(&self) -> bool {
            // Something goes here...
            self.sender_country != self.recipient_country
        }

        fn get_fees(&self, cents_per_gram: u32) -> u32{
            // Something goes here...
            self.weight_in_grams * cents_per_gram
        }
    }

    #[cfg(test)]
    mod tests {
        use super::*;

        #[test]
        #[should_panic]
        fn fail_creating_weightless_package() {
            let sender_country = String::from("Spain");
            let recipient_country = String::from("Austria");

            Package::new(sender_country, recipient_country, 5);
        }

        #[test]
        fn create_international_package() {
            let sender_country = String::from("Spain");
            let recipient_country = String::from("Russia");

            let package = Package::new(sender_country, recipient_country, 1200);

            assert!(package.is_international());
        }

        #[test]
        fn create_local_package() {
            let sender_country = String::from("Canada");
            let recipient_country = sender_country.clone();

            let package = Package::new(sender_country, recipient_country, 1200);

            assert!(!package.is_international());
        }

        #[test]
        fn calculate_transport_fees() {
            let sender_country = String::from("Spain");
            let recipient_country = String::from("Spain");

            let cents_per_gram = 3;

            let package = Package::new(sender_country, recipient_country, 1500);

            assert_eq!(package.get_fees(cents_per_gram), 4500);
            assert_eq!(package.get_fees(cents_per_gram * 2), 9000);
        }
    }
    ```


## 08_enums

### 介绍

### 练习

## 09_strings

### 介绍

### 练习

## 10_modules

### 介绍

### 练习

## 11_hashmaps

## 12_options

## 13_error_handling

## 14_generics

## 15_traits

## 16_lifetimes

## 17_tests

## 18_iterators

## 19_smart_pointers

## 20_threads

## 21_macros

## 22_clippy

## 23_conversions

## quiz
