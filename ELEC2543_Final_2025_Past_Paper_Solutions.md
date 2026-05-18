# ELEC2543 Final 2025 Past Paper Solutions

这份是答案版，不重复抄完整题目。重点写法按 open-book exam 使用：先给最终答案，再给可以直接写进答题纸的解释。

## Question 1 Multiple Choice

### Q1(i)

**Answer: a) Java Virtual Machine**

- a) Java Virtual Machine: correct. JVM 负责在某个平台上执行已经编译好的 Java bytecode，也就是 `.class` file。
- b) Java Compiler: incorrect. Compiler 的工作是把 `.java` source code 编译成 `.class` bytecode，不是执行 bytecode。
- c) Java Programming Manual: incorrect. Manual 只是文档，不会执行程序。
- d) Eclipse Editor: incorrect. Eclipse 是 IDE/editor，用来写和运行项目，但它本身不是 Java 语言执行 bytecode 的核心程序。

记忆：

```text
javac compiles source code.
JVM executes bytecode.
```

### Q1(ii)

**Answer: c) False**

- a) Null: incorrect for primitive `boolean`. `null` 是 object reference 的默认值。如果题目是 wrapper class `Boolean`，默认值才是 `null`。
- b) True: incorrect. Java 的 boolean 默认值不是 `true`。
- c) False: correct. Class field / instance variable / static variable 如果是 primitive `boolean`，默认值是 `false`。
- d) 0: incorrect. `0` 是 numeric primitive type 例如 `int` 的默认值，不是 boolean 的值。

注意：local variable 没有默认值，必须先 assign 才能使用。

### Q1(iii)

**Answer: c) Supports multiple inheritance**

- a) Platform-independent: incorrect as the answer, because this is a Java feature. Java source code compiled into bytecode, then different platforms use their own JVM to run it.
- b) High-performance: incorrect as the answer. Java usually被描述为有比较好的 performance，特别是有 JIT compilation。
- c) Supports multiple inheritance: correct. Java class 不支持 multiple inheritance。一个 class 只能 `extends` 一个 superclass。
- d) Automatic memory management: incorrect as the answer. Java 有 garbage collection，所以这是 Java feature。

重点：

```java
class C extends A, B { }      // invalid
class C implements X, Y { }   // valid if X and Y are interfaces
```

### Q1(iv)

**Answer: c) java.lang**

- a) java.util: incorrect. `java.util` 常见的是 `Scanner`, `Random`, `ArrayList`, `HashMap`。
- b) java.io: incorrect. `java.io` 主要是 file 和 input/output，例如 `File`, `FileReader`, `IOException`。
- c) java.lang: correct. `System`, `String`, `Math`, `Object` 都在 `java.lang`。
- d) java.text: incorrect. `java.text` 常用于格式化，例如 `NumberFormat`, `DecimalFormat`。

`java.lang` 会自动 imported，所以平时不需要写：

```java
import java.lang.String;
import java.lang.System;
```

### Q1(v)

**Answer: d) Compile-time exception**

- a) Checked exception: incorrect as the answer. Checked exception 是 Java exception 的正式分类，compiler 会要求你 handle 或 declare。
- b) Unchecked exception: incorrect as the answer. Unchecked exception 也是 Java exception 分类，compiler 不强制处理。
- c) Runtime exception: incorrect as the answer. `RuntimeException` 是 unchecked exception 的重要类别。
- d) Compile-time exception: correct. Java 常见分类是 checked exception / unchecked exception，不把 "compile-time exception" 作为 exception type。

例子：

```text
Checked: IOException, FileNotFoundException
Unchecked / Runtime: NullPointerException, ArithmeticException, ArrayIndexOutOfBoundsException
```

### Q1(vi)

**Answer: d) All of the above**

- a) To define a constant: correct but incomplete. `final` variable cannot be reassigned, so it is commonly used for constants.
- b) To prevent method overriding: correct but incomplete. `final` method cannot be overridden by subclass.
- c) To prevent inheritance: correct but incomplete. `final` class cannot be extended.
- d) All of the above: correct. `final` can apply to variables, methods, and classes.

Examples:

```java
final int MAX = 100;          // constant
public final void run() { }   // cannot override
public final class Test { }   // cannot extend
```

### Q1(vii)

**Answer: b) Runtime error**

- a) Compilation error: incorrect. A Java class can compile even if it does not contain `main`.
- b) Runtime error: correct. If you try to run that class directly, Java launcher cannot find the entry point and reports that the main method is missing.
- c) The program will run normally: incorrect. Without a valid `main`, there is no normal starting point for standalone execution.
- d) None of the above: incorrect because b is the best answer.

Valid main method:

```java
public static void main(String[] args)
```

### Q1(viii)

**Answer: a) It refers to the immediate superclass object**

- a) It refers to the immediate superclass object: correct. `super` is used to access superclass constructor, method, or field.
- b) It refers to the current class object: incorrect. Current object is `this`, not `super`.
- c) It refers to the constructor: incorrect. `super()` can call superclass constructor, but `super` itself means superclass side of the current object.
- d) It refers to a static method: incorrect. Static methods belong to class, and `super` is mainly about superclass instance members / constructor calls.

Examples:

```java
super();          // call superclass constructor
super.eat();      // call superclass method
super.name;       // access superclass field if visible
```

### Q1(ix)

**Answer: d) Compilation Error**

As written:

```java
String s = "blue";
s.concat("berry");
system.out.println(s);
```

- a) blue: would be the output if the last line were `System.out.println(s);`, because `String` is immutable and `concat` returns a new String without changing `s`. But the code as written has `system`, lowercase.
- b) blueberry: incorrect. Even if `System` were capitalized, `s.concat("berry")` does not modify `s` unless assigned back, for example `s = s.concat("berry");`.
- c) berry: incorrect. Nothing assigns `"berry"` to `s`.
- d) Compilation Error: correct. Java is case-sensitive. `System` must start with capital `S`; `system` is not the standard class.

Exam note: If the teacher treats lowercase `system` as a typo, then the conceptual output would be `blue`. But based on the code exactly printed, the MCQ answer is compilation error.

### Q1(x)

**Answer: a) Result: 1020**

Code:

```java
System.out.println("Result: " + 10 + 20);
```

- a) Result: 1020: correct. Evaluation is left-to-right. Once `"Result: " + 10` becomes a String, the following `+ 20` is also string concatenation.
- b) Result: 30: incorrect. That would require arithmetic first, such as `"Result: " + (10 + 20)`.
- c) Result: Error: incorrect. This is valid Java.
- d) Result: 2010: incorrect. There is no right-to-left concatenation here.

Key rule:

```text
"Result: " + 10 + 20
-> "Result: 10" + 20
-> "Result: 1020"
```

## Question 2 Program Output

### Q2(a)

**Output:**

```text
2 3 0 0 10 0
1 0 10 0 10 0
```

Detailed trace:

In `main`:

```java
int y = 1;
int x = 3;
int[] a = new int[4];   // [0, 0, 0, 0]
mystery(a, y, x);
```

The first call is:

```java
mystery(a, 1, 3)
```

Inside `mystery`, the parameter names are:

```java
public static void mystery(int[] a, int x, int y)
```

So inside the first call:

```text
x = 1
y = 3
a = [0, 0, 0, 0]
```

Since `x < y` is `1 < 3`, the `if` branch runs:

```java
x++;       // x becomes 2
a[x] = 10; // a[2] = 10
```

Then it prints:

```text
2 3 0 0 10 0
```

Back in `main`:

```java
x = y - 1;
```

Since `y` in main is still `1`, `x` becomes `0`. The array is still changed because arrays are objects and the method modified the same array:

```text
a = [0, 0, 10, 0]
```

The second call is:

```java
mystery(a, 1, 0)
```

Inside the second call:

```text
x = 1
y = 0
a = [0, 0, 10, 0]
```

Now `x < y` is `1 < 0`, false, so the `else` branch runs:

```java
a[y] = 10;  // a[0] = 10
```

Then it prints:

```text
1 0 10 0 10 0
```

Important concept:

```text
primitive int parameter: copied value, changing it inside method does not change main's variable.
array parameter: copied reference, changing array elements affects the original array object.
```

### Q2(b)

**Output:**

```text
abcde
```

Trace:

1. `System.out.print("a");` prints `a`.
2. Enter `try`.
3. `System.out.print("b");` prints `b`.
4. `throw new IllegalArgumentException();` throws an exception.
5. `IllegalArgumentException` is a subclass of `RuntimeException`, so the `catch (RuntimeException e)` block catches it.
6. Catch block prints `c`.
7. `finally` always runs, so it prints `d`.
8. After try-catch-finally finishes, execution continues after the block and prints `e`.

So the output is:

```text
a b c d e
```

Without spaces:

```text
abcde
```

### Q2(c)

**Output if the throw line is commented:**

```text
abde
```

Trace:

1. Print `a`.
2. Enter `try`.
3. Print `b`.
4. No exception is thrown.
5. `catch` is skipped because there is no exception.
6. `finally` still runs and prints `d`.
7. After the try-catch-finally block, print `e`.

So:

```text
a b d e
```

Without spaces:

```text
abde
```

### Q2(d)

**Output:**

```text
num1 == num2
num3 != num4
```

Explanation:

```java
Integer num1 = 100;
Integer num2 = 100;
Integer num3 = 500;
Integer num4 = 500;
```

`Integer` is a wrapper class, so variables like `num1` and `num2` are object references. For objects, `==` compares whether two references point to the same object, not whether their values are mathematically equal.

However, Java caches many small `Integer` objects, usually from `-128` to `127`. Therefore:

```java
Integer num1 = 100;
Integer num2 = 100;
```

usually refer to the same cached object, so:

```text
num1 == num2
```

For `500`, it is outside the usual cache range, so:

```java
Integer num3 = 500;
Integer num4 = 500;
```

usually create / refer to different objects, so:

```text
num3 != num4
```

Exam-safe note:

```java
num3.equals(num4)
```

would compare values and return `true`. But `num3 == num4` compares references.

## Question 3 Binary Search Tree

Numbers inserted:

```text
60, 41, 74, 16, 53, 65, 25, 46, 55, 63, 70
```

### Q3(a)

**Final BST:**

```text
            60
          /    \
        41      74
       /  \    /
     16   53  65
       \  / \ /  \
       25 46 55 63 70
```

Same tree, written more structurally:

```text
60
├── left: 41
│   ├── left: 16
│   │   └── right: 25
│   └── right: 53
│       ├── left: 46
│       └── right: 55
└── right: 74
    └── left: 65
        ├── left: 63
        └── right: 70
```

Insertion logic:

- 60 becomes root.
- 41 < 60, so 41 is left child of 60.
- 74 > 60, so 74 is right child of 60.
- 16 < 60 and 16 < 41, so 16 is left child of 41.
- 53 < 60 but 53 > 41, so 53 is right child of 41.
- 65 > 60 but 65 < 74, so 65 is left child of 74.
- 25 < 60, < 41, but > 16, so 25 is right child of 16.
- 46 < 60, > 41, < 53, so 46 is left child of 53.
- 55 < 60, > 41, > 53, so 55 is right child of 53.
- 63 > 60, < 74, < 65, so 63 is left child of 65.
- 70 > 60, < 74, > 65, so 70 is right child of 65.

### Q3(b)

**Traversal that gives sorted order: inorder traversal.**

Inorder means:

```text
left subtree -> root -> right subtree
```

For a BST, inorder traversal prints values from smallest to largest.

**Result:**

```text
16, 25, 41, 46, 53, 55, 60, 63, 65, 70, 74
```

### Q3(c)

**Preorder traversal:**

Preorder means:

```text
root -> left subtree -> right subtree
```

**Result:**

```text
60, 41, 16, 25, 53, 46, 55, 74, 65, 63, 70
```

Trace:

```text
Visit 60
Then left subtree of 60: 41, 16, 25, 53, 46, 55
Then right subtree of 60: 74, 65, 63, 70
```

### Q3(d)

**Postorder traversal:**

Postorder means:

```text
left subtree -> right subtree -> root
```

**Result:**

```text
25, 16, 46, 55, 53, 41, 63, 70, 65, 74, 60
```

Trace:

```text
Left subtree of 60 gives: 25, 16, 46, 55, 53, 41
Right subtree of 60 gives: 63, 70, 65, 74
Then root: 60
```

## Question 4 AVL Tree

### Q4(a)

**Minimum height of an AVL tree with n nodes:**

If height is counted by edges:

```text
minimum height = ceil(log2(n + 1)) - 1
```

If height is counted by number of levels/nodes on the longest path:

```text
minimum height = ceil(log2(n + 1))
```

Most data structure lectures define height as number of edges on the longest path from root to leaf, so the first formula is usually the expected one.

Reason:

For any binary tree with height `h` counted by edges, the maximum number of nodes is:

```text
2^(h + 1) - 1
```

To store `n` nodes with minimum possible height:

```text
n <= 2^(h + 1) - 1
n + 1 <= 2^(h + 1)
log2(n + 1) <= h + 1
h >= log2(n + 1) - 1
```

Therefore:

```text
h = ceil(log2(n + 1)) - 1
```

### Q4(b)

**Complexities in AVL tree:**

```text
search:    O(log n)
insertion: O(log n)
deletion:  O(log n)
```

Explanation:

An AVL tree keeps itself balanced. The height of the tree is always `O(log n)`, so search follows at most one path from root to leaf, which is `O(log n)`.

Insertion and deletion first do normal BST insertion/deletion along a root-to-leaf path, then rebalance on the way back using rotations. The number of checked ancestors is proportional to height, and each rotation is `O(1)`, so insertion and deletion are also `O(log n)`.

### Q4(c)

Initial AVL tree:

```text
        60
       /  \
     20    100
          /   \
        80     120
```

Insert `70` using normal BST rule:

```text
70 > 60, go right to 100
70 < 100, go left to 80
70 < 80, insert as left child of 80
```

Tree immediately after insertion:

```text
        60
       /  \
     20    100
          /   \
        80     120
       /
     70
```

Now check balance:

- Node 80 is still balanced: left height 0, right height -1 if empty tree has height -1.
- Node 100 is still balanced: left subtree height 1, right subtree height 0.
- Node 60 becomes unbalanced: left subtree height 0, right subtree height 2, difference is 2.

The inserted value went to:

```text
right child of 60 -> left child of 100
```

So this is a **Right-Left (RL) case** at node 60.

For RL case, use two rotations:

1. Right rotation at 100.
2. Left rotation at 60.

Step 1: right rotation at 100:

```text
        60
       /  \
     20    80
          /  \
        70    100
                \
                120
```

Step 2: left rotation at 60:

```text
          80
        /    \
      60      100
     /  \       \
   20    70      120
```

**Final updated AVL tree:**

```text
          80
        /    \
      60      100
     /  \       \
   20    70      120
```

Check final tree:

- Node 60 has children 20 and 70, balanced.
- Node 100 has right child 120 only, balanced.
- Root 80 has left subtree height 1 and right subtree height 1, balanced.

## Question 5 Sorting

Array:

```text
2, 6, 5, 7, 9, 8, 3, 4
```

### Q5(a) Merge Sort

Merge sort idea:

```text
split into halves -> sort each half -> merge sorted halves
```

Steps:

```text
[2, 6, 5, 7, 9, 8, 3, 4]

Split:
[2, 6, 5, 7]            [9, 8, 3, 4]

Split again:
[2, 6] [5, 7]           [9, 8] [3, 4]

Split to single elements:
[2] [6] [5] [7]         [9] [8] [3] [4]

Merge pairs:
[2, 6] [5, 7]           [8, 9] [3, 4]

Merge halves:
[2, 5, 6, 7]            [3, 4, 8, 9]

Final merge:
[2, 3, 4, 5, 6, 7, 8, 9]
```

Final sorted array:

```text
2, 3, 4, 5, 6, 7, 8, 9
```

### Q5(b) Quick Sort

Since the question does not specify the pivot rule, use a clear common rule: **choose the first element as pivot**.

Quick sort idea:

```text
choose pivot -> partition smaller values to left and larger values to right -> recursively sort both sides
```

Steps:

```text
[2, 6, 5, 7, 9, 8, 3, 4]
pivot = 2
left = []
right = [6, 5, 7, 9, 8, 3, 4]
=> [] 2 [6, 5, 7, 9, 8, 3, 4]
```

Sort the right side:

```text
[6, 5, 7, 9, 8, 3, 4]
pivot = 6
left = [5, 3, 4]
right = [7, 9, 8]
=> [5, 3, 4] 6 [7, 9, 8]
```

Sort `[5, 3, 4]`:

```text
[5, 3, 4]
pivot = 5
left = [3, 4]
right = []
=> [3, 4] 5 []
```

Sort `[3, 4]`:

```text
[3, 4]
pivot = 3
left = []
right = [4]
=> [] 3 [4]
=> [3, 4]
```

So the left part of 6 becomes:

```text
[3, 4, 5]
```

Sort `[7, 9, 8]`:

```text
[7, 9, 8]
pivot = 7
left = []
right = [9, 8]
=> [] 7 [9, 8]
```

Sort `[9, 8]`:

```text
[9, 8]
pivot = 9
left = [8]
right = []
=> [8] 9 []
=> [8, 9]
```

So the right part of 6 becomes:

```text
[7, 8, 9]
```

Combine:

```text
[] 2 [3, 4, 5, 6, 7, 8, 9]
=> [2, 3, 4, 5, 6, 7, 8, 9]
```

Final sorted array:

```text
2, 3, 4, 5, 6, 7, 8, 9
```

Exam note: Quick Sort 的中间步骤可能因为 pivot choice 不同而不同。只要 partition 逻辑合理，最后 sorted result 一样。

### Q5(c)

**Merge Sort average time complexity:**

```text
O(n log n)
```

**Merge Sort worst case time complexity:**

```text
O(n log n)
```

Reason:

Merge Sort always splits the array into two halves, giving about `log n` levels. At each level, merging all elements together costs `O(n)`. Therefore:

```text
O(n) per level * O(log n) levels = O(n log n)
```

This does not depend much on whether the input is already sorted or not, so average and worst case are both `O(n log n)`.

### Q5(d)

**Quick Sort average time complexity:**

```text
O(n log n)
```

**Quick Sort worst case time complexity:**

```text
O(n^2)
```

Reason:

In the average case, the pivot splits the array reasonably evenly, so there are about `log n` levels and each level does `O(n)` partition work:

```text
average = O(n log n)
```

In the worst case, the pivot is always the smallest or largest element, so the split is extremely unbalanced:

```text
n-1 elements on one side, 0 elements on the other side
```

Then the work becomes:

```text
n + (n - 1) + (n - 2) + ... + 1 = O(n^2)
```

## Problem 6 OOP

### Q6(a)

One correct implementation:

```java
class Animal {
    public void eat() {
        System.out.println("The animal is eating");
    }
}

class Bird extends Animal {
    @Override
    public void eat() {
        System.out.println("The bird is eating worms");
    }
}

class Fish extends Animal {
    @Override
    public void eat() {
        System.out.println("The fish is eating shrimps");
    }
}
```

Explanation:

- `Animal` is the superclass.
- `Bird extends Animal`, so Bird is a subclass of Animal.
- `Fish extends Animal`, so Fish is also a subclass of Animal.
- Both `Bird` and `Fish` override `eat()`.
- When the subclass version of `eat()` is called, Java uses the overridden method in the subclass.

Example test:

```java
Animal a1 = new Bird();
Animal a2 = new Fish();

a1.eat();  // The bird is eating worms
a2.eat();  // The fish is eating shrimps
```

This demonstrates polymorphism: the variable type can be `Animal`, but the actual object decides which overridden method runs.

### Q6(b)

One correct implementation:

```java
interface Animal {
    void eat();
}

interface Wings {
    void fly();
}

class Eagle implements Animal, Wings {
    @Override
    public void eat() {
        System.out.println("The eagle is eating");
    }

    @Override
    public void fly() {
        System.out.println("The eagle is flying");
    }
}
```

Example test:

```java
Eagle eagle = new Eagle();
eagle.eat();  // The eagle is eating
eagle.fly();  // The eagle is flying
```

Explanation:

- `Animal` is an interface with the required behavior `eat()`.
- `Wings` is another interface with the required behavior `fly()`.
- `Eagle implements Animal, Wings`, meaning Eagle promises to provide both methods.
- Java class can implement multiple interfaces, so this avoids the single-inheritance limitation of classes.

### Q6(c)

**Preferred answer: interfaces, if the goal is flexibility.**

Reason:

Java only allows a class to extend one superclass, but it allows a class to implement multiple interfaces. In this question, `Eagle` needs both eating behavior and flying behavior. Interfaces are better for modeling abilities or contracts such as `eat()` and `fly()`, because one class can combine several abilities.

A good exam answer:

```text
I prefer the interface implementation because it is more flexible. Java does not support multiple inheritance for classes, so a class can only extend one superclass. However, a class can implement multiple interfaces. In this example, Eagle can implement both Animal and Wings, so it can have both eat() and fly() behavior. Interfaces are suitable when we want to define required methods without forcing all classes to share the same superclass implementation.
```

However, inheritance is still useful when subclasses truly share common code:

```text
If Bird and Fish share common fields or methods from Animal, class inheritance is useful because it expresses an is-a relationship and supports code reuse.
```

So the balanced final answer:

```text
Use class inheritance when there is a strong is-a relationship and shared implementation. Use interfaces when the design is based on abilities or when a class needs to combine multiple behaviors. For this question, I prefer interfaces because Eagle can implement both Animal and Wings.
```
