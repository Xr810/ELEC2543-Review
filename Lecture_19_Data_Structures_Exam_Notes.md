# Lecture 19: Data Structures - Exam Notes

> 范围：`Lecture/19_Data_Structure.pdf`，主要是 collections、ADT、dynamic structures、linked lists、queues、stacks、trees/graphs、Java Collections API、generics。
> 读法：先看第 0 节速查，再重点看第 3 节 linked list pointer updates、第 4 节 queue、第 5 节 stack。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Data Structures 必背

| 问题 | 答案 |
|---|---|
| `collection` 是什么？ | helps organize and manage other objects 的 object |
| `ADT` 是什么？ | organized collection of information + operations to manage it |
| static data structure | fixed size，例子：array |
| dynamic data structure | runtime grows/shrinks，用 object references as links |
| reference 还可叫什么？ | pointer |
| linked list node 通常有什么？ | data/object reference + next link |
| 为什么用 intermediate node？ | stored object 不需要知道自己在 data structure 中的 link details |
| insert after current | `newNode.next = current.next; current.next = newNode;` |
| delete after current in singly list | `current.next = current.next.next;` |
| queue | FIFO, add rear, remove front |
| stack | LIFO, add/remove top |
| queue operations | `enqueue`, `dequeue`, `empty` |
| stack operations | `push`, `pop`, `peek/top`, `empty` |
| tree | non-linear hierarchy with root and children |
| graph | non-linear structure, no root, nodes connected by edges |
| digraph | directed graph, edges have direction |
| Java Collections API examples | `ArrayList`, `LinkedList`, `List`, `Set`, `Map` |
| generics benefit | type safety；取出时 type already established |

### Queue vs Stack

| Structure | Rule | Insert | Remove | Example |
|---|---|---|---|---|
| `Queue` | FIFO | rear | front | line at bank |
| `Stack` | LIFO | top | top | stack of plates |

---

## 1. Collections and ADTs

### 1.1 Collection

`Collection` 是：

```text
an object that helps us organize and manage other objects
```

例子：

- list of books。
- queue of customers。
- stack of characters。
- set of unique values。
- map from key to value。

### 1.2 Abstract Data Type (ADT)

`ADT` 是：

```text
an organized collection of information and a set of operations used to manage that information
```

重点：

```text
ADT describes what operations are available, not necessarily how they are implemented.
```

例子：

`Queue ADT` 规定：

- `enqueue`
- `dequeue`
- `empty`

但 implementation 可以是：

- linked list。
- circular array。
- `ArrayList`。

### 1.3 Interface vs implementation

这讲反复出现一个思想：

```text
Separate the interface from the implementation.
```

意思：

- User cares about operations。
- Implementer decides internal representation。

例子：

```text
Queue operation: dequeue()
Implementation A: move front pointer in linked list
Implementation B: remove from circular array
```

外部代码不应该依赖内部细节。

### 1.4 Common data structures in Java

Lecture list：

- Array
- Linked List
- Stack
- Queue
- Binary Tree
- Binary Search Tree
- Heap
- Hashing
- Graph

本讲主要展开：

- linked list。
- queues and stacks。
- trees and graphs overview。
- Java Collections API。

---

## 2. Dynamic Structures and References

### 2.1 Static vs dynamic

| Structure type | Meaning | Example |
|---|---|---|
| static data structure | fixed size after creation | array |
| dynamic data structure | grows/shrinks at execution time | linked list |

Array:

```java
int[] arr = new int[10];
```

长度固定。

Linked list:

```text
add node -> list grows
remove node -> list shrinks
```

### 2.2 Object reference

Object reference stores address of an object。

也可叫：

```text
pointer
```

在 linked structures 中，reference 用来连接 objects。

### 2.3 Node class

最简单 node：

```java
class Node {
    int info;
    Node next;
}
```

这里：

- `info` 存 data。
- `next` 指向下一个 node。

### 2.4 Intermediate nodes

课件强调：

```text
The objects being stored should not be concerned with the details of the data structure.
```

错误设计：

```java
class Student {
    String name;
    Student next; // Student itself stores list link
}
```

这样 `Student` 被 linked list implementation 污染。

更好：

```java
class StudentNode {
    Student student;
    StudentNode next;
}
```

好处：

- `Student` 只表示 student。
- `StudentNode` 才负责 list structure。

---

## 3. Linked Lists

### 3.1 Singly linked list 结构

```text
list -> [Magazine | next] -> [Magazine | next] -> [Magazine | null]
```

`list` 是 head reference，指向 first node。

如果 list empty：

```text
list == null
```

### 3.2 MagazineList 结构

简化版：

```java
public class MagazineList {
    private MagazineNode list;

    public MagazineList() {
        list = null;
    }

    private class MagazineNode {
        public Magazine magazine;
        public MagazineNode next;

        public MagazineNode(Magazine mag) {
            magazine = mag;
            next = null;
        }
    }
}
```

这里 `MagazineNode` 是 private inner class。

意思：

```text
Only MagazineList needs to know node details.
```

### 3.3 Add to end

逻辑：

1. 创建 new node。
2. 如果 list empty，`list = node`。
3. 否则从 head 走到 last node。
4. 令 last node 的 `next` 指向 new node。

模板：

```java
public void add(Magazine mag) {
    MagazineNode node = new MagazineNode(mag);

    if (list == null) {
        list = node;
    } else {
        MagazineNode current = list;

        while (current.next != null) {
            current = current.next;
        }

        current.next = node;
    }
}
```

### 3.4 Traverse linked list

模板：

```java
MagazineNode current = list;

while (current != null) {
    System.out.println(current.magazine);
    current = current.next;
}
```

重点：

```text
Use current to walk through the list.
Stop when current == null.
```

不要移动 `list` 本身，否则会丢掉 head。

### 3.5 Insert node after current

题目：

```text
Write code that inserts newNode after the node pointed to by current.
```

答案：

```java
newNode.next = current.next;
current.next = newNode;
```

顺序不能反。

为什么？

原来：

```text
current -> A
newNode -> null
```

先做：

```java
newNode.next = current.next;
```

变成：

```text
newNode -> A
```

再做：

```java
current.next = newNode;
```

变成：

```text
current -> newNode -> A
```

如果先写 `current.next = newNode`，原来的后续 node `A` 会丢失。

### 3.6 Delete node after current in singly linked list

题目：

```text
Write code that deletes the node following current.
```

答案：

```java
if (current.next != null) {
    current.next = current.next.next;
}
```

图像：

```text
current -> target -> after
```

做完：

```text
current -> after
```

`target` 没有 reference 指向它，之后可被 garbage collected。

### 3.7 Doubly linked list

Doubly linked list node 有：

```java
Node prev;
Node next;
```

结构：

```text
null <- A <-> B <-> C -> null
```

优点：

- 可以 forward traversal。
- 可以 backward traversal。
- deletion/insertion 有时更方便。

### 3.8 Delete after current in doubly linked list

课件 Quick Check：

```java
if (current.next != null) {
    current.next = current.next.next;

    if (current.next != null) {
        current.next.prev = current;
    }
}
```

解释：

1. 先跳过 target node。
2. 如果跳过后还有 node，要把那个 node 的 `prev` 改回 `current`。

图像：

```text
current <-> target <-> after
```

删除后：

```text
current <-> after
```

所以需要同时维护：

- `next`
- `prev`

### 3.9 Java `LinkedList`

Java standard library 有：

```java
LinkedList<Book> myList = new LinkedList<Book>();
```

常见 methods：

| Method | Meaning |
|---|---|
| `add(Object)` | add element at end |
| `add(int index, Object)` | add element at specific index |
| `remove(Object)` | remove first occurrence of object |
| `remove(int index)` | remove element at index |
| iteration | loop through elements |

实际考试如果问 pointer update，通常考手写 linked list；如果问 library，用 method names。

---

## 4. Queues

### 4.1 Queue definition

Queue 是：

```text
add at rear, remove from front
```

规则：

```text
FIFO = First-In, First-Out
```

比喻：

```text
a line of people at a bank teller's window
```

先排队的人先被服务。

### 4.2 Queue operations

| Operation | Meaning |
|---|---|
| `enqueue` | add item to rear |
| `dequeue` / `serve` | remove item from front |
| `empty` | return true if queue is empty |

### 4.3 Queue exercise

初始 empty。

Operations：

```text
enqueue(45)
enqueue(12)
enqueue(28)
dequeue()
dequeue()
enqueue(69)
enqueue(27)
```

Trace：

```text
[]
enqueue 45 -> [45]
enqueue 12 -> [45, 12]
enqueue 28 -> [45, 12, 28]
dequeue    -> [12, 28]       removed 45
dequeue    -> [28]           removed 12
enqueue 69 -> [28, 69]
enqueue 27 -> [28, 69, 27]
```

答案：

```text
(front/head) 28, 69, 27 (rear)
```

### 4.4 Queue with linked list

高效 queue 通常维护两个 references：

```java
private MyNode front;
private MyNode rear;
```

Empty：

```java
public boolean empty() {
    return front == null;
}
```

Enqueue：

```java
public void enqueue(Object item) {
    MyNode newNode = new MyNode(item);

    if (empty()) {
        front = rear = newNode;
    } else {
        rear.next = newNode;
        rear = newNode;
    }
}
```

注意：课件 slide 中 linked-list 版本显示 `rear.next = newNode`，实际完整实现还应更新：

```java
rear = newNode;
```

否则 `rear` 不会移动。

Dequeue：

```java
public Object dequeue() {
    if (empty()) {
        return null;
    }

    Object obj = front.obj;

    if (front == rear) {
        front = rear = null;
    } else {
        front = front.next;
    }

    return obj;
}
```

### 4.5 Queue with array / circular array

课件提到：

```text
Array implementation can use remainder operator (%) to wrap around.
```

直觉：

```java
rear = (rear + 1) % array.length;
front = (front + 1) % array.length;
```

这样到 array end 后，可以回到 index 0 使用空位。

---

## 5. Stacks

### 5.1 Stack definition

Stack 是：

```text
items are added and removed from only one end
```

规则：

```text
LIFO = Last-In, First-Out
```

比喻：

```text
a stack of plates
```

最后放上去的 plate 最先拿走。

### 5.2 Stack operations

| Operation | Meaning |
|---|---|
| `push` | add item to top |
| `pop` | remove item from top |
| `peek` / `top` | get top item without removing |
| `empty` | return true if stack is empty |

### 5.3 Stack exercise

初始 empty。

Operations：

```text
push(45)
push(12)
push(28)
pop()
pop()
push(69)
push(27)
push(99)
```

Trace：

```text
[]
push 45 -> [45]
push 12 -> [45, 12]
push 28 -> [45, 12, 28]
pop     -> [45, 12]       removed 28
pop     -> [45]           removed 12
push 69 -> [45, 69]
push 27 -> [45, 69, 27]
push 99 -> [45, 69, 27, 99]
```

答案：

```text
(bottom) 45, 69, 27, 99 (top)
```

### 5.4 Stack linked-list representation

用 singly linked list 表示 stack 时：

```text
first node in list is top of stack
```

Push at front：

```java
newNode.next = top;
top = newNode;
```

Pop from front：

```java
Object item = top.obj;
top = top.next;
return item;
```

这样 `push` / `pop` 都是 `O(1)`。

### 5.5 Stack array representation

用 array 表示：

```text
bottom of stack at index 0
top moves upward
```

常见：

```java
top++;
stack[top] = item;
```

Pop：

```java
Object item = stack[top];
top--;
return item;
```

### 5.6 Decode example

Lecture 里的 stack example：反转每个 word。

Input：

```text
artxE eseehc esaelp
```

每个 word 的 letters 是反的。

Algorithm：

1. 逐个 character 读当前 word。
2. 把 characters push 到 stack。
3. 遇到 space 时，不断 pop 并 print。
4. 因为 stack 是 LIFO，pop 顺序会反转 word。

例子：

```text
push a, r, t, x, E
pop -> E, x, t, r, a
```

输出：

```text
Extra
```

完整输出：

```text
Extra cheese please
```

---

## 6. Trees and Graphs

### 6.1 Tree overview

Tree 是 non-linear data structure。

特点：

- 有 root node。
- 有 potentially many levels。
- 形成 hierarchy。
- nodes with no children are leaf nodes。
- nodes except root/leaf are internal nodes。
- general tree 中每个 node 可以有 many children。

### 6.2 Binary tree

Binary tree 中：

```text
each node has no more than two child nodes
```

通常存：

```java
Node left;
Node right;
```

Lecture 20 会专门讲 tree traversal 和 BST。

### 6.3 Graph

Graph 是 non-linear structure。

不同于 tree：

```text
A graph does not have a root.
Any node can be connected to any other node by an edge.
```

比喻：

```text
highway system connecting cities
```

### 6.4 Digraph

Directed graph / digraph：

```text
each edge has a specific direction
```

Directed edge 有时叫：

```text
arc
```

比喻：

```text
airline flights between airports
```

因为有些 flight 可能只从 A 到 B，不一定有 B 到 A。

### 6.5 Graph representation

Graphs / digraphs 可以用：

- dynamic links。
- arrays。

选择原则：

```text
Representation should facilitate intended operations.
```

---

## 7. Java Collections API and Generics

### 7.1 Java Collections API

Java standard library 有很多 collection classes。

例子：

```java
ArrayList
LinkedList
```

它们的 class names 往往暗示 underlying implementation。

常见 interfaces：

| Interface | Meaning |
|---|---|
| `List` | ordered collection, can contain duplicates |
| `Set` | collection with no duplicates |
| `SortedSet` | set with sorted order |
| `Map` | key-value mapping |
| `SortedMap` | map with sorted keys |

### 7.2 Generics

Java supports generic types：

```java
LinkedList<Book> myList = new LinkedList<Book>();
```

意思：

```text
myList stores Book objects.
```

Benefits：

1. Only objects of that type can be added。
2. When object is removed, its type is already established。
3. Less manual casting。
4. More compile-time type safety。

没有 generics 的写法：

```java
LinkedList myList = new LinkedList();
Object obj = myList.get(0);
Book book = (Book) obj;
```

有 generics：

```java
LinkedList<Book> myList = new LinkedList<Book>();
Book book = myList.get(0);
```

---

## 8. Complexity Quick Reference

本 lecture 没有深挖 complexity，但考试常见可记：

| Operation | Linked list with head only | Linked list with head+tail | ArrayList |
|---|---:|---:|---:|
| add at front | `O(1)` | `O(1)` | `O(n)` if shifting needed |
| add at end | `O(n)` | `O(1)` | amortized `O(1)` |
| remove front | `O(1)` | `O(1)` | `O(n)` if shifting |
| search by value | `O(n)` | `O(n)` | `O(n)` |
| access by index | `O(n)` | `O(n)` | `O(1)` |

Queue linked-list implementation:

```text
enqueue O(1) if rear is maintained
dequeue O(1)
```

Stack linked-list implementation:

```text
push O(1)
pop O(1)
peek O(1)
```

---

## 9. 考试答题模板

### 9.1 Insert after current in linked list

```java
newNode.next = current.next;
current.next = newNode;
```

必须先保存 old `current.next`。

### 9.2 Delete after current in singly linked list

```java
if (current.next != null) {
    current.next = current.next.next;
}
```

### 9.3 Delete after current in doubly linked list

```java
if (current.next != null) {
    current.next = current.next.next;

    if (current.next != null) {
        current.next.prev = current;
    }
}
```

### 9.4 Queue trace

```text
enqueue adds to rear.
dequeue removes from front.
First in, first out.
```

### 9.5 Stack trace

```text
push adds to top.
pop removes from top.
Last in, first out.
```

### 9.6 Explain generics

```text
Generics specify the type stored in a collection.
They prevent adding wrong types and avoid manual casts when retrieving elements.
```

---

## 10. 最容易错的点

### 10.1 Insert linked list pointer order

错误：

```java
current.next = newNode;
newNode.next = current.next;
```

这样 `newNode.next` 会指向自己或丢失原 node，取决于写法。

正确：

```java
newNode.next = current.next;
current.next = newNode;
```

### 10.2 Queue 和 Stack 搞反

Queue：

```text
FIFO
front remove
rear add
```

Stack：

```text
LIFO
top add/remove
```

### 10.3 Linked list traversal 不要移动 head

错误：

```java
while (list != null) {
    list = list.next;
}
```

如果 `list` 是 head field，会丢失整个 list。

正确：

```java
Node current = list;
while (current != null) {
    current = current.next;
}
```

### 10.4 Doubly linked list 要维护两个方向

删除/插入时不能只改 `next`，还要改相邻 node 的 `prev`。

### 10.5 `remove(0)` on ArrayList queue 可能慢

课件展示 `ArrayList` queue 版本：

```java
queue.remove(0)
```

概念上简单，但实际会 shift elements，可能 `O(n)`。

高效 queue 更适合 linked list 或 circular array。

---

## 11. 最后 30 秒记忆版

```text
Collection = object that organizes/manages other objects.
ADT = data + operations; describes what, not how.

Static data structure fixed size: array.
Dynamic data structure grows/shrinks at runtime: linked list.

Reference = pointer.
Node = data/object reference + link(s).

Insert after current:
newNode.next = current.next;
current.next = newNode;

Delete after current:
current.next = current.next.next;

Queue = FIFO.
enqueue rear, dequeue front.

Stack = LIFO.
push top, pop top, peek top without removing.

Tree = non-linear hierarchy with root.
Binary tree = at most two children.
Graph = no root, arbitrary edges.
Digraph = directed edges/arcs.

Java Collections API: ArrayList, LinkedList, List, Set, Map.
Generics: LinkedList<Book> stores Book objects with type safety.
```
