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

###### [变量与可变性](https://kaisery.github.io/trpl-zh-cn/ch03-01-variables-and-mutability.html)

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
      let x == 0;  // 不能直接设置一个空变量
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
if是最基本的控制流结构，它允许根据条件来执行不同的代码块。

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

## 05_vecs

## 06_move_semantics

## 07_structs

## 08_enums

## 09_strings

## 10_modules

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



```

```