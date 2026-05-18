# Lecture 21: Advanced Tree Structures - Exam Notes

> 范围：`Lecture/21_Advanced_Tree_Structures.pdf`，主要是 `AVL Tree` 和 `Heap Tree`。
> 读法：先看第 0 节速查，再重点看第 3 节四种 AVL rotation。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### AVL Tree 必背

| 问题 | 答案 |
|---|---|
| AVL Tree 是什么？ | `self-balancing binary search tree` |
| AVL 比普通 BST 多了什么条件？ | 每个 node 的 left/right subtree heights 相差最多 `1` |
| empty tree height | `-1` |
| leaf / single node height | `0` |
| 为什么需要 AVL？ | 普通 BST 可能退化成 linked list，search 从 `O(log n)` 退化到 `O(n)` |
| AVL search / insert / delete | `O(log n)` |
| 什么时候 rotate？ | 插入后某个 node 的 left/right height difference 变成 `2` |
| 同侧不平衡 | `LL` / `RR`，用 `single rotation` |
| 异侧不平衡 | `LR` / `RL`，用 `double rotation` |

### 四种 rotation 总表

| Case | 新 node 在哪里？ | 操作 | 记忆 |
|---|---|---|---|
| `LL` | `alpha.left.left` | right rotation at `alpha` | 左边的左边太高，向右转 |
| `RR` | `alpha.right.right` | left rotation at `alpha` | 右边的右边太高，向左转 |
| `LR` | `alpha.left.right` | left rotation at `alpha.left`，再 right rotation at `alpha` | 先把弯折拉直，再单转 |
| `RL` | `alpha.right.left` | right rotation at `alpha.right`，再 left rotation at `alpha` | 先把弯折拉直，再单转 |

`alpha` = 从 inserted node 往上走时，第一个变得不平衡的 node。

### Heap 必背

| 问题 | 答案 |
|---|---|
| Heap 是什么？ | `complete binary tree` + `heap property` |
| Complete binary tree | 除最后一层外每层填满；最后一层从左到右填 |
| Min-heap property | parent value `<=` child value |
| min-heap root | whole heap 的 minimum |
| access minimum | `O(1)` |
| remove minimum | `O(log n)` |
| insert new value | `O(log n)` |
| heap sort | `O(n log n)` |
| array left child | `arr[2*i + 1]` |
| array right child | `arr[2*i + 2]` |
| array parent | `arr[(i - 1) / 2]` |

---

## 1. 前置：Tree / BST 只保留考试必要部分

### 1.1 Tree

Tree 是一种 `non-linear data structure`，由 `nodes` 和 `edges` 组成。

关键性质：

- 任意两个 nodes 之间只有一条 path。
- 如果 tree 有 `N` 个 nodes，就有 `N - 1` 条 links/edges。
- General tree 中，一个 node 可以有任意多个 children。

### 1.2 Binary Tree

Binary tree 是每个 node 最多有两个 children 的 tree：

- `left child`
- `right child`

常见术语：

| Term | 中文理解 |
|---|---|
| `root` | 最上面的 node |
| `parent` | 某个 node 的上一级 |
| `children` | 某个 node 的下一级 |
| `siblings` | 同一个 parent 的 children |
| `leaf` | 没有 child 的 node |
| `internal node` | 有一个或两个 children 的 node |
| `subtree` | 某个 node 和它下面所有 descendants 组成的 tree |
| `ancestor` | 从 node 往 root 走经过的上层 nodes |
| `descendant` | 在某个 node 的 subtree 里面的 nodes |
| `height` | 从 root 到 leaf 的最长 path 上的 edge 数 |

### 1.3 Traversal

| Traversal | 顺序 | 记忆 |
|---|---|---|
| `Preorder` | Root, Left, Right | root 先访问 |
| `Inorder` | Left, Root, Right | root 在中间 |
| `Postorder` | Left, Right, Root | root 最后访问 |

BST 里最重要的是：

```text
Inorder traversal of BST gives ascending order.
```

### 1.4 BST / Ordered Binary Tree

`Binary Search Tree (BST)` 的规则：

```text
left subtree values <= current node value
right subtree values > current node value
```

插入 `insert(x)`：

1. 如果 root 是 `null`，`x` 成为 root。
2. 如果 `x <= current.value`，去 left subtree。
3. 如果 `x > current.value`，去 right subtree。
4. 一直走到空位置，把 `x` 放进去。

删除 `delete(x)`：

| 被删 node 情况 | 做法 |
|---|---|
| leaf | 直接删除 |
| 只有一个 child | child 顶上来 |
| 有两个 children | 找 right subtree 中最左边的 node，也就是 successor，用它替换 |

为什么 AVL 要先懂 BST？
因为 AVL insertion 仍然先按普通 BST 规则插入，只是在插入后检查 balance，不平衡才 rotation。

---

## 2. AVL Tree 核心概念

### 2.1 为什么普通 BST 不够？

普通 BST 的效率取决于 tree height。

如果插入顺序很差，例如：

```text
insert 1, 2, 3, 4, 5

1
 \
  2
   \
    3
     \
      4
       \
        5
```

这棵 BST 看起来像 linked list：

- search `5` 要走 5 个 nodes。
- height 约等于 `n`。
- search/insert/delete 最坏可以是 `O(n)`。

AVL Tree 的目的：

```text
通过 rotation 自动保持 tree balanced，
让 height 保持在 O(log n)，
所以 search/insert/delete 都保持 O(log n)。
```

### 2.2 AVL Tree 定义

AVL Tree 是：

```text
self-balancing binary search tree
```

也就是说它必须同时满足：

1. 它是 BST：left 小，right 大。
2. 它是 balanced：对每个 node，left subtree height 和 right subtree height 的差值最多为 `1`。

课程里的 height 定义：

```text
empty tree height = -1
single node height = 0
node height = max(left height, right height) + 1
```

可以用一个辅助概念帮助判断：

```text
balance difference = abs(height(left subtree) - height(right subtree))
AVL valid if balance difference <= 1 at every node.
```

### 2.3 判断是否 AVL 的例子

例子 A：

```text
        5
       / \
      3   9
     / \
    1   4
```

检查：

- Node `1`, `4`, `9` 都是 leaf，height = `0`。
- Node `3` 的 left height = `0`，right height = `0`，difference = `0`。
- Node `5` 的 left subtree height = `1`，right subtree height = `0`，difference = `1`。

所以这是 AVL。

例子 B：

```text
        5
       / \
      3   9
     / \
    1   4
     \
      2
```

检查关键 node：

- Node `1` 的 right child 是 `2`，所以 node `1` height = `1`。
- Node `3` 的 left subtree height = `1`，right subtree height = `0`，difference = `1`。
- Node `5` 的 left subtree height = `2`，right subtree height = `0`，difference = `2`。

因为 node `5` difference > `1`，所以不是 AVL。

### 2.4 AVL insertion 的总流程

考试画 AVL insertion 时按这个流程：

1. 先按普通 BST insertion 插入新 value。
2. 从新插入的 node 往 root 方向检查 ancestors。
3. 找到第一个不平衡的 node，记作 `alpha`。
4. 看新 node 相对 `alpha` 走的是哪条路径：`LL`、`RR`、`LR`、`RL`。
5. 根据 case 做 rotation。
6. rotation 后重新检查 subtree 是否满足 BST order 和 AVL balance。

关键直觉：

```text
Rotation 只改变局部 subtree 的形状，
不会破坏 BST 的 sorted order。
```

---

## 3. AVL Rotations：四种必须会

### 3.1 先理解 single rotation

Single rotation 处理的是“同侧太高”：

```text
LL: left-left too tall  -> right rotation
RR: right-right too tall -> left rotation
```

直觉：

- 左边太重，就把左 child 提上来，所以整体向右转。
- 右边太重，就把右 child 提上来，所以整体向左转。

### 3.2 Case 1：LL rotation

触发条件：

```text
alpha 的 left subtree 太高，
而新 node 又在 alpha.left 的 left subtree。
```

最小例子：插入 `30, 20, 10`

插入后：

```text
      30  <- alpha
     /
   20
   /
 10
```

问题：

- Node `30` 的 left height = `1`。
- Node `30` 的 right height = `-1`。
- difference = `2`，不平衡。
- 新 node `10` 在 `30.left.left`，所以是 `LL`。

操作：right rotation at `30`

```text
before:
      30
     /
   20
   /
 10

after:
     20
    /  \
  10    30
```

为什么这样正确？

- BST order 保持：`10 < 20 < 30`。
- 新 root `20` 左右各一个 child，balanced。

更一般的 LL 模板：

```text
before:
        alpha
        /
       b
      / \
     X   Y

after right rotation at alpha:
        b
       / \
      X   alpha
          /
         Y
```

考试写指针更新时可以记：

```java
b = alpha.left;
alpha.left = b.right;
b.right = alpha;
return b; // b becomes new subtree root
```

### 3.3 Case 2：RR rotation

触发条件：

```text
alpha 的 right subtree 太高，
而新 node 又在 alpha.right 的 right subtree。
```

最小例子：插入 `10, 20, 30`

插入后：

```text
10  <- alpha
  \
   20
     \
      30
```

问题：

- Node `10` 的 left height = `-1`。
- Node `10` 的 right height = `1`。
- difference = `2`，不平衡。
- 新 node `30` 在 `10.right.right`，所以是 `RR`。

操作：left rotation at `10`

```text
before:
10
  \
   20
     \
      30

after:
     20
    /  \
  10    30
```

为什么这样正确？

- BST order 保持：`10 < 20 < 30`。
- 新 root `20` 左右各一个 child，balanced。

更一般的 RR 模板：

```text
before:
      alpha
          \
           b
          / \
         Y   Z

after left rotation at alpha:
          b
         / \
    alpha   Z
       \
        Y
```

考试写指针更新时可以记：

```java
b = alpha.right;
alpha.right = b.left;
b.left = alpha;
return b; // b becomes new subtree root
```

### 3.4 什么时候需要 double rotation？

Double rotation 处理的是“异侧太高”：

```text
LR: left-right too tall
RL: right-left too tall
```

异侧的意思是：

- 第一步往左，第二步往右：`LR`。
- 第一步往右，第二步往左：`RL`。

为什么不能直接 single rotation？

因为 subtree 是“弯”的。要先把弯折变成直线，再做 single rotation。

记忆：

```text
LR: first left rotation on left child, then right rotation on alpha
RL: first right rotation on right child, then left rotation on alpha
```

### 3.5 Case 3：LR rotation

触发条件：

```text
alpha 的 left subtree 太高，
而新 node 在 alpha.left 的 right subtree。
```

最小例子：插入 `30, 10, 20`

插入后：

```text
      30  <- alpha
     /
   10
     \
      20
```

问题：

- Node `30` left height = `1`，right height = `-1`，difference = `2`。
- 新 node `20` 在 `30.left.right`，所以是 `LR`。

Step 1：left rotation at `10`

```text
before step 1:
      30
     /
   10
     \
      20

after step 1:
      30
     /
   20
   /
 10
```

现在变成 LL shape。

Step 2：right rotation at `30`

```text
before step 2:
      30
     /
   20
   /
 10

after step 2:
     20
    /  \
  10    30
```

最终：

```text
     20
    /  \
  10    30
```

考试一句话写法：

```text
LR case: rotate left at alpha.left, then rotate right at alpha.
```

Java-ish 组合：

```java
alpha.left = rotateLeft(alpha.left);
return rotateRight(alpha);
```

### 3.6 Case 4：RL rotation

触发条件：

```text
alpha 的 right subtree 太高，
而新 node 在 alpha.right 的 left subtree。
```

最小例子：插入 `10, 30, 20`

插入后：

```text
10  <- alpha
  \
   30
   /
 20
```

问题：

- Node `10` left height = `-1`，right height = `1`，difference = `2`。
- 新 node `20` 在 `10.right.left`，所以是 `RL`。

Step 1：right rotation at `30`

```text
before step 1:
10
  \
   30
   /
 20

after step 1:
10
  \
   20
     \
      30
```

现在变成 RR shape。

Step 2：left rotation at `10`

```text
before step 2:
10
  \
   20
     \
      30

after step 2:
     20
    /  \
  10    30
```

最终：

```text
     20
    /  \
  10    30
```

考试一句话写法：

```text
RL case: rotate right at alpha.right, then rotate left at alpha.
```

Java-ish 组合：

```java
alpha.right = rotateRight(alpha.right);
return rotateLeft(alpha);
```

### 3.7 四种 rotation 的共同直觉

四个最小例子其实最终都变成：

```text
     middle
    /      \
 smallest  largest
```

具体：

| 插入顺序 | Case | 最终 balanced subtree |
|---|---|---|
| `30, 20, 10` | LL | root = `20` |
| `10, 20, 30` | RR | root = `20` |
| `30, 10, 20` | LR | root = `20` |
| `10, 30, 20` | RL | root = `20` |

所以如果考试只给三个 key，可以先找中间值：

```text
三者中间值 usually becomes subtree root after rotation.
```

但如果 subtree 里还有 `X/Y/Z/W`，不能只凭中间值画，要保持所有 subtree 的 BST order。

### 3.8 从路径判断 case：最稳方法

不要只看图像“歪向哪里”，容易看错。用路径法：

1. 找第一个不平衡 node：`alpha`。
2. 从 `alpha` 走到新 inserted node。
3. 记录前两步方向。

```text
Left, Left   -> LL
Right, Right -> RR
Left, Right  -> LR
Right, Left  -> RL
```

例子：

```text
        60
       /  \
     20    100
           /
         80
        /
      70
```

从 `60` 到新 node `70`：

```text
60 -> right to 100 -> left to 80 -> left to 70
```

前两步是：

```text
Right, Left
```

所以在 node `60` 上是 `RL case`。

---

## 4. Lecture / Past Paper 风格 AVL 例子：insert 70

这是 sample final 里最像 Lecture 21 的题型。

初始 AVL tree：

```text
        60
       /  \
     20    100
           /  \
         80    120
```

### 4.1 先按 BST 插入 70

路径：

```text
70 > 60   -> go right
70 < 100  -> go left
70 < 80   -> go left
```

插入后：

```text
        60
       /  \
     20    100
           /  \
         80    120
        /
      70
```

### 4.2 检查 balance

从新 node `70` 往上检查：

| Node | left height | right height | difference | OK? |
|---|---:|---:|---:|---|
| `80` | `0` | `-1` | `1` | OK |
| `100` | `1` | `0` | `1` | OK |
| `60` | `0` | `2` | `2` | not OK |

所以：

```text
alpha = 60
```

从 `60` 到 `70` 的前两步：

```text
Right, Left
```

所以：

```text
RL case
```

### 4.3 RL double rotation

RL 的规则：

```text
1. right rotation at alpha.right
2. left rotation at alpha
```

这里就是：

```text
1. right rotation at 100
2. left rotation at 60
```

Step 1：right rotation at `100`

```text
        60
       /  \
     20    80
          /  \
        70    100
                \
                120
```

Step 2：left rotation at `60`

```text
          80
        /    \
      60      100
     /  \       \
   20    70      120
```

最终答案：

```text
          80
        /    \
      60      100
     /  \       \
   20    70      120
```

最终检查：

- Node `60`：left `20`，right `70`，balanced。
- Node `100`：right `120`，left empty，difference = `1`，balanced。
- Root `80`：left subtree height = `1`，right subtree height = `1`，balanced。

---

## 5. AVL 复杂度和 height 公式

### 5.1 操作复杂度

| Operation | Complexity | 原因 |
|---|---|---|
| Search | `O(log n)` | 只沿着一条 root-to-leaf path 走 |
| Insert | `O(log n)` | BST 插入走一条 path；回溯检查 ancestors；rotation 是 `O(1)` |
| Delete | `O(log n)` | BST 删除 + 回溯 rebalance；height 是 `O(log n)` |

### 5.2 Minimum height

如果题目问：

```text
If there are n nodes in an AVL tree, what is the minimum height?
```

最小 height 其实是“尽量填满”的 binary tree height。

课程 Lecture 20 对 height 的定义是 edge 数，所以：

```text
minimum height = ceil(log2(n + 1)) - 1
```

如果答案选项比较粗略，有时会写成：

```text
floor(log2 n)
```

常见值：

| `n` | minimum height |
|---:|---:|
| 1 | 0 |
| 2-3 | 1 |
| 4-7 | 2 |
| 8-15 | 3 |
| 16-31 | 4 |

推导：

```text
height = h 的 binary tree 最多有 2^(h + 1) - 1 个 nodes
n <= 2^(h + 1) - 1
n + 1 <= 2^(h + 1)
log2(n + 1) <= h + 1
h >= log2(n + 1) - 1
```

所以取最小整数：

```text
h = ceil(log2(n + 1)) - 1
```

---

## 6. Heap Tree

### 6.1 Heap 定义

Lecture 21 讲的是 min-heap。

Heap 是：

```text
complete binary tree + heap property
```

Complete binary tree：

- 除了 bottommost level，其他每一层必须填满。
- bottommost level 如果没填满，必须从左到右连续填，不能中间空着。

Min-heap property：

```text
parent value <= child value
```

合法 min-heap：

```text
        4
       / \
      5   7
     / \ /
    16 10 15
```

检查：

- `4 <= 5`，`4 <= 7`
- `5 <= 16`，`5 <= 10`
- `7 <= 15`

不合法 heap：

```text
        4
       / \
     15   7
     / \ /
    5 10 16
```

原因：

```text
15 has child 5, but 15 > 5
```

违反 min-heap property。

### 6.2 Heap vs BST

Heap 不是 BST。不要混淆。

| 对比 | BST | Min-heap |
|---|---|---|
| left/right 有大小顺序吗？ | 有：left <= root < right | 没有这种 left/right sorted rule |
| root 是什么？ | 不一定最小或最大 | 一定是 minimum |
| inorder 会 sorted 吗？ | 会 | 不会保证 |
| 主要用途 | search / ordered data | priority queue / repeatedly get minimum |
| shape | 不一定 complete | 必须 complete |

### 6.3 Heap property 的直接结果

Min-heap 的 root 一定是 whole heap minimum。

为什么？

```text
每个 parent 都 <= child。
从 root 走到任何 node，value 都不会比 root 更小。
所以 root 是 minimum。
```

复杂度：

| Operation | Complexity |
|---|---|
| access minimum | `O(1)` |
| remove minimum | `O(log n)` |
| insert new number | `O(log n)` |

原因：

- Heap 是 complete binary tree。
- Complete binary tree 的 height 是 `O(log n)`。
- 插入/删除最多沿着一条 root-to-leaf path 调整。

---

## 7. Heap Operations

### 7.1 Remove minimum

Min-heap 删除 minimum，一定是删除 root。

步骤：

1. 把最后一个 node 的 value copy 到 root。
2. 删除最后一个 node。
3. 从 root 开始 `re-heapify down`。
4. 每次和较小的 child 比。
5. 如果 parent 比较小 child 大，就 swap。
6. 一直 swap 到 parent <= both children。

例子：删除 `4`

原 heap：

```text
        4
       / \
      5   7
     / \ /
    16 10 15
```

Step 1：last node `15` copy 到 root，删除原来的 last node：

```text
       15
       / \
      5   7
     / \
    16 10
```

Step 2：比较 `15` 的 children：`5` 和 `7`，选较小 child `5`，swap：

```text
        5
       / \
     15   7
     / \
    16 10
```

Step 3：继续比较 `15` 的 children：`16` 和 `10`，选较小 child `10`，swap：

```text
        5
       / \
     10   7
     / \
    16 15
```

完成，因为 `15` 已经是 leaf。

删除出来的 minimum 是：

```text
4
```

删除后的 heap 是：

```text
        5
       / \
     10   7
     / \
    16 15
```

注意：这仍然是 valid min-heap，因为 parent 都 <= children。
它不需要像 BST 那样 left < root < right。

### 7.2 Insert new number

Heap 插入时不能随便放，因为要保持 complete binary tree。

步骤：

1. 把新 value 放在 bottommost level 最左边可用位置。
2. 等价于 array 表示里的末尾 append。
3. 从新 node 开始 `re-heapify up`。
4. 如果 new value 比 parent 小，就和 parent swap。
5. 重复直到 parent <= new value，或者 new value 到 root。

例子 A：把 `5` 插入这个 heap：

```text
        7
       / \
     10   15
     /
    16
```

先 append 到 bottommost level：

```text
        7
       / \
     10   15
     / \
    16  5
```

`5 < 10`，swap：

```text
        7
       / \
      5  15
     / \
    16 10
```

`5 < 7`，继续 swap：

```text
        5
       / \
      7  15
     / \
    16 10
```

最终 heap：

```text
        5
       / \
      7  15
     / \
    16 10
```

例子 B：再把 `4` 插入上面的 heap：

先 append：

```text
        5
       / \
      7  15
     / \ /
    16 10 4
```

`4 < 15`，swap：

```text
        5
       / \
      7   4
     / \ /
    16 10 15
```

`4 < 5`，swap：

```text
        4
       / \
      7   5
     / \ /
    16 10 15
```

最终 heap：

```text
        4
       / \
      7   5
     / \ /
    16 10 15
```

---

## 8. Heap Sort

Heap sort 的思路：

1. 把每个 number 一个一个 insert 到 heap。
2. 不断 remove minimum。
3. 每次 remove 出来的 value 按顺序写下来。
4. 结果就是 ascending order。

复杂度：

| Phase | Complexity |
|---|---|
| Insert all `n` numbers | 每次 `O(log n)`，总共 `O(n log n)` |
| Delete all `n` numbers | 每次 `O(log n)`，总共 `O(n log n)` |
| Total | `O(n log n)` |

如果用 min-heap：

```text
remove sequence = ascending order
```

如果用 max-heap：

```text
remove sequence = descending order
```

Lecture 21 用的是 min-heap。

---

## 9. Heap 的 Array Implementation

因为 heap 是 complete binary tree，所以非常适合用 array 存。

存法：

```text
level-by-level, left-to-right
```

例子：

```text
        4
       / \
      5   7
     / \ /
    16 10 15
```

Array：

```text
index:  0  1  2   3   4   5
value:  4  5  7  16  10  15
```

也就是：

```text
arr = {4, 5, 7, 16, 10, 15}
```

对 `arr[i]`：

```text
left child  = arr[2*i + 1]
right child = arr[2*i + 2]
parent      = arr[(i - 1) / 2]
```

注意 Java integer division：

```java
int parent = (i - 1) / 2;
```

对于 `i = 0`，root 没有 parent，不要算 parent。

常见 index 例子：

| `i` | `arr[i]` | left child index | right child index | parent index |
|---:|---:|---:|---:|---:|
| 0 | 4 | 1 | 2 | none |
| 1 | 5 | 3 | 4 | 0 |
| 2 | 7 | 5 | 6 | 0 |
| 3 | 16 | 7 | 8 | 1 |

在 array 里做 swap 就等价于 tree 里交换 node values。

---

## 10. 考试答题模板

### 10.1 AVL insertion / rotation 题模板

如果题目说：

```text
Show the steps and result of the updated AVL tree after insertion of X.
```

答案结构：

```text
1. Insert X using normal BST insertion.
2. Check balance from X upward.
3. The first imbalanced node is alpha = ___.
4. The path from alpha to X is ___, so it is ___ case.
5. Perform rotation(s):
   - If LL: right rotation at alpha.
   - If RR: left rotation at alpha.
   - If LR: left rotation at alpha.left, then right rotation at alpha.
   - If RL: right rotation at alpha.right, then left rotation at alpha.
6. Draw final AVL tree.
```

### 10.2 Heap remove minimum 题模板

```text
1. The minimum is at the root.
2. Replace root with the last node.
3. Remove the last node.
4. Re-heapify down:
   compare with both children,
   swap with smaller child,
   repeat until heap property is restored.
```

### 10.3 Heap insert 题模板

```text
1. Insert new value at the next available position in bottommost level.
2. Re-heapify up:
   compare with parent,
   swap if parent is larger,
   repeat until parent is smaller or the new value becomes root.
```

### 10.4 Heap array index 题模板

```text
For arr[i]:
left child index  = 2*i + 1
right child index = 2*i + 2
parent index      = (i - 1) / 2
```

---

## 11. 最容易错的点

### 11.1 AVL 和 BST 的关系

错误：

```text
AVL is different from BST.
```

正确：

```text
AVL is a BST with extra balance property.
```

所以 AVL insertion 一定先按 BST insertion 插入。

### 11.2 Height 的 empty tree 是 -1

课程明确使用：

```text
empty tree height = -1
leaf height = 0
```

所以一个只有 left leaf、right empty 的 node：

```text
left height = 0
right height = -1
difference = 1
```

这是 balanced。

### 11.3 不平衡 node 不是一定是新 node 的 parent

插入后从新 node 往上检查，可能：

- parent 还是 balanced。
- grandparent 还是 balanced。
- 更上面的 ancestor 才 imbalanced。

Sample final insert `70` 里：

- `80` balanced。
- `100` balanced。
- `60` 才 imbalanced。

### 11.4 LL / RR / LR / RL 是看 alpha 到新 node 的路径

不要看整棵树大概往哪边歪。
最稳的是：

```text
alpha -> inserted node 的前两步方向
```

### 11.5 Double rotation 不是随便转两次

固定顺序：

```text
LR = left rotation at left child, then right rotation at alpha
RL = right rotation at right child, then left rotation at alpha
```

### 11.6 Heap 不是 BST

Min-heap 只保证：

```text
parent <= children
```

不保证：

```text
left subtree <= root <= right subtree
```

所以 heap 的 inorder traversal 不一定 sorted。

### 11.7 Remove min 时要和较小 child swap

`re-heapify down` 时，如果两个 children 都比 parent 小：

```text
swap with the smaller child
```

否则 swap 后可能仍然违反 heap property。

---

## 12. 最后 30 秒记忆版

```text
AVL = self-balancing BST.
Every node: abs(left height - right height) <= 1.
empty height = -1, leaf height = 0.

Insert:
BST insert first -> find first imbalanced alpha -> rotate.

LL -> right rotation.
RR -> left rotation.
LR -> left at alpha.left, then right at alpha.
RL -> right at alpha.right, then left at alpha.

Heap = complete binary tree + heap property.
Min-heap root = minimum.
Delete min: last node to root, then down-heap with smaller child.
Insert: append at bottom, then up-heap with parent.
Array: left 2i+1, right 2i+2, parent (i-1)/2.
Heap sort = O(n log n).
```
