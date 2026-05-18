# Lecture 17: Recursion - Exam Notes

> 范围：`Lecture/17_Recursion.pdf`，主要是 recursive thinking、recursive programming、base case、recursive case、maze traversal、Towers of Hanoi。
> 读法：先看第 0 节速查，再重点看第 2 节递归执行流程、第 4 节 maze、第 5 节 Towers of Hanoi。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Recursion 必背

| 问题 | 答案 |
|---|---|
| recursive definition 是什么？ | 用被定义的 concept 自己来定义自己 |
| recursive method 是什么？ | method invokes itself |
| 每个 recursion 必须有什么？ | `base case` 和 `recursive case` |
| base case 作用 | 停止 recursion，避免 infinite recursion |
| recursive case 作用 | 把原问题变成 smaller/simpler version |
| direct recursion | method 直接调用自己 |
| indirect recursion | method A 调 B，B 调 C，最后又调回 A |
| 每次 recursive call 会怎样？ | 建立新的 execution environment，拥有自己的 parameters/local variables |
| recursion 结束后控制权去哪？ | 返回给调用它的 previous invocation |
| 什么时候不应该用 recursion？ | iterative solution 更简单清楚时，例如简单 summation |
| Maze traversal base cases | invalid move 或 reached destination |
| Towers of Hanoi base case | `numDisks == 1` |
| Towers of Hanoi moves | `2^n - 1` moves |

### 写 recursive method 模板

```java
returnType method(parameters) {
    if (base case) {
        return base result;
    } else {
        return combine(current work, method(smaller problem));
    }
}
```

### Recursion 检查清单

| 检查 | 必须满足 |
|---|---|
| 有 base case 吗？ | yes |
| 每次 recursive call 更接近 base case 吗？ | yes |
| base case 的 return value 对吗？ | yes |
| recursive case 是否正确 combine result？ | yes |
| 有没有可能无限递归？ | no |

---

## 1. Recursive Thinking

### 1.1 Recursive definition

`Recursive definition` 是：

```text
A definition that uses the word or concept being defined.
```

在英文词典里这样通常不好，因为会循环解释。

但在 programming / math 里，它可以非常自然。

### 1.2 List 的 recursive definition

课件例子：

```text
24, 88, 40, 37
```

可以递归定义 `List`：

```text
A List is:
1. a number
or
2. a number, comma, List
```

拆开看：

```text
24, 88, 40, 37
= number, List
= 24, (88, 40, 37)
= 24, (88, (40, 37))
= 24, (88, (40, (37)))
```

最后的 `37` 是 non-recursive part。

### 1.3 Base case

所有 recursion 都必须有：

```text
base case
```

base case 是不再 recursive 的情况。

如果没有 base case：

```text
infinite recursion
```

这类似 infinite loop，但发生在 method calls / definitions 里。

### 1.4 Recursive case

`recursive case` 是把问题变成更小的问题。

例子：

```text
N! = N * (N - 1)!
```

`N!` 用更小的 `(N-1)!` 来定义。

关键：

```text
The smaller problem must move toward the base case.
```

---

## 2. Recursive Factorial and Sum

### 2.1 Factorial definition

`N!` 表示：

```text
product of all integers between 1 and N inclusive
```

递归定义：

```text
1! = 1
N! = N * (N - 1)!
```

其中：

- `1! = 1` 是 base case。
- `N! = N * (N-1)!` 是 recursive case。

### 2.2 Factorial 展开例子

```text
5!
= 5 * 4!
= 5 * 4 * 3!
= 5 * 4 * 3 * 2!
= 5 * 4 * 3 * 2 * 1!
= 5 * 4 * 3 * 2 * 1
= 120
```

Call stack 视角：

```text
factorial(5)
  waits for factorial(4)
    waits for factorial(3)
      waits for factorial(2)
        waits for factorial(1)
          returns 1
        returns 2 * 1 = 2
      returns 3 * 2 = 6
    returns 4 * 6 = 24
  returns 5 * 24 = 120
```

### 2.3 Factorial code 模板

```java
public int factorial(int n) {
    if (n == 1) {
        return 1;
    } else {
        return n * factorial(n - 1);
    }
}
```

如果题目允许 `0!`：

```java
public int factorial(int n) {
    if (n == 0) {
        return 1;
    } else {
        return n * factorial(n - 1);
    }
}
```

本 lecture slides 用的是 `1! = 1`。

### 2.4 Quick Check：recursive definition of `5 * n`

题目：

```text
Write a recursive definition of 5 * n, where n > 0.
```

答案：

```text
5 * 1 = 5
5 * n = 5 + (5 * (n - 1))
```

解释：

```text
5 * 4 = 5 + 5 * 3
5 * 3 = 5 + 5 * 2
5 * 2 = 5 + 5 * 1
5 * 1 = 5
```

### 2.5 Sum from 1 to N

问题：

```text
sum(1..N)
```

递归定义：

```text
sum(1) = 1
sum(N) = N + sum(N - 1)
```

Java：

```java
public int sum(int num) {
    int result;

    if (num == 1) {
        result = 1;
    } else {
        result = num + sum(num - 1);
    }

    return result;
}
```

例子：

```text
sum(4)
= 4 + sum(3)
= 4 + 3 + sum(2)
= 4 + 3 + 2 + sum(1)
= 4 + 3 + 2 + 1
= 10
```

### 2.6 不是能递归就应该递归

课件强调：

```text
Just because we can use recursion does not mean we should.
```

`sum(1..N)` 用 loop 更简单：

```java
int sum = 0;
for (int i = 1; i <= n; i++) {
    sum += i;
}
```

所以：

- 如果 iterative version 更清楚，就用 iterative。
- 如果 problem 本身 naturally recursive，例如 tree、maze、Hanoi，recursion 更优雅。

---

## 3. Recursive Programming

### 3.1 Recursive method 的执行环境

每次 method call 都有自己的：

- parameters。
- local variables。
- return point。

所以 recursive method 调用自己时：

```text
Each call sets up a new execution environment.
```

例子：

```java
sum(3)
```

不是同一个 `num` 被不断改，而是：

```text
sum(3) has num = 3
sum(2) has num = 2
sum(1) has num = 1
```

各自独立。

### 3.2 Direct recursion

`Direct recursion`：

```java
void f() {
    f();
}
```

或更合理：

```java
int sum(int n) {
    if (n == 1) return 1;
    return n + sum(n - 1);
}
```

method 直接调用自己。

### 3.3 Indirect recursion

`Indirect recursion`：

```text
m1 calls m2
m2 calls m3
m3 calls m1
```

它仍然是 recursion，也必须有 base case。

难点：

```text
Indirect recursion is often more difficult to trace and debug.
```

因为 recursive cycle 分散在多个 methods 中。

### 3.4 Infinite recursion 常见原因

1. 没有 base case。
2. base case 条件写错。
3. recursive call 没有朝 base case 前进。
4. parameters 被错误更新。

错误例子：

```java
int sum(int n) {
    if (n == 1) return 1;
    return n + sum(n); // n did not decrease
}
```

会 infinite recursion。

正确：

```java
return n + sum(n - 1);
```

---

## 4. Maze Traversal

### 4.1 Maze problem

目标：

```text
Find a path from the top-left corner to the bottom-right corner.
```

Grid 中：

- `1` 表示可以走。
- `0` 表示 blocked。
- `3` 表示 tried but not on final path。
- `7` 表示 final path。

### 4.2 Recursive idea

从当前位置出发：

1. 如果当前位置 invalid，返回 `false`。
2. 如果当前位置是 destination，标记 path，返回 `true`。
3. 否则标记为 tried。
4. 依次尝试四个方向。
5. 如果任何方向能到终点，当前位置标记为 path，返回 `true`。
6. 如果四个方向都不行，返回 `false`。

### 4.3 Base cases

Maze traversal 的 base cases：

| Base case | Return |
|---|---|
| move out of bounds | `false` |
| cell is blocked (`0`) | `false` |
| cell already tried/path | `false` |
| reached destination | `true` |

### 4.4 `valid(row, column)` 模板

```java
private boolean valid(int row, int column) {
    boolean result = false;

    if (row >= 0 && row < grid.length &&
        column >= 0 && column < grid[row].length) {
        if (grid[row][column] == 1) {
            result = true;
        }
    }

    return result;
}
```

重点：

```text
Check bounds before accessing grid[row][column].
```

否则可能 `ArrayIndexOutOfBoundsException`。

### 4.5 `traverse(row, column)` 模板

```java
public boolean traverse(int row, int column) {
    boolean done = false;

    if (valid(row, column)) {
        grid[row][column] = TRIED;

        if (row == grid.length - 1 && column == grid[0].length - 1) {
            done = true;
        } else {
            done = traverse(row + 1, column);      // down
            if (!done) done = traverse(row, column + 1); // right
            if (!done) done = traverse(row - 1, column); // up
            if (!done) done = traverse(row, column - 1); // left
        }

        if (done) {
            grid[row][column] = PATH;
        }
    }

    return done;
}
```

方向顺序不一定唯一，但逻辑是：

```text
try possible directions recursively until one succeeds.
```

### 4.6 为什么要标记 `TRIED`？

如果不标记 visited/tried cells：

```text
The recursion may walk in circles forever.
```

例如从 A 到 B，再从 B 回到 A，不断重复。

所以一旦访问过某 cell，就先标记。

### 4.7 Maze output 怎么读？

如果最终 maze 有：

```text
7 = path
3 = tried but not final path
1 = open but unused
0 = wall
```

看到很多 `3` 表示：

```text
The algorithm tried those cells, but they led to dead ends.
```

---

## 5. Towers of Hanoi

### 5.1 Problem rules

Towers of Hanoi 有：

- three pegs。
- several disks。
- disks 大小不同，开始都在 one peg。

规则：

1. Only one disk can be moved at a time。
2. A larger disk cannot be put on top of a smaller disk。
3. Goal：move all disks from start peg to end peg。

### 5.2 Recursive insight

要把 `n` 个 disks 从 `start` 移到 `end`，借助 `temp`：

1. 把 top `n-1` disks 从 `start` 移到 `temp`。
2. 把 largest disk 从 `start` 移到 `end`。
3. 把 `n-1` disks 从 `temp` 移到 `end`。

这三步中，第 1 和第 3 步本身又是同一个 Hanoi problem。

### 5.3 Base case

```text
If numDisks == 1:
move one disk directly from start to end.
```

Java：

```java
if (numDisks == 1) {
    moveOneDisk(start, end);
}
```

### 5.4 Recursive method

```java
private void moveTower(int numDisks, int start, int end, int temp) {
    if (numDisks == 1) {
        moveOneDisk(start, end);
    } else {
        moveTower(numDisks - 1, start, temp, end);
        moveOneDisk(start, end);
        moveTower(numDisks - 1, temp, end, start);
    }
}
```

参数含义：

| Parameter | Meaning |
|---|---|
| `numDisks` | 当前要移动多少个 disks |
| `start` | source peg |
| `end` | destination peg |
| `temp` | temporary peg |

### 5.5 3 disks 的 move sequence

Move 3 disks from 1 to 3 using 2：

```text
moveTower(3, 1, 3, 2)

1. moveTower(2, 1, 2, 3)
   1. moveTower(1, 1, 3, 2): 1 -> 3
   2. moveOneDisk(1, 2):      1 -> 2
   3. moveTower(1, 3, 2, 1): 3 -> 2
2. moveOneDisk(1, 3):         1 -> 3
3. moveTower(2, 2, 3, 1)
   1. moveTower(1, 2, 1, 3): 2 -> 1
   2. moveOneDisk(2, 3):      2 -> 3
   3. moveTower(1, 1, 3, 2): 1 -> 3
```

最终 7 moves。

### 5.6 Number of moves

递推：

```text
T(1) = 1
T(n) = 2T(n - 1) + 1
```

解：

```text
T(n) = 2^n - 1
```

所以：

| disks | moves |
|---:|---:|
| 1 | 1 |
| 2 | 3 |
| 3 | 7 |
| 4 | 15 |
| 5 | 31 |

Lecture 示例用 4 disks，所以输出 15 lines。

---

## 6. Recursion vs Iteration

### 6.1 什么时候 recursion 好？

Recursion 适合：

- problem itself is recursive。
- problem can naturally split into smaller same-form problems。
- tree traversal。
- maze/backtracking。
- divide-and-conquer algorithms。
- Hanoi。

### 6.2 什么时候 iteration 好？

Iteration 适合：

- simple counting。
- simple summation。
- straight-line loops。
- recursion 只会让 code 更难读。

例子：

```text
sum 1..N is easier with a loop.
```

### 6.3 Recursion 的代价

每次 recursive call 都要建立新的 method call frame。

可能问题：

- memory overhead。
- too deep recursion can cause stack overflow。
- tracing/debugging can be harder。

考试一般不要求深入 JVM stack，但要理解：

```text
Every recursive call has its own parameters and local variables.
```

---

## 7. 考试答题模板

### 7.1 写 recursive definition

```text
Base case:
f(1) = ...

Recursive case:
f(n) = ... f(n - 1) ...
```

或根据题目：

```text
f(0) = ...
f(n) = ... f(n - 1) ...
```

重点：

```text
Use a smaller version of the same problem.
```

### 7.2 写 recursive Java method

```java
public int f(int n) {
    if (n == baseValue) {
        return baseResult;
    } else {
        return combine(n, f(n - 1));
    }
}
```

### 7.3 Trace recursion

用两阶段：

```text
Call phase: keep expanding until base case.
Return phase: compute results while returning.
```

例子：

```text
sum(3)
-> 3 + sum(2)
-> 3 + 2 + sum(1)
-> 3 + 2 + 1
-> 6
```

### 7.4 判断 infinite recursion

检查：

```text
Is there a base case?
Can the base case actually be reached?
Does each recursive call move closer to the base case?
```

如果任意一个答案是 no，就可能 infinite recursion。

---

## 8. 最容易错的点

### 8.1 忘记 base case

错误：

```java
int factorial(int n) {
    return n * factorial(n - 1);
}
```

没有停止条件。

### 8.2 Recursive call 没有变小

错误：

```java
return n + sum(n);
```

`n` 没有改变，永远到不了 base case。

### 8.3 Base case 写得太窄

如果 method 可能收到 `0`，但 base case 只有 `n == 1`，可能出错：

```java
sum(0) -> 0 + sum(-1) -> ...
```

考试要按题目 domain 处理。

如果题目说 `n > 0`，`n == 1` base case 可以。

### 8.4 Maze 不标记 visited

Maze recursion 中，如果不标记 `TRIED`，可能来回走无限循环。

### 8.5 Hanoi 三个 peg 参数顺序写错

正确：

```java
moveTower(numDisks - 1, start, temp, end);
moveOneDisk(start, end);
moveTower(numDisks - 1, temp, end, start);
```

第一步把小盘移到 temp。

第三步把小盘从 temp 移到 end。

---

## 9. 最后 30 秒记忆版

```text
Recursion = method invokes itself.
Every recursive solution needs:
1. base case
2. recursive case

Base case stops recursion.
Recursive case solves smaller same-form problem.

Each call has its own parameters and local variables.
When a call completes, control returns to the previous invocation.

factorial:
1! = 1
N! = N * (N - 1)!

sum:
sum(1) = 1
sum(N) = N + sum(N - 1)

Direct recursion: method calls itself.
Indirect recursion: method call chain eventually calls original method.

Maze base case:
invalid move -> false
destination -> true
mark tried cells to avoid cycles.

Hanoi:
move n-1 start -> temp
move 1 start -> end
move n-1 temp -> end
base case n == 1
moves = 2^n - 1
```
