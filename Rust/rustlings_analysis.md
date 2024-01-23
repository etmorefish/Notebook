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
- variables1.rs
  ```rust
  fn main() {
    let x = 5;   // 使用let声明变量
    println!("x has the value {}", x);
    }
  ```
- variables2.rs
  ```rust
    fn main() {
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
        let x: i32;
        println!("Number {}", x);
    }
  ```
- variables4.rs
    ```rust
    fn main() {
        let x = 3;
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
      number = 3; // don't rename this variable
      println!("Number plus two is : {}", number + 2);
  }
  ```
- variables6.rs
  ```rust
  // 声明常量的时候需要添加上对应的类型
  const NUMBER: i32 = 3;
  fn main() {
      println!("Number {}", NUMBER);
  }

  ```


## 02_functions
### 介绍
学习如何定义和使用函数。

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


