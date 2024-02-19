# 2024春晚刘谦魔术的程序模拟

2024年春晚，刘谦的第二个魔术是一个复杂的自我工作技巧。魔术的基本流程包括以下几个步骤：
```
1、每个人拿起4张牌。eg：ABCD。从中间撕开变成8张牌并叠在一起。eg：ABCDABCD
2、根据自己名字有几个字，从牌顶放几张牌到牌底。eg：以名字两个字为例，CDABCDAB，但无论怎么操作，第4张和第8张牌都是一样的。
3、拿起牌顶三张牌插入牌的中间。这一步非常重要！因为操作完之后必然出现第1张和第8张牌是一样的！eg:以名字两个字为例，可以写成B??????B（这里的?是其他和B不同的牌）。
4、记住此时的顶牌，把牌顶放到旁边。此时顶牌为B，剩下的序列是??????B，一共7张牌。
5、南方人把牌顶1张牌放入牌中间任意位置，北方人放2张，不知道放3张。但不会改变剩下的序列是??????B的结果。
6、男生丢掉一张顶牌，女生丢两张。也就是男生剩下6张，女生剩下5张。分别是?????B和????B。
7、按照见证奇迹的时刻从牌顶放7张牌到牌底。此时男生女生的牌分别是是????B?和??B??。
8、此时放一张顶牌到牌底，再丢一张顶牌。执行约瑟夫环过程！最后剩下的一张牌和第5步记住的牌一样。当牌数为6时（男生），剩下的就是第5张牌；当牌数为5时（女生），剩下的就是第3张牌。就是第4步拿掉的那张牌！
```
这个魔术主要运用了三个数学性质：周期为4的环旋转不变性、1+6x和2+5y的最小公倍数以及长度为5和6的约瑟夫环。其中，约瑟夫环是一个在计算机编程中常见的问题，用于解释魔术的第一步。整个魔术通过这些数学原理的巧妙运用，达到了令人惊叹的效果。

## 什么是约瑟夫环问题？

约瑟夫环问题（Josephus problem）是一个著名的数学问题，以古犹太历史学家约瑟夫·弗拉维乌斯的名字命名。这个问题可以描述为：设有编号为1到n的n个人围坐一圈，从第一个人开始报数，每数到m个人，该人出列，然后从下一个人重新开始报数，如此循环，直到所有人都出列。问题是：哪些位置上的人会最后出列，以及他们的出列顺序是什么？

约瑟夫环问题可以通过递归、迭代或使用数学公式来解决。一个著名的解法是使用同余运算。给定人数n和报数m，可以构造一个序列来表示每个人的出列顺序。例如，当n=10，m=3时，出列的顺序是3, 6, 9, 2, 7, 1, 8, 5, 10, 4。

这个问题不仅在数学和计算机科学中有教育意义，而且在密码学、计算机编程竞赛等领域也有实际应用。在刘谦的魔术中，约瑟夫环问题被用作某种数学原理，来保证魔术的特定效果。


下面是完整的 Python 代码实现：
```python
import random


def move_card_back(n, arr):
    """把牌堆顶n张牌移动到末尾"""
    for i in range(n):
        move_card = arr.pop(0)
        arr.append(move_card)
    return arr


def move_card_middle_random(n, arr):
    """把牌堆顶n张牌移动到中间的任意位置"""
    idx = random.randint(n + 1, len(arr) - 1)
    new_arr = arr[n:idx] + arr[0:n] + arr[idx:]
    return new_arr


# 步骤1：初始化4张牌，假设为"ABCD"
arr = ["A", "B", "C", "D", "A", "B", "C", "D"]
print("步骤1：拿出4张牌，对折撕成8张，按顺序叠放。")
print(f"此时序列为：{''.join(arr)} \n-----")

# 步骤2（非关步骤）：名字长度随机选取，这里取2到5（其实任意整数都行）
name_len = random.randint(2, 5)
move_card_back(name_len, arr)
print(f"步骤2：随机选取名字长度为{name_len}，把第1张牌放到末尾，操作{name_len}次。")
print(f"此时序列为：{''.join(arr)}\n-----")

# 步骤3（关键步骤）：把牌堆顶三张放到中间任意位置
arr = move_card_middle_random(3, arr)
print("步骤3：把牌堆顶3张放到中间的随机位置。")
print(f"此时序列为：{''.join(arr)}\n-----")

# 步骤4（关键步骤）：把最顶上的牌拿走
rest_card = arr.pop(0)
print("步骤4：把最顶上的牌拿走，放在一边。")
print(f"拿走的牌为：{rest_card}")
print(f"此时序列为：{''.join(arr)}\n-----")

# 步骤5（非关步骤）：南方人把牌顶1张牌放入牌中间任意位置，北方人放2张，不知道放3张
# 随机选择1、2、3中的任意一个数字
move_num = random.randint(1, 3)
arr = move_card_middle_random(move_num, arr)
print(
    f"步骤5：我{'是南方人' if move_num == 1 else '是北方人' if move_num == 2 else '不确定自己是哪里人'}，把{move_num}张牌插入到中间的随机位置。")
print(f"此时序列为：{''.join(arr)}\n-----")

# 步骤6（关键步骤）：根据性别男或女，移除牌堆顶的1或2张牌
male_num = random.randint(1, 2)
for i in range(male_num):
    arr.pop(0)
print(f"步骤6：我是{'男生' if male_num == 1 else '女生'}，移除牌堆顶的{male_num}张牌。")
print(f"此时序列为：{''.join(arr)}\n-----")

# 步骤7（关键步骤）：把顶部的牌移动到末尾，执行7次
for i in range(7):
    move_card = arr.pop(0)
    arr.append(move_card)
print("步骤7：把顶部的牌移动到末尾，执行7次")
print(f"此时序列为：{''.join(arr)}\n-----")

# 步骤8（关键步骤）：执行约瑟夫环过程。把牌堆顶一张牌放到末尾，再移除一张牌，直到只剩下一张牌。
print("步骤8：把牌堆顶一张牌放到末尾，再移除一张牌，直到只剩下一张牌。")
while len(arr) > 1:
    luck = arr.pop(0)
    arr.append(luck)
    print(f"好运留下来：{luck}\t\t此时序列为：{''.join(arr)}")
    sadness = arr.pop(0)
    print(f"烦恼都丢掉：{sadness}\t\t此时序列为：{''.join(arr)}")
print(f"-----\n最终结果：剩下的牌为 {arr[0]}，步骤4中留下来的牌也是{rest_card}")

```
out:
```shell
步骤1：拿出4张牌，对折撕成8张，按顺序叠放。
此时序列为：ABCDABCD 
-----
步骤2：随机选取名字长度为4，把第1张牌放到末尾，操作4次。
此时序列为：ABCDABCD
-----
步骤3：把牌堆顶3张放到中间的随机位置。
此时序列为：DABCABCD
-----
步骤4：把最顶上的牌拿走，放在一边。
拿走的牌为：D
此时序列为：ABCABCD
-----
步骤5：我是南方人，把1张牌插入到中间的随机位置。
此时序列为：BCAABCD
-----
步骤6：我是男生，移除牌堆顶的1张牌。
此时序列为：CAABCD
-----
步骤7：把顶部的牌移动到末尾，执行7次
此时序列为：AABCDC
-----
步骤8：把牌堆顶一张牌放到末尾，再移除一张牌，直到只剩下一张牌。
好运留下来：A		此时序列为：ABCDCA
烦恼都丢掉：A		此时序列为：BCDCA
好运留下来：B		此时序列为：CDCAB
烦恼都丢掉：C		此时序列为：DCAB
好运留下来：D		此时序列为：CABD
烦恼都丢掉：C		此时序列为：ABD
好运留下来：A		此时序列为：BDA
烦恼都丢掉：B		此时序列为：DA
好运留下来：D		此时序列为：AD
烦恼都丢掉：A		此时序列为：D
-----
最终结果：剩下的牌为 D，步骤4中留下来的牌也是D
```

Rust 代码实现：
> https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=0c86e4410e121635dc1f140be7d17753
```rust
use rand::Rng;

fn move_card_back(n: usize, arr: &mut Vec<char>) {
    for _ in 0..n {
        let move_card = arr.remove(0);
        arr.push(move_card);
    }
}

fn move_card_middle_random(n: usize, arr: &mut Vec<char>) {
    let len = arr.len();
    let idx = rand::thread_rng().gen_range(n + 1..len);
    let mut new_arr = arr[n..idx].to_vec();
    new_arr.extend_from_slice(&arr[0..n]);
    new_arr.extend_from_slice(&arr[idx..]);
    *arr = new_arr;
}

fn main() {
    let mut arr = vec!['A', 'B', 'C', 'D', 'A', 'B', 'C', 'D'];
    println!("步骤1：拿出4张牌，对折撕成8张，按顺序叠放。");
    println!("此时序列为：{}", arr.iter().collect::<String>());

    let name_len = rand::thread_rng().gen_range(2..=5);
    move_card_back(name_len, &mut arr);
    println!("步骤2：随机选取名字长度为{}，把第1张牌放到末尾，操作{}次。", name_len, name_len);
    println!("此时序列为：{}", arr.iter().collect::<String>());

    move_card_middle_random(3, &mut arr);
    println!("步骤3：把牌堆顶3张放到中间的随机位置。");
    println!("此时序列为：{}", arr.iter().collect::<String>());

    let rest_card = arr.remove(0);
    println!("步骤4：把最顶上的牌拿走，放在一边。");
    println!("拿走的牌为：{}", rest_card);
    println!("此时序列为：{}", arr.iter().collect::<String>());

    let move_num = rand::thread_rng().gen_range(1..=3);
    move_card_middle_random(move_num, &mut arr);
    println!("步骤5：我{}，把{}张牌插入到中间的随机位置。", if move_num == 1 { "是南方人" } else if move_num == 2 { "是北方人" } else { "不确定自己是哪里人" }, move_num);
    println!("此时序列为：{}", arr.iter().collect::<String>());

    let male_num = rand::thread_rng().gen_range(1..=2);
    for _ in 0..male_num {
        arr.remove(0);
    }
    println!("步骤6：我是{}，移除牌堆顶的{}张牌。", if male_num == 1 { "男生" } else { "女生" }, male_num);
    println!("此时序列为：{}", arr.iter().collect::<String>());

    for _ in 0..7 {
        let move_card = arr.remove(0);
        arr.push(move_card);
    }
    println!("步骤7：把顶部的牌移动到末尾，执行7次");
    println!("此时序列为：{}", arr.iter().collect::<String>());

    println!("步骤8：把牌堆顶一张牌放到末尾，再移除一张牌，直到只剩下一张牌。");
    while arr.len() > 1 {
        let luck = arr.remove(0);
        arr.push(luck);
        println!("好运留下来：{}\t此时序列为：{}", luck, arr.iter().collect::<String>());

        let sadness = arr.remove(0);
        println!("烦恼都丢掉：{}\t此时序列为：{}", sadness, arr.iter().collect::<String>());
    }
    println!("最终结果：剩下的牌为 {}，步骤4中留下来的牌也是{}", arr[0], rest_card);
}
```

在Rust中，我们使用了Vec<char>来存储扑克牌，并使用remove和push方法来移动牌。rand crate用于生成随机数。这段代码应该能够正确地模拟魔术中的步骤。在运行之前，请确保您已经添加了rand crate到您的Cargo.toml文件中：
```toml
[dependencies]
rand = "0.8.5"
```