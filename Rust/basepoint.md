`filter_map` 和 `map` 是 Rust 标准库中的两个迭代器适配器（iterator adapter）。它们的功能有一些相似之处，但主要区别在于对元素的选择和过滤方式。

1. **`map`：**
   - `map` 用于将一个迭代器中的每个元素按照指定的闭包函数进行转换，并返回一个新的迭代器。该闭包函数的返回值决定了新迭代器中的每个元素。
   - `map` 并不会过滤掉任何元素，它仅仅对每个元素进行映射操作。

   示例：
   ```rust
   let numbers = vec![1, 2, 3, 4];
   let squared: Vec<i32> = numbers.iter().map(|&x| x * x).collect();
   // squared 现在是 [1, 4, 9, 16]
   ```

2. **`filter_map`：**
   - `filter_map` 组合了 `filter` 和 `map` 的功能。它首先对迭代器进行过滤，然后对每个符合条件的元素应用一个闭包进行转换，最终返回一个新的迭代器。
   - 闭包函数返回 `Option<T>` 类型，`filter_map` 会过滤掉那些返回 `None` 的元素，只保留返回 `Some(T)` 的元素。

   示例：
   ```rust
   let numbers = vec![Some(1), Some(2), None, Some(3), None, Some(4)];
   let squared: Vec<i32> = numbers.into_iter().filter_map(|x| x.map(|y| y * y)).collect();
   // squared 现在是 [1, 4, 9, 16]
   ```

总体来说，`map` 主要用于映射元素，而 `filter_map` 则用于在映射的同时进行过滤，且对于不满足条件的元素可以直接过滤掉。选择使用哪个取决于你的需求，如果你需要在映射的同时过滤一些元素，那么 `filter_map` 可能更适合。