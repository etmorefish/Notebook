# rustlings 🦀❤️

该项目包含一些小练习，可帮助您习惯阅读和编写 Rust 代码。这包括阅读和响应编译器消息！

对于初次学习 Rust 的人来说，还有其他一些资源：

- 这本书[en]([The Rust Programming Language - The Rust Programming Language (rust-lang.org)](https://doc.rust-lang.org/book/index.html)) [cn]([Rust 程序设计语言 - Rust 程序设计语言 简体中文版 (kaisery.github.io)](https://kaisery.github.io/trpl-zh-cn/title-page.html))- 学习 Rust 最全面的资源，但有时有点理论性。您将与 Rustlings 一起使用它！

- [Introduction - Rust By Example (rust-lang.org)](https://doc.rust-lang.org/rust-by-example/index.html) - 通过解决小练习来学习 Rust！

- [Rust语言圣经(Rust Course)]([关于本书 - Rust语言圣经(Rust Course)](https://course.rs/about-book.html)) - 不可多得的中文资料
- [Rust 语言实战](https://github.com/sunface/rust-by-practice), Rust 语言圣经配套习题，支持中英双语，可以在右上角切换

## Getting Started

> 您需要安装 Rust。您可以通过访问 https://rustup.rs 获取它。这还将安装 Cargo，Rust 的包/项目管理器。

## MacOS/Linux

```bash
curl -L https://raw.githubusercontent.com/rust-lang/rustlings/main/install.sh | bash
```

这将安装 Rustlings 并允许您访问 `rustlings` 命令。运行它即可开始！

```bash
╭─ubuntu@VM-4-6-ubuntu in ~
╰$ rustlings

       welcome to...
                 _   _ _
  _ __ _   _ ___| |_| (_)_ __   __ _ ___
 | '__| | | / __| __| | | '_ \ / _` / __|
 | |  | |_| \__ \ |_| | | | | | (_| \__ \
 |_|   \__,_|___/\__|_|_|_| |_|\__, |___/
                               |___/

/home/ubuntu/.cargo/bin/rustlings must be run from the rustlings directory
Try `cd rustlings/`!
```



## Doing exercises

练习按主题排序，可以在子目录 `rustlings/exercises/<topic>` 中找到。对于每个主题，都有一个附加的自述文件，其中包含一些资源，可帮助您开始了解该主题。我们强烈建议您在开始之前先看一下它们。

任务很简单。大多数练习都包含一个错误，导致它们无法编译，您需要修复它！有些练习也作为测试运行，但 rustles 仍然可以处理它们。要按照建议的顺序运行练习，请执行：

```bash
rustlings watch
```

这将尝试按预定顺序验证每个练习的完成情况（我们认为这对新手来说是最好的）。每次您更改 `exercises/` 目录中的文件时，它也会自动重新运行。如果您只想运行一次，可以使用：

```bash
rustlings verify
```

这与 watch 的作用相同，但运行后会退出。

如果您想按照自己的顺序进行，或者只想验证单个练习，您可以运行：

```bash
rustlings run myExercise1
```

您还可以使用以下命令获取下一个未解决的练习的提示：

```bash
rustlings hint next
```

要检查进度，您可以运行以下命令：

```bash
rustlings list
```

## Testing yourself

每几个部分之后，都会有一个测验，一次测试您对多个部分的知识。这些测验可在 `exercises/quizN.rs` 中找到。

## Enabling `rust-analyzer`

运行命令 `rustlings lsp` ，这将在项目的根目录生成一个 `rust-project.json` ，这允许 rust-analyzer 解析每个练习。

## Uninstalling Rustlings

如果您想从系统中删除 Rustlings，有两个步骤。首先，您需要删除安装脚本为您创建的练习文件夹：

```bash
rm -rf rustlings # or your custom folder name, if you chose and or renamed it
```

其次，运行 `cargo uninstall` 以删除 `rustlings` 二进制文件：

```bash
cargo uninstall rustlings
```

现在你应该完成了！