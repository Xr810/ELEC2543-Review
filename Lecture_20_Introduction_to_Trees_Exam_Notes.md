# Lecture 20: Introduction to Trees - Exam Notes

> 范围：`Lecture/20_Intro_Tree.pdf`，主要是 tree / binary tree terminology、tree traversal、ordered binary tree / BST、BST insertion 和 deletion。
> 读法：先看第 0 节速查，再重点看第 2 节 traversal、第 3-4 节 BST insertion/deletion。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Tree 必背

| 问题 | 答案 |
|---|---|
| tree 是什么？ | non-linear data structure，由 nodes 和 edges 组成 |
| tree 中任意两个 nodes 之间有几条 path？ | exactly one path |
| N 个 nodes 的 tree 有几条 links/edges？ | `N - 1` |
| general tree 中每个 node 可有几个 children？ | arbitrary number |
| binary tree 中每个 node 最多几个 children？ | two：left child and right child |
| root | topmost node |
| siblings | same parent 的 nodes |
| leaf | no child 的 node |
| internal node | 有 one or two children 的 node |
| subtree | 某 node 和它所有 descendants 组成的 tree |
| ancestor | path from node to root 上的上层 node |
| descendant | 在某 node subtree 中的 node |
| height of tree | number of edges on longest path from root to a leaf |

### Traversal 必背

| Traversal | 顺序 | 记忆 |
|---|---|---|
| `Preorder` | Root, Left, Right | root first |
| `Inorder` | Left, Root, Right | root in middle |
| `Postorder` | Left, Right, Root | root last |

### BST 必背

| 问题 | 答案 |
|---|---|
| Ordered Binary Tree 也叫什么？ | Binary Search Tree (`BST`) |
| BST property | left subtree values `<= root`, right subtree values `> root` |
| BST inorder traversal 结果 | ascending order |
| insert `x <= current.value` | go left |
| insert `x > current.value` | go right |
| delete leaf | simply remove it |
| delete node with one child | promote child |
| delete node with two children | find leftmost node in right subtree as replacement |

---

## 1. What is a Tree?

### 1.1 Tree definition

Tree 是：

```text
a non-linear data structure that consists of nodes and edges connecting them
```

重要性质：

```text
There is exactly one path connecting any two nodes.
```

因此：

```text
If there are N nodes in a tree, there are exactly N - 1 links.
```

如果有 cycle，或两个 nodes 之间有多条 path，就不是 tree。

### 1.2 General tree

General tree 中：

```text
each node can connect to an arbitrary number of nodes
```

例子：

```text
        A
     /  |  \
    B   C   D
       / \
      E   F
```

`A` 有 3 个 children，仍然可以是 general tree。

---

## 2. Binary Trees and Terminology

### 2.1 Binary tree

Binary tree 是有额外限制的 tree：

```text
each node has at most two children
```

两个 children 叫：

- `left child`
- `right child`

结构：

```text
        root
       /    \
   left     right
```

### 2.2 Common terms

| Term | 中文理解 |
|---|---|
| `root` | 最上面的 node |
| `parent` | 某 node 的上一级 |
| `child` | 某 node 的下一级 |
| `siblings` | 同一个 parent 的 children |
| `leaf` | 没有 child 的 node |
| `internal node` | 有 one or two children 的 node |
| `subtree` | 某 node 和它下面所有 descendants |
| `ancestor` | 从 node 往 root 走会经过的上层 nodes |
| `descendant` | 在某 node 下方 subtree 中的 nodes |
| `height` | root 到 leaf 的最长 path 上 edge 数 |

### 2.3 Height

Lecture 定义：

```text
Height of Tree = number of edges on the longest path from root to a leaf
```

例子：

```text
        10
       /  \
      6    15
     /
    2
     \
      5
```

最长 path：

```text
10 -> 6 -> 2 -> 5
```

edges 数：

```text
3
```

所以 height = `3`。

注意：

```text
height counts edges, not nodes.
```

---

## 3. Tree Traversal

### 3.1 Traversal 是什么？

`Traversing a binary tree` 意思是：

```text
visit each node in the tree
```

不同 traversal 只是在 root/left/right 的访问顺序不同。

### 3.2 Preorder traversal

顺序：

```text
Root, Left, Right
```

Java-ish：

```java
void preorder(Node node) {
    if (node != null) {
        visit(node);
        preorder(node.left);
        preorder(node.right);
    }
}
```

记忆：

```text
pre = root before children
```

### 3.3 Inorder traversal

顺序：

```text
Left, Root, Right
```

Java-ish：

```java
void inorder(Node node) {
    if (node != null) {
        inorder(node.left);
        visit(node);
        inorder(node.right);
    }
}
```

记忆：

```text
in = root in between left and right
```

对于 BST：

```text
inorder traversal gives ascending order.
```

### 3.4 Postorder traversal

顺序：

```text
Left, Right, Root
```

Java-ish：

```java
void postorder(Node node) {
    if (node != null) {
        postorder(node.left);
        postorder(node.right);
        visit(node);
    }
}
```

记忆：

```text
post = root after children
```

### 3.5 Lecture example

Tree：

```text
          10
        /    \
       6      15
      / \     /
     2   8   13
      \      /
       5    11
```

Preorder：

```text
10, 6, 2, 5, 8, 15, 13, 11
```

Inorder：

```text
2, 5, 6, 8, 10, 11, 13, 15
```

Postorder：

```text
5, 2, 8, 6, 11, 13, 15, 10
```

### 3.6 Exercise example

Tree：

```text
        1
       / \
      2   3
     / \
    4   5
```

Inorder：

```text
4, 2, 5, 1, 3
```

Preorder：

```text
1, 2, 4, 5, 3
```

Postorder：

```text
4, 5, 2, 3, 1
```

考试手算方法：

1. 先写 root。
2. 圈出 left subtree 和 right subtree。
3. 对每个 subtree 用同样规则递归处理。

---

## 4. Ordered Binary Tree / BST

### 4.1 Definition

Ordered Binary Tree 也叫：

```text
Binary Search Tree (BST)
```

BST property：

```text
left subtree values <= current node value
right subtree values > current node value
```

课程 insertion 规则使用：

```text
less than or equal -> left
larger -> right
```

所以 duplicate values 会去 left subtree。

### 4.2 为什么 inorder gives ascending order？

Inorder 顺序：

```text
Left, Root, Right
```

BST property：

```text
left <= root < right
```

所以 traversal 顺序就是：

```text
smaller values -> current value -> larger values
```

递归地看整棵树，就是 ascending order。

### 4.3 Search in BST

虽然 slides 重点是 insert/delete，但 search 是理解基础。

搜索 `x`：

```text
if root is null -> not found
if x == root.value -> found
if x <= root.value -> search left
if x > root.value -> search right
```

注意：如果 duplicates go left，search equal 时通常已经 found，不需要继续 left。

---

## 5. Insertion in Ordered Binary Tree

### 5.1 Insertion rules

课件规则：

1. If root is null, make new element the root。
2. If item `<= root`, add on left subtree。
3. If item `> root`, add on right subtree。
4. If target subtree is missing, insert there。
5. Can be implemented using recursion。

### 5.2 Insert example：add 6, 3, 4, 9, 10

Start empty。

Add `6`：

```text
6
```

Add `3`：

```text
3 <= 6 -> left

  6
 /
3
```

Add `4`：

```text
4 <= 6 -> left to 3
4 > 3  -> right

  6
 /
3
 \
  4
```

Add `9`：

```text
9 > 6 -> right

  6
 / \
3   9
 \
  4
```

Add `10`：

```text
10 > 6 -> right to 9
10 > 9 -> right

  6
 / \
3   9
 \   \
  4   10
```

### 5.3 Recursive insert template

```java
private Node insert(Node root, int value) {
    if (root == null) {
        return new Node(value);
    }

    if (value <= root.value) {
        root.left = insert(root.left, value);
    } else {
        root.right = insert(root.right, value);
    }

    return root;
}
```

Public wrapper：

```java
public void add(int value) {
    root = insert(root, value);
}
```

为什么要 `return root`？

```text
So the parent call can attach the updated subtree back to left or right.
```

---

## 6. Deletion in Ordered Binary Tree

### 6.1 Three cases

删除 node 时分三种情况：

| Case | 做法 |
|---|---|
| node is leaf | simply remove it |
| node has single child | promote the child |
| node has two children | find leftmost child in right subtree to replace it |

### 6.2 Case 1：delete leaf

例子：

```text
          10
        /    \
       6      15
      / \     /
     2   8   13
      \
       5
```

Delete `5`。

`5` 是 leaf，没有 children。

直接删除：

```text
          10
        /    \
       6      15
      / \     /
     2   8   13
```

### 6.3 Case 2：delete node with one child

例子：

```text
          10
        /    \
       6      15
      / \     /
     2   8   13
            /
           11
```

Delete `15`。

`15` 只有一个 child：`13`。

Promote child：

```text
          10
        /    \
       6      13
      / \     /
     2   8   11
```

核心：

```text
parent of deleted node points directly to deleted node's child.
```

### 6.4 Case 3：delete node with two children

例子：

```text
          10
        /    \
       6      15
      / \     /
     2   8   13
            /
           11
```

Delete `10`。

`10` 有 two children。

做法：

```text
find leftmost child on the right subtree
```

Right subtree root 是 `15`。

往 left 走：

```text
15 -> 13 -> 11
```

leftmost = `11`。

用 `11` replace `10`：

```text
          11
        /    \
       6      15
      / \     /
     2   8   13
```

为什么找 right subtree 的 leftmost？

```text
It is the smallest value greater than the deleted node.
```

也叫：

```text
inorder successor
```

这样替换后 BST order 仍然成立。

### 6.5 `findSmallestValue`

课件给的 idea：

```java
private int findSmallestValue(Node root) {
    if (root.left == null) {
        return root.value;
    } else {
        return findSmallestValue(root.left);
    }
}
```

注意需要 `return` recursive result。

如果写成：

```java
findSmallestValue(root.left);
```

但不 `return`，Java 会 compile error 或逻辑错误。

### 6.6 Recursive delete template

```java
private Node delete(Node root, int value) {
    if (root == null) {
        return null;
    }

    if (value < root.value) {
        root.left = delete(root.left, value);
    } else if (value > root.value) {
        root.right = delete(root.right, value);
    } else {
        // found node to delete
        if (root.left == null && root.right == null) {
            return null;
        }

        if (root.left == null) {
            return root.right;
        }

        if (root.right == null) {
            return root.left;
        }

        int successor = findSmallestValue(root.right);
        root.value = successor;
        root.right = delete(root.right, successor);
    }

    return root;
}
```

考试如果不用写 full code，至少要写清楚 three cases。

---

## 7. Traversal Code Templates

### 7.1 Node structure

```java
private class Node {
    int value;
    Node left;
    Node right;

    Node(int value) {
        this.value = value;
    }
}
```

### 7.2 Inorder

```java
private void inorder(Node root) {
    if (root != null) {
        inorder(root.left);
        System.out.println(root.value);
        inorder(root.right);
    }
}
```

For BST:

```text
prints values in ascending order
```

### 7.3 Preorder

```java
private void preorder(Node root) {
    if (root != null) {
        System.out.println(root.value);
        preorder(root.left);
        preorder(root.right);
    }
}
```

Useful for:

```text
copying tree structure / prefix expression style questions
```

### 7.4 Postorder

```java
private void postorder(Node root) {
    if (root != null) {
        postorder(root.left);
        postorder(root.right);
        System.out.println(root.value);
    }
}
```

Useful for:

```text
deleting/freeing nodes / postfix expression style questions
```

---

## 8. 考试手算模板

### 8.1 Traversal template

遇到 traversal 题：

1. 写出 traversal rule。
2. 对 root 应用 rule。
3. 对 left subtree 递归应用。
4. 对 right subtree 递归应用。

```text
Preorder:  Root, Left, Right
Inorder:   Left, Root, Right
Postorder: Left, Right, Root
```

### 8.2 BST insertion template

```text
Start at root.
If value <= current, go left.
If value > current, go right.
Repeat until empty spot.
Insert new node there.
```

### 8.3 BST deletion template

```text
Find the node using BST search.

Case 1: leaf
Remove it directly.

Case 2: one child
Promote the child.

Case 3: two children
Find leftmost node in right subtree.
Replace deleted node's value with it.
Delete that successor node from the right subtree.
```

### 8.4 Validate BST after operation

After insert/delete, check:

```text
Every node's left subtree values <= node value.
Every node's right subtree values > node value.
Inorder traversal is ascending.
```

---

## 9. 最容易错的点

### 9.1 Height 数 edges，不是 nodes

错误：

```text
root to leaf has 4 nodes, height = 4
```

正确：

```text
height = number of edges = 3
```

### 9.2 Traversal 顺序混淆

记：

```text
Pre: root before
In: root in middle
Post: root after
```

### 9.3 BST duplicate direction

本课件 insertion 规则：

```text
less than or equal -> left
larger -> right
```

所以 `<=` 去 left。

### 9.4 Delete two-children node 不要随便拿 child 顶上

有 two children 时，不能简单 promote left 或 right child。

要找：

```text
leftmost node in right subtree
```

也就是 successor。

### 9.5 `findSmallestValue` 递归要 return

错误：

```java
else findSmallestValue(root.left);
```

正确：

```java
else return findSmallestValue(root.left);
```

### 9.6 BST 和普通 binary tree 不一样

普通 binary tree 没有 ordering rule。

BST 有：

```text
left <= root < right
```

所以只有 BST 的 inorder traversal 才保证 ascending order。

---

## 10. 最后 30 秒记忆版

```text
Tree = non-linear nodes + edges.
Exactly one path between any two nodes.
N nodes -> N - 1 links.

Binary tree:
each node has at most two children: left and right.

root = top node.
siblings = same parent.
leaf = no child.
internal node = has child.
height = number of edges on longest root-to-leaf path.

Traversal:
Preorder = Root Left Right.
Inorder = Left Root Right.
Postorder = Left Right Root.

BST / Ordered Binary Tree:
left subtree <= root.
right subtree > root.
Inorder traversal gives ascending order.

Insert:
root null -> new root.
value <= current -> left.
value > current -> right.

Delete:
leaf -> remove.
one child -> promote child.
two children -> replace with leftmost node in right subtree.
```
