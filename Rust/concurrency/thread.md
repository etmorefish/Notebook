# Chapter 1: 线程 (Threads)



![](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/sm-thread.png)

![](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/sm-thread-1.png)

在Rust中，线程是实现并发的基本单位，为程序提供了一种同时执行多个任务的机制。本章将深入介绍Rust中线程的使用，涵盖标准库提供的基本线程操作以及一些强大的第三方库，帮助你更好地理解并利用线程进行并发编程。

### 1.1 标准库中的线程

#### 1.1.1 线程创建与执行

Rust标准库提供了`std::thread`模块，使得线程的创建和执行变得相对简单。我们将学习如何使用`std::thread::spawn`函数创建新线程，并探讨线程的执行过程。一个 Rust 程序执行的会启动一个进程，这个进程会包含一个或者多个线程，Rust 中的线程是纯操作的系统的线程，拥有自己的栈和状态。

```rust
use std::thread;

fn main() {
    // 创建新线程并执行任务
    let handle = thread::spawn(|| {
        // 线程执行的代码
        println!("Hello from a new thread!");
    });

    // 等待新线程执行完成
    handle.join().unwrap();
}
```

#### 1.1.2 线程间通信

线程之间的通信是并发编程中一个关键的问题。通过引入`std::sync`模块，我们可以使用`Mutex`和`Arc`等同步原语来实现线程之间的安全数据共享。

```rust
use std::sync::{Mutex, Arc};
use std::thread;

fn main() {
    // 共享数据
    let counter = Arc::new(Mutex::new(0));

    // 创建多个线程共享数据
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }

    // 等待所有线程执行完成
    for handle in handles {
        handle.join().unwrap();
    }

    // 打印共享数据
    println!("Result: {}", *counter.lock().unwrap());
}
```

### 1.2 第三方库中的线程

Rust生态系统中有许多优秀的第三方库，为线程管理和并发问题提供了更高级的解决方案。在这一节，我们将介绍一些流行的库，如`rayon`和`crossbeam`，它们能够简化并发编程的复杂性，提高开发效率。

#### 1.2.1 Rayon

`rayon`是一个并行计算库，通过简单的API使得在集合上执行并行操作变得轻而易举。我们将深入研究如何使用`rayon`来加速迭代操作，实现高效的数据并行处理。

```rust
use rayon::prelude::*;

fn main() {
    let data = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

    // 使用Rayon进行并行迭代
    let sum: i32 = data.par_iter().map(|&x| x * x).sum();

    println!("Sum of squares: {}", sum);
}
```

#### 1.2.2 Crossbeam

`crossbeam`是一个提供先进同步原语的库，它使得在Rust中进行复杂的并发编程变得更加容易。我们将学习如何使用`crossbeam`来创建高性能的并发数据结构和进行无锁编程。

```rust
use crossbeam::channel;

fn main() {
    // 创建无锁通道
    let (sender, receiver) = channel::unbounded();

    // 多线程间通信
    let handle = std::thread::spawn(move || {
        sender.send("Hello from another thread!").unwrap();
    });

    // 接收消息
    let message = receiver.recv().unwrap();
    println!("{}", message);

    // 等待线程执行完成
    handle.join().unwrap();
}
```

通过深入学习这些内容，你将更好地掌握Rust中线程的使用方法，能够在实际项目中有效地利用并发编程的优势。在下一章节，我们将进一步探讨线程池的概念及其在Rust中的应用。