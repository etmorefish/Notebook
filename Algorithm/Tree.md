# 二叉树

「二叉树 binary tree」是一种非线性数据结构，代表“祖先”与“后代”之间的派生关系，体现了“一分为二”的分治逻辑。与链表类似，二叉树的基本单元是节点，每个节点包含值、左子节点引用和右子节点引用。



## 二叉树基本操作

### 初始化二叉树

与链表类似，首先初始化节点，然后构建引用（指针）。

### 插入与删除节点

与链表类似，在二叉树中插入与删除节点可以通过修改指针来实现。

需要注意的是，插入节点可能会改变二叉树的原有逻辑结构，而删除节点通常意味着删除该节点及其所有子树。因此，在二叉树中，插入与删除通常是由一套操作配合完成的，以实现有实际意义的操作。

## 二叉树遍历

###  层序遍历

「层序遍历 level-order traversal」从顶部到底部逐层遍历二叉树，并在每一层按照从左到右的顺序访问节点。

层序遍历本质上属于「广度优先遍历 breadth-first traversal」，也称「广度优先搜索 breadth-first search, BFS」，它体现了一种“一圈一圈向外扩展”的逐层遍历方式。

#####  代码实现 

广度优先遍历通常借助“队列”来实现。队列遵循“先进先出”的规则，而广度优先遍历则遵循“逐层推进”的规则，两者背后的思想是一致的。

##### 复杂度分析 

- **时间复杂度为 O(n)** ：所有节点被访问一次，使用 O(n) 时间，其中 n 为节点数量。
- **空间复杂度为O(n)** ：在最差情况下，即满二叉树时，遍历到最底层之前，队列中最多同时存在 (n+1)/2 个节点，占用 O(n)空间。



### 前序、中序、后序遍历

相应地，前序、中序和后序遍历都属于「深度优先遍历 depth-first traversal」，也称「深度优先搜索 depth-first search, DFS」，它体现了一种“先走到尽头，再回溯继续”的遍历方式。

**深度优先遍历就像是绕着整棵二叉树的外围“走”一圈**，在每个节点都会遇到三个位置，分别对应前序遍历、中序遍历和后序遍历。

#####   复杂度分析

- **时间复杂度为 O(n)** ：所有节点被访问一次，使用 O(n) 时间。
- **空间复杂度为 O(n)** ：在最差情况下，即树退化为链表时，递归深度达到n ，系统占用 O(n) 栈帧空间。

```rust
use std::cell::RefCell;
use std::collections::VecDeque;
use std::rc::Rc;

/* 二叉树节点结构体 */
struct TreeNode {
    val: i32,                             // 节点值
    left: Option<Rc<RefCell<TreeNode>>>,  // 左子节点引用
    right: Option<Rc<RefCell<TreeNode>>>, // 右子节点引用
}

impl TreeNode {
    /* 构造方法 */
    fn new(val: i32) -> Rc<RefCell<Self>> {
        Rc::new(RefCell::new(Self {
            val,
            left: None,
            right: None,
        }))
    }

    /* 层序遍历 */
    /// 广度优先遍历通常借助“队列”来实现。队列遵循“先进先出”的规则，
    /// 而广度优先遍历则遵循“逐层推进”的规则，两者背后的思想是一致的。
    fn level_order(root: &Rc<RefCell<TreeNode>>) -> Vec<i32> {
        // 初始化队列，加入根节点
        let mut que = VecDeque::new();
        que.push_back(Rc::clone(&root));
        // 初始化一个列表，用于保存遍历序列
        let mut vec = Vec::new();

        while let Some(node) = que.pop_front() {
            // 队列出队
            vec.push(node.borrow().val); // 保存节点值
            if let Some(left) = node.borrow().left.as_ref() {
                que.push_back(Rc::clone(left)); // 左子节点入队
            }
            if let Some(right) = node.borrow().right.as_ref() {
                que.push_back(Rc::clone(right)); // 右子节点入队
            };
        }
        vec
    }

    /* 前序遍历 */
    fn pre_order(root: Option<&Rc<RefCell<TreeNode>>>) -> Vec<i32> {
        let mut result = vec![];

        if let Some(node) = root {
            // 访问优先级：根节点 -> 左子树 -> 右子树
            result.push(node.borrow().val);
            result.append(&mut Self::pre_order(node.borrow().left.as_ref()));
            result.append(&mut Self::pre_order(node.borrow().right.as_ref()));
        }
        result
    }

    /* 中序遍历 */
    fn in_order(root: Option<&Rc<RefCell<TreeNode>>>) -> Vec<i32> {
        let mut result = vec![];

        if let Some(node) = root {
            // 访问优先级：左子树 -> 根节点 -> 右子树
            result.append(&mut Self::in_order(node.borrow().left.as_ref()));
            result.push(node.borrow().val);
            result.append(&mut Self::in_order(node.borrow().right.as_ref()));
        }
        result
    }

    /* 后序遍历 */
    fn post_order(root: Option<&Rc<RefCell<TreeNode>>>) -> Vec<i32> {
        let mut result = vec![];

        if let Some(node) = root {
            // 访问优先级：左子树 -> 右子树 -> 根节点
            result.append(&mut Self::post_order(node.borrow().left.as_ref()));
            result.append(&mut Self::post_order(node.borrow().right.as_ref()));
            result.push(node.borrow().val);
        }
        result
    }
}

fn main() {
    // 初始化节点
    let root = TreeNode::new(0);
    let n1 = TreeNode::new(1);
    let n2 = TreeNode::new(2);
    let n3 = TreeNode::new(3);
    let n4 = TreeNode::new(4);
    let n5 = TreeNode::new(5);
    // 构建节点之间的引用（指针）
    root.borrow_mut().left = Some(n1.clone());
    root.borrow_mut().right = Some(n2.clone());
    n1.borrow_mut().left = Some(n3);
    n1.borrow_mut().right = Some(n4);
    n2.borrow_mut().left = Some(n5.clone());

    let p = TreeNode::new(6);
    // 在 root -> n1 中间插入节点 P
    root.borrow_mut().left = Some(p.clone());
    p.borrow_mut().left = Some(n1.clone());
    println!("{:?}", TreeNode::level_order(&root));

    // 删除节点 p
    root.borrow_mut().left = Some(n1);

    println!("{:?}", TreeNode::pre_order(Some(&root)));
    println!("{:?}", TreeNode::in_order(Some(&root)));
    println!("{:?}", TreeNode::post_order(Some(&root)));
}

```

# 二叉树数组表示

##  表示完美二叉树

给定一棵完美二叉树，我们将所有节点按照层序遍历的顺序存储在一个数组中，则每个节点都对应唯一的数组索引。

根据层序遍历的特性，我们可以推导出父节点索引与子节点索引之间的“映射公式”：**若某节点的索引为 i ，则该节点的左子节点索引为 2i+1 ，右子节点索引为 2i+2** 。

**映射公式的角色相当于链表中的指针**。给定数组中的任意一个节点，我们都可以通过映射公式来访问它的左（右）子节点。

## 表示任意二叉树

完美二叉树是一个特例，在二叉树的中间层通常存在许多 `None` 。由于层序遍历序列并不包含这些 `None` ，因此我们无法仅凭该序列来推测 `None` 的数量和分布位置。**这意味着存在多种二叉树结构都符合该层序遍历序列**。

给定一棵非完美二叉树，上述数组表示方法已经失效。

为了解决此问题，**我们可以考虑在层序遍历序列中显式地写出所有 None** 。这样处理后，层序遍历序列就可以唯一表示二叉树了。

```rust
/* 二叉树的数组表示 */
// 使用 None 来标记空位
let tree = [Some(1), Some(2), Some(3), Some(4), None, Some(6), Some(7), Some(8), Some(9), None, None, Some(12), None, None, Some(15)];
```





```rust
/* 数组表示下的二叉树类 */
struct ArrayBinaryTree {
    tree: Vec<Option<i32>>,
}

impl ArrayBinaryTree {
    /* 构造方法 */
    fn new(arr: Vec<Option<i32>>) -> Self {
        Self { tree: arr }
    }

    /* 列表容量 */
    fn size(&self) -> i32 {
        self.tree.len() as i32
    }

    /* 获取索引为 i 节点的值 */
    fn val(&self, i: i32) -> Option<i32> {
        // 若索引越界，则返回 None ，代表空位
        if i < 0 || i >= self.size() {
            None
        } else {
            self.tree[i as usize]
        }
    }

    /* 获取索引为 i 节点的左子节点的索引 */
    fn left(&self, i: i32) -> i32 {
        2 * i + 1
    }

    /* 获取索引为 i 节点的右子节点的索引 */
    fn right(&self, i: i32) -> i32 {
        2 * i + 2
    }

    /* 获取索引为 i 节点的父节点的索引 */
    fn parent(&self, i: i32) -> i32 {
        (i - 1) / 2
    }

    /* 层序遍历 */
    fn level_order(&self) -> Vec<i32> {
        let mut res = vec![];
        // 直接遍历数组
        for i in 0..self.size() {
            if let Some(val) = self.val(i) {
                res.push(val)
            }
        }
        res
    }

    /* 深度优先遍历 */
    fn dfs(&self, i: i32, order: &str, res: &mut Vec<i32>) {
        if self.val(i).is_none() {
            return;
        }
        let val = self.val(i).unwrap();
        // 前序遍历
        if order == "pre" {
            res.push(val);
        }
        self.dfs(self.left(i), order, res);
        // 中序遍历
        if order == "in" {
            res.push(val);
        }
        self.dfs(self.right(i), order, res);
        // 后序遍历
        if order == "post" {
            res.push(val);
        }
    }

    /* 前序遍历 */
    fn pre_order(&self) -> Vec<i32> {
        let mut res = vec![];
        self.dfs(0, "pre", &mut res);
        res
    }

    /* 中序遍历 */
    fn in_order(&self) -> Vec<i32> {
        let mut res = vec![];
        self.dfs(0, "in", &mut res);
        res
    }

    /* 后序遍历 */
    fn post_order(&self) -> Vec<i32> {
        let mut res = vec![];
        self.dfs(0, "post", &mut res);
        res
    }
}

fn main() {
    /* 二叉树的数组表示 */
// 使用 None 来标记空位
    let arr = [Some(1), Some(2), Some(3), Some(4), None, Some(6), Some(7), Some(8), Some(9), None, None, Some(12), None, None, Some(15)];
    println!("arr: {:?}", arr);
    println!("\n初始化二叉树\n");
    println!("二叉树的数组表示：");
    println!(
        "[{}]",
        arr.iter()
            .map(|&val| if let Some(val) = val {
                format!("{val}")
            } else {
                "null".to_string()
            })
            .collect::<Vec<String>>()
            .join(", ")
    );

    // 数组表示下的二叉树类
    let t = ArrayBinaryTree::new(Vec::from(arr));


    // 访问节点
    let i = 1;
    let l = t.left(i);
    let r = t.right(i);
    let p = t.parent(i);
    println!(
        "\n当前节点的索引为 {} ，值为 {}",
        i,
        if let Some(val) = t.val(i) {
            format!("{val}")
        } else {
            "null".to_string()
        }
    );
    println!(
        "其左子节点的索引为 {} ，值为 {}",
        l,
        if let Some(val) = t.val(l) {
            format!("{val}")
        } else {
            "null".to_string()
        }
    );
    println!(
        "其右子节点的索引为 {} ，值为 {}",
        r,
        if let Some(val) = t.val(r) {
            format!("{val}")
        } else {
            "null".to_string()
        }
    );
    println!(
        "其父节点的索引为 {} ，值为 {}",
        p,
        if let Some(val) = t.val(p) {
            format!("{val}")
        } else {
            "null".to_string()
        }
    );

    // 遍历树
    let mut res = t.level_order();
    println!("\n层序遍历为：{:?}", res);
    res = t.pre_order();
    println!("前序遍历为：{:?}", res);
    res = t.in_order();
    println!("中序遍历为：{:?}", res);
    res = t.post_order();
    println!("后序遍历为：{:?}", res);
}
```

