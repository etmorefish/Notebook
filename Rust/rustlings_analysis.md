# Rustlings Analysis

## 00_intro

### 介绍
学会使用`print!` 和 `println!`宏打印字符串到标准输出。

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
- functions4.rs
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