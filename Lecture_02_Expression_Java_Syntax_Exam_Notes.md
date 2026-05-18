# Lecture 2: Expression and Java Syntax - Exam Notes

> 范围：`Lecture/02_Expression.pdf`，主要是 Java vs Python syntax、primitive types、variables/constants、expressions/operators、conditions、loops、strings、escape sequences、data conversion、Scanner input。
> 读法：先看第 0 节速查，再重点看第 2 节 expression/operator、第 4 节 control flow、第 6 节 data conversion。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Java Syntax 必背

| 问题 | 答案 |
|---|---|
| Java variable 使用前要不要 declaration？ | 要 |
| Java variable type 可以中途变吗？ | 不可以，必须保存 declared type 的 value |
| Java 用什么定义 scope？ | braces `{}` |
| Java statement 结束符 | semicolon `;` |
| Java condition 必须是什么 type？ | `boolean` expression |
| Java boolean operators | `!`, `&&`, `||` |
| Java 可以 chained comparison 吗？ | 不可以，不能写 `a < b < c` |
| primitive types 有几个？ | 8 |
| integer primitive types | `byte`, `short`, `int`, `long` |
| floating point primitive types | `float`, `double` |
| character type | `char` |
| boolean type | `boolean` |
| constant keyword | `final` |
| integer division | if both operands are integers, `/` returns integer result |
| remainder operator | `%` |
| post-increment | `j = k++;` means `j = k; k = k + 1;` |
| pre-increment | `j = ++k;` means `k = k + 1; j = k;` |
| String concatenation | `+` |
| keyboard input class | `Scanner` |
| Scanner package | `java.util` |

### Primitive Types

| Type | Meaning |
|---|---|
| `byte` | 8-bit integer, `-128` to `127` |
| `short` | 16-bit integer |
| `int` | 32-bit integer |
| `long` | 64-bit integer |
| `float` | floating point, about 6 digits precision |
| `double` | floating point, about 15 digits precision |
| `char` | Unicode character |
| `boolean` | `true` or `false` only |

---

## 1. Java vs Python Differences

### 1.1 Variables

Python：

```python
x = 3
x = "hello"
```

same variable 可以保存不同 type。

Java：

```java
int x;
x = 3;
x = "hello"; // compile error
```

Java variable 必须先声明 type，之后只能保存 compatible type。

### 1.2 Scope

Python 用 indentation。

Java 用 braces：

```java
if (a > 0) {
    b = -1;
}
```

Indentation 不改变 Java semantics，只影响 readability。

### 1.3 Statement ending

Java statements 通常用 semicolon 结束：

```java
int total = 0;
total += 5;
```

Missing semicolon 是常见 compile error。

### 1.4 Inheritance preview

Lecture 2 提到：

```text
Python supports multiple inheritance.
Java has single inheritance.
```

Java class inheritance 只能 `extends` 一个 parent class；后面 Lecture 14 详细讲。

---

## 2. Primitive Data Types

### 2.1 Eight primitive types

Java 有 8 种 primitive data types：

```text
byte, short, int, long, float, double, char, boolean
```

除了这些，其他大多数是 objects / references。

### 2.2 Integer types

| Type | Range / size |
|---|---|
| `byte` | `-128` to `127`, 8 bits |
| `short` | about `-32768` to `32767`, 16 bits |
| `int` | about `-2.1 billion` to `2.1 billion`, 32 bits |
| `long` | 64 bits |

考试默认整数常用 `int`。

### 2.3 Floating point types

| Type | Precision |
|---|---|
| `float` | about 6 digits |
| `double` | about 15 digits |

默认小数常用 `double`。

### 2.4 `char`

`char` 存 Unicode character。

例子：

```java
char grade = 'A';
```

注意：

- single quotes for `char`
- double quotes for `String`

```java
'A'    // char
"A"    // String
```

### 2.5 `boolean`

`boolean` 只有：

```java
true
false
```

不能用 `0` / `1` 代替。

```java
boolean done = false;
```

---

## 3. Variables, Constants, Expressions

### 3.1 Variable declaration

Syntax：

```java
<type> <variableName>;
```

例子：

```java
int total;
double price;
boolean done;
```

可以 declaration + initialization：

```java
int total = 0;
double rate = 0.06;
```

### 3.2 Constants

Constant 是整个 existence 中 value 不变的 identifier。

Java 用：

```java
final int MIN_HEIGHT = 69;
```

Convention：

```text
constants use UPPER_CASE_WITH_UNDERSCORES
```

### 3.3 Expressions

Expression 是：

```text
combination of operators and operands
```

Arithmetic operators：

| Operator | Meaning |
|---|---|
| `+` | addition / string concatenation |
| `-` | subtraction |
| `*` | multiplication |
| `/` | division |
| `%` | remainder |

### 3.4 Integer division

如果 `/` 两边都是 integers，结果也是 integer。

```java
int result = 7 / 2; // 3
```

小数部分被丢掉。

如果想要 floating result：

```java
double result = (double) 7 / 2; // 3.5
```

### 3.5 Increment / decrement

Post-increment：

```java
j = k++;
```

等价：

```java
j = k;
k = k + 1;
```

Pre-increment：

```java
j = ++k;
```

等价：

```java
k = k + 1;
j = k;
```

例子：

```java
int k = 5;
int j = k++;
// j = 5, k = 6
```

```java
int k = 5;
int j = ++k;
// k = 6, j = 6
```

### 3.6 Assignment operators

```java
j += k;
```

等价：

```java
j = j + k;
```

其他：

```java
-=, *=, /=, %=
```

---

## 4. Operator Precedence

### 4.1 Main rule

Java operator precedence generally follows algebra：

1. Parentheses first。
2. Multiplication/division/remainder before addition/subtraction。
3. Same precedence arithmetic operators evaluated left to right。
4. Assignment has lower precedence than arithmetic。

### 4.2 Example

```java
result = total + count / max - offset;
```

先：

```text
count / max
```

再：

```text
total + ...
```

再：

```text
... - offset
```

最后 assignment to `result`。

### 4.3 Force order with parentheses

```java
a / (b + c) - d % e
```

先算：

```text
b + c
```

再：

```text
a / (b + c)
d % e
```

再 subtraction。

### 4.4 Assignment revisited

```java
answer = sum / 4 + MAX * lowest;
```

顺序：

1. `sum / 4`
2. `MAX * lowest`
3. add two results
4. store into `answer`

Assignment right side 先 complete，结果再存到 left side variable。

---

## 5. Conditions and Control Flow

### 5.1 Boolean expressions

Java condition 必须是 `boolean`。

错误：

```java
if (3) { } // invalid in Java
```

正确：

```java
if (x != 0) { }
```

### 5.2 Comparison operators

```java
<, >, ==, <=, >=, !=
```

注意：

```text
comparisons cannot be chained
```

错误：

```java
if (a < b < c) { }
```

正确：

```java
if (a < b && b < c) { }
```

### 5.3 Boolean operators

| Java | Meaning | Python equivalent |
|---|---|---|
| `!` | not | `not` |
| `&&` | and | `and` |
| `||` | or | `or` |

### 5.4 `if-else`

```java
if (condition) {
    statement1;
} else if (condition) {
    statement2;
} else {
    statement3;
}
```

If only one statement is inside scope, braces can be omitted：

```java
if (a > 0)
    b = -1;
```

但考试写代码建议保留 braces，减少错误。

### 5.5 `switch-case`

Template：

```java
switch (expression) {
    case value:
        statement;
        break;
    case value2:
        statement;
        break;
    default:
        statement;
}
```

注意：

- `break` prevents fall-through。
- `default` optional。
- 多个 case 可以共享 statement：

```java
case 'C':
case 'D':
    System.out.println("You passed");
    break;
```

### 5.6 Loops

`while`：

```java
while (condition) {
    statement;
}
```

`for`：

```java
for (initialization; booleanExpression; update) {
    statement;
}
```

例子：

```java
for (int x = 10; x < 20; x++) {
    System.out.println(x);
}
```

---

## 6. Strings and Output

### 6.1 String

String 是 character sequence。

```java
String text = "This is a string.";
```

Every character string is an object in Java, defined by `String` class。

### 6.2 String concatenation

`+` 可以 concatenate strings：

```java
"Peanut butter " + "and jelly"
```

如果 String 和 number 一起 `+`：

```java
"Answer: " + 5
```

结果是：

```text
Answer: 5
```

### 6.3 `print` vs `println`

```java
System.out.print("A");
System.out.print("B");
```

输出：

```text
AB
```

```java
System.out.println("A");
System.out.println("B");
```

输出：

```text
A
B
```

`println` prints and advances to next line。

### 6.4 Escape sequences

Escape sequence starts with backslash `\`。

| Sequence | Meaning |
|---|---|
| `\b` | backspace |
| `\t` | tab |
| `\n` | newline |
| `\"` | double quote |
| `\'` | single quote |
| `\\` | backslash |

例子：

```java
System.out.println("I said \"Hello\" to you.");
```

输出：

```text
I said "Hello" to you.
```

---

## 7. Data Conversion

### 7.1 Conversion does not change original variable

Data conversion converts a value as part of a computation。

它不会改变 variable 本身的 declared type。

例子：

```java
int dollars = 10;
float money;
money = dollars;
```

`dollars` 仍然是 `int`。

### 7.2 Widening vs narrowing

| Conversion | Meaning | Risk |
|---|---|---|
| widening | small type to larger type | safer |
| narrowing | large type to smaller type | may lose information |

例子：

```java
short -> int      // widening
int -> short      // narrowing
int -> double     // widening
double -> int     // narrowing
```

### 7.3 Three ways conversions happen

Lecture list：

1. assignment conversion。
2. promotion。
3. casting。

### 7.4 Assignment conversion

Assignment conversion happens when assigning one type to another。

Only widening conversions can happen automatically via assignment。

```java
float money;
int dollars = 10;
money = dollars; // ok, int to float widening
```

But：

```java
int x;
double y = 3.5;
x = y; // compile error without cast
```

### 7.5 Promotion

Promotion happens automatically in expressions。

```java
float sum = 100;
int count = 10;
float result = sum / count;
```

`count` is promoted to float for the division。

`count` variable itself remains `int`。

### 7.6 Casting

Casting explicitly converts value：

```java
result = (float) total / count;
```

如果 `total` 和 `count` 都是 int，这能避免 integer division。

Casting powerful but dangerous，因为 narrowing may lose information。

```java
int x = (int) 3.9; // x = 3
```

---

## 8. Reading Input with `Scanner`

### 8.1 Create scanner

`Scanner` 在 `java.util` package。

```java
import java.util.Scanner;

Scanner scan = new Scanner(System.in);
```

`System.in` represents keyboard input。

### 8.2 Common Scanner methods

| Method | Reads |
|---|---|
| `nextLine()` | whole line as String |
| `nextInt()` | int |
| `nextDouble()` | double |

### 8.3 GasMileage example

```java
import java.util.Scanner;

public class GasMileage {
    public static void main(String[] args) {
        int miles;
        double gallons, mpg;

        Scanner scan = new Scanner(System.in);

        System.out.print("Enter the number of miles: ");
        miles = scan.nextInt();

        System.out.print("Enter the gallons of fuel used: ");
        gallons = scan.nextDouble();

        mpg = miles / gallons;
        System.out.println("Miles Per Gallon: " + mpg);
    }
}
```

如果 first question 输入不是 integer：

```text
Scanner input mismatch / runtime exception
```

后面 Exception lecture 会正式讲。

---

## 9. 考试答题模板

### 9.1 Determine expression result

Checklist：

```text
1. Parentheses.
2. *, /, % before +, -.
3. Same precedence left to right.
4. Check integer division.
5. Apply assignment last.
```

### 9.2 Convert chained comparison

Python style：

```text
a < b < c
```

Java：

```java
a < b && b < c
```

### 9.3 Force floating-point division

```java
double result = (double) total / count;
```

or：

```java
double result = total / (double) count;
```

### 9.4 Scanner input template

```java
import java.util.Scanner;

Scanner scan = new Scanner(System.in);
int n = scan.nextInt();
double x = scan.nextDouble();
String line = scan.nextLine();
```

Be careful when mixing `nextInt()` and `nextLine()` because newline remains in input buffer。

---

## 10. 最容易错的点

### 10.1 Integer division

```java
double x = 5 / 2;
```

结果：

```text
2.0
```

因为 `5 / 2` 先 integer division 得到 `2`，再 assignment conversion to `double`。

正确：

```java
double x = (double) 5 / 2; // 2.5
```

### 10.2 `=` vs `==`

```java
x = 5;   // assignment
x == 5;  // equality comparison
```

### 10.3 `true` / `false` 不是 numbers

错误：

```java
if (1) { }
```

正确：

```java
if (x != 0) { }
```

### 10.4 `char` vs `String`

```java
'A'  // char
"A"  // String
```

### 10.5 `break` in switch

Missing `break` 会 fall-through 到下一个 case。

---

## 11. 最后 30 秒记忆版

```text
Java variable needs type declaration.
Variable keeps values of declared type.
Braces define scopes; semicolons end statements.

8 primitive types:
byte short int long float double char boolean.
boolean only true/false.

final declares constant.

Expression = operators + operands.
Arithmetic: + - * / %
Integer / integer gives integer.

k++ returns old value then increments.
++k increments then returns new value.

Condition must be boolean.
Boolean operators: ! && ||
No chained comparisons; use a < b && b < c.

if/else, switch/case/break, while, for.

String uses double quotes; char uses single quotes.
print stays on same line; println moves to next line.

Data conversion:
assignment conversion, promotion, casting.
Widening safer; narrowing can lose information.

Scanner is in java.util.
Scanner scan = new Scanner(System.in);
nextLine, nextInt, nextDouble.
```
