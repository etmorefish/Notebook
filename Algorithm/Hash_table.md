# 哈希表

「哈希表 hash table」，又称「散列表」，其通过建立键 `key` 与值 `value` 之间的映射，实现高效的元素查询。具体而言，我们向哈希表输入一个键 `key` ，则可以在 O(1) 时间内获取对应的值 `value` 。

除哈希表外，数组和链表也可以实现查询功能，它们的效率对比如表下。

|          | 数组 | 链表 | 哈希表 |
| :--------: | :--: | :--: | :----: |
| 查找元素 | O(n) | O(n) |  O(1)  |
| 添加元素 | O(1) | O(1) |  O(1)  |
| 删除元素 | O(n) | O(n) |  O(1)  |

观察发现，**在哈希表中进行增删查改的时间复杂度都是 O(1)** ，非常高效。

## 哈希表常用操作

哈希表的常见操作包括：初始化、查询操作、添加键值对和删除键值对等，同时还有三种常用的遍历方式：遍历键值对、遍历键和遍历值。

## 哈希表简单实现

我们先考虑最简单的情况，**仅用一个数组来实现哈希表**。在哈希表中，我们将数组中的每个空位称为「桶 bucket」，每个桶可存储一个键值对。因此，查询操作就是找到 `key` 对应的桶，并在桶中获取 `value` 。

那么，如何基于 `key` 定位对应的桶呢？这是通过「哈希函数 hash function」实现的。哈希函数的作用是将一个较大的输入空间映射到一个较小的输出空间。在哈希表中，输入空间是所有 `key` ，输出空间是所有桶（数组索引）。换句话说，输入一个 `key` ，**我们可以通过哈希函数得到该 key 对应的键值对在数组中的存储位置**。

输入一个 `key` ，哈希函数的计算过程分为以下两步。

1. 通过某种哈希算法 `hash()` 计算得到哈希值。
2. 将哈希值对桶数量（数组长度）`capacity` 取模，从而获取该 `key` 对应的数组索引 `index` 。

```
index = hash(key) % capacity
```

随后，我们就可以利用 `index` 在哈希表中访问对应的桶，从而获取 `value` 。

设数组长度 `capacity = 100`、哈希算法 `hash(key) = key` ，易得哈希函数为 `key % 100` 

![](https://cdn.jsdelivr.net/gh/etmorefish/picbed@main/hash_table1.png)





hash_table.rs

```
/* 键值对 */
#[derive(Debug, Clone, PartialEq)]
pub struct Pair {
    pub key: i32,
    pub val: String,
}

/* 基于数组实现的哈希表 */
pub struct ArrayHashMap {
    buckets: Vec<Option<Pair>>,
}

impl ArrayHashMap {
    pub fn new() -> ArrayHashMap {
        // 初始化数组，包含 100 个桶
        Self {
            buckets: vec![None; 100],
        }
    }

    /* 哈希函数 */
    fn hash_func(&self, key: i32) -> usize {
        key as usize % 100
    }

    /* 查询操作 */
    pub fn get(&self, key: i32) -> Option<&String> {
        let index = self.hash_func(key);
        self.buckets[index].as_ref().map(|pair| &pair.val)
    }

    /* 添加操作 */
    pub fn put(&mut self, key: i32, val: &str) {
        let index = self.hash_func(key);
        self.buckets[index] = Some(Pair {
            key,
            val: val.to_string(),
        });
    }

    /* 删除操作 */
    pub fn remove(&mut self, key: i32) {
        let index = self.hash_func(key);
        // 置为 None ，代表删除
        self.buckets[index] = None;
    }

    /* 获取所有键值对 */
    pub fn entry_set(&self) -> Vec<&Pair> {
        self.buckets
            .iter()
            .filter_map(|pair| pair.as_ref())
            .collect()
    }

    /* 获取所有键 */
    pub fn key_set(&self) -> Vec<&i32> {
        self.buckets
            .iter()
            .filter_map(|pair| pair.as_ref().map(|pair| &pair.key))
            .collect()
    }

    /* 获取所有值 */
    pub fn value_set(&self) -> Vec<&String> {
        self.buckets
            .iter()
            .filter_map(|pair| pair.as_ref().map(|pair| &pair.val))
            .collect()
    }

    /* 打印哈希表 */
    pub fn print(&self) {
        for pair in self.entry_set() {
            println!("{} -> {}", pair.key, pair.val);
        }
    }
}

fn main() {
    let mut hash_table = ArrayHashMap::new();
    hash_table.put(1, "hello");
    hash_table.put(2, "world");
    hash_table.put(3, "rust");
    hash_table.put(4, "hello");
    hash_table.put(5, "world");
    hash_table.put(6, "rust");
    hash_table.print();
    println!("{:?}", hash_table.get(1));
    hash_table.remove(1);
    hash_table.print();
    println!("{:?}", hash_table.key_set());
    println!("{:?}", hash_table.value_set());
}

```



hash_algo.rs

```
/* 加法哈希 */
fn add_hash(key: &str) -> i32 {
    let mut hash = 0_i64;
    const MODULUS: i64 = 1000000007;

    for c in key.chars() {
        hash = (hash + c as i64) % MODULUS;
    }

    hash as i32
}

/* 乘法哈希 */
fn mul_hash(key: &str) -> i32 {
    let mut hash = 0_i64;
    const MODULUS: i64 = 1000000007;

    for c in key.chars() {
        hash = (31 * hash + c as i64) % MODULUS;
    }

    hash as i32
}

/* 异或哈希 */
fn xor_hash(key: &str) -> i32 {
    let mut hash = 0_i64;
    const MODULUS: i64 = 1000000007;

    for c in key.chars() {
        hash ^= c as i64;
    }

    (hash & MODULUS) as i32
}

/* 旋转哈希 */
fn rot_hash(key: &str) -> i32 {
    let mut hash = 0_i64;
    const MODULUS: i64 = 1000000007;

    for c in key.chars() {
        hash = ((hash << 4) ^ (hash >> 28) ^ c as i64) % MODULUS;
    }

    hash as i32
}

fn main() {
    println!("{:?}", add_hash("abc"));
    println!("{:?}", mul_hash("abc"));
    println!("{:?}", xor_hash("abc"));
    println!("{:?}", rot_hash("abc"));
}

```

