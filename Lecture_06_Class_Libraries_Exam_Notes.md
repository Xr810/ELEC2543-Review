# Lecture 6: Class Libraries - Exam Notes

> 范围：`Lecture/06_Class_Libraries.pdf`，主要是 Java standard class library、packages、`import`、`String`、`Math`、`Random`、`NumberFormat`、`DecimalFormat`。
> 读法：先看第 0 节速查，再重点看第 2 节 import、第 3 节 String immutability、第 4-5 节 Math/Random/formatting。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Class Libraries 必背

| 问题 | 答案 |
|---|---|
| class library 是什么？ | collection of classes we can use when developing programs |
| Java standard class library 是 Java language 本身吗？ | 不是 language 本身，但 development environment 都包含 |
| classes 如何组织？ | packages |
| `java.lang` 是否自动 import？ | yes |
| 为什么不用 import `System` / `String`？ | both in `java.lang`，automatic import |
| `Scanner` package | `java.util` |
| import one class | `import java.util.Scanner;` |
| import whole package | `import java.util.*;` |
| fully qualified name | `java.util.Scanner` |
| String 是否 immutable？ | yes，value and length cannot be changed |
| `string.charAt(index)` index 从几开始？ | 0 |
| `"Hello".charAt(0)` | `'H'` |
| `"Hello".charAt(4)` | `'o'` |
| `Math` methods 是否 static？ | yes，用 `Math.sqrt(...)` |
| `Random` package | `java.util` |
| `nextInt(10)` range | `0` to `9` |
| `nextFloat()` range | `[0, 1)` |
| `NumberFormat` / `DecimalFormat` package | `java.text` |

---

## 1. Class Libraries and Packages

### 1.1 Class library

`Class library` 是：

```text
a collection of classes that we can use when developing programs
```

Java standard class library：

- included in Java development environment。
- not part of the core Java language syntax。
- heavily used in real programs。

Examples used so far：

```java
System
Scanner
String
Math
Random
NumberFormat
DecimalFormat
```

### 1.2 Packages

Classes in Java standard class library are organized into packages。

Examples：

| Package | Purpose |
|---|---|
| `java.lang` | general support |
| `java.applet` | applets for web |
| `java.awt` | graphics and GUI |
| `javax.swing` | additional GUI |
| `java.net` | network communication |
| `java.util` | utilities |
| `javax.xml.parsers` | XML document processing |

Exam focus：

```text
java.lang, java.util, java.text
```

---

## 2. The `import` Declaration

### 2.1 Fully qualified name

You can use a class with full package name：

```java
java.util.Scanner scan = new java.util.Scanner(System.in);
```

This works but is verbose。

### 2.2 Import one class

```java
import java.util.Scanner;
```

Then：

```java
Scanner scan = new Scanner(System.in);
```

### 2.3 Import whole package

```java
import java.util.*;
```

This imports all classes in that package。

Then can use：

```java
Scanner
Random
ArrayList
```

without full package prefix。

### 2.4 `java.lang` automatic import

All classes in `java.lang` are imported automatically。

Equivalent idea：

```java
import java.lang.*;
```

This is why earlier programs did not import：

```java
System
String
Math
Integer
```

But `Scanner` is in `java.util`，so need import。

---

## 3. The `String` Class

### 3.1 Create String

Using `new`：

```java
String name = new String("ELEC 2543");
```

Common shorthand：

```java
String name = "ELEC 2543";
```

Both create/reference a `String` object。

### 3.2 String immutability

Lecture key point：

```text
Once a String object has been created, neither its value nor its length can be changed.
```

So `String` is immutable。

Example：

```java
String name = "ELEC 2543";
name = name.replace('E', 'X');
```

What happens：

1. Original `"ELEC 2543"` object is not modified。
2. `replace` returns a new String `"XLXC 2543"`。
3. `name` reference now points to new String。

### 3.3 String indexes

Use：

```java
string.charAt(index)
```

Indexes begin at zero。

For:

```text
"Hello"
```

| index | char |
|---:|---|
| 0 | `H` |
| 1 | `e` |
| 2 | `l` |
| 3 | `l` |
| 4 | `o` |

So：

```java
"Hello".charAt(4) // 'o'
```

Invalid index causes runtime error later known as `StringIndexOutOfBoundsException`。

### 3.4 How many String objects?

If:

```java
String s = "ELEC 2543";
s = s.replace('E', 'X');
```

At least two logical String objects are involved：

```text
"ELEC 2543"
"XLXC 2543"
```

Because Strings are immutable。

---

## 4. The `Math` and `Random` Classes

### 4.1 `Math`

`Math` is in `java.lang`。

Its methods are static/class methods：

```java
value = Math.cos(90) + Math.sqrt(delta);
```

No `Math` object needed：

```java
Math m = new Math(); // not how we use it
```

Common constants/methods：

```java
Math.PI
Math.sqrt(x)
Math.cos(x)
Math.abs(x)
Math.random()
```

### 4.2 `Random`

`Random` is in `java.util`。

Need：

```java
import java.util.Random;
```

Create object：

```java
Random generator = new Random();
```

Use methods：

```java
int num1 = generator.nextInt();
int num2 = generator.nextInt(10);
float num3 = generator.nextFloat();
```

### 4.3 Random ranges

| Method | Range / meaning |
|---|---|
| `nextInt()` | any int range |
| `nextInt(10)` | integer `0` to `9` |
| `nextFloat()` | float in `[0, 1)` |

If want 1 to 6：

```java
int roll = generator.nextInt(6) + 1;
```

---

## 5. Formatting Output

### 5.1 `NumberFormat`

`NumberFormat` is in：

```java
java.text
```

Need：

```java
import java.text.NumberFormat;
```

Create via class methods, not `new`：

```java
NumberFormat money = NumberFormat.getCurrencyInstance();
NumberFormat percent = NumberFormat.getPercentInstance();
```

Format：

```java
money.format(subtotal)
percent.format(TAX_RATE)
```

### 5.2 Purchase example

```java
final double TAX_RATE = 0.06;
int quantity = 3;
double unitPrice = 4;

double subtotal = quantity * unitPrice;
double tax = subtotal * TAX_RATE;
double totalCost = subtotal + tax;

NumberFormat fmt1 = NumberFormat.getCurrencyInstance();
NumberFormat fmt2 = NumberFormat.getPercentInstance();

System.out.println("Subtotal: " + fmt1.format(subtotal));
System.out.println("Tax: " + fmt1.format(tax) + " at " + fmt2.format(TAX_RATE));
System.out.println("Total: " + fmt1.format(totalCost));
```

Output idea：

```text
Subtotal: HKD12.00
Tax: HKD0.72 at 6%
Total: HKD12.72
```

Exact currency symbol can depend on locale。

### 5.3 `DecimalFormat`

`DecimalFormat` also in `java.text`。

Need：

```java
import java.text.DecimalFormat;
```

Use `new` with pattern：

```java
DecimalFormat fmt = new DecimalFormat("0.####");
System.out.println("The value of PI is " + fmt.format(Math.PI));
```

Pattern `"0.####"` means up to 4 digits after decimal。

Output：

```text
The value of PI is 3.1416
```

### 5.4 `NumberFormat` vs `DecimalFormat`

| Class | Creation | Use |
|---|---|---|
| `NumberFormat` | class method like `getCurrencyInstance()` | currency / percent formatting |
| `DecimalFormat` | `new DecimalFormat(pattern)` | custom decimal pattern |

---

## 6. Common Package Memory

| Class | Package | Import needed? |
|---|---|---|
| `String` | `java.lang` | no |
| `System` | `java.lang` | no |
| `Math` | `java.lang` | no |
| `Integer` | `java.lang` | no |
| `Scanner` | `java.util` | yes |
| `Random` | `java.util` | yes |
| `ArrayList` | `java.util` | yes |
| `NumberFormat` | `java.text` | yes |
| `DecimalFormat` | `java.text` | yes |

---

## 7. 考试答题模板

### 7.1 Use Scanner

```java
import java.util.Scanner;

Scanner scan = new Scanner(System.in);
```

### 7.2 Use Random

```java
import java.util.Random;

Random generator = new Random();
int value = generator.nextInt(6) + 1;
```

### 7.3 Use currency and percent format

```java
import java.text.NumberFormat;

NumberFormat money = NumberFormat.getCurrencyInstance();
NumberFormat percent = NumberFormat.getPercentInstance();

System.out.println(money.format(total));
System.out.println(percent.format(rate));
```

### 7.4 Use decimal pattern

```java
import java.text.DecimalFormat;

DecimalFormat fmt = new DecimalFormat("0.####");
System.out.println(fmt.format(Math.PI));
```

### 7.5 Explain String immutability

```text
String objects are immutable.
Methods such as replace return a new String object.
The original String object is not changed.
```

---

## 8. 最容易错的点

### 8.1 `java.lang` does not need import

No need：

```java
import java.lang.String;
import java.lang.System;
```

They are automatic。

### 8.2 `Scanner` needs import

Need：

```java
import java.util.Scanner;
```

### 8.3 String index starts at 0

```java
"Hello".charAt(1)
```

returns：

```text
e
```

not `H`。

### 8.4 String `replace` does not mutate original object

```java
String s = "ELEC";
s.replace('E', 'X');
System.out.println(s); // still ELEC
```

Need assign result：

```java
s = s.replace('E', 'X');
```

### 8.5 `NumberFormat` not created with `new`

Use：

```java
NumberFormat.getCurrencyInstance()
```

`DecimalFormat` uses `new`。

---

## 9. 最后 30 秒记忆版

```text
Class library = collection of reusable classes.
Java standard class library is organized into packages.

Use fully qualified name:
java.util.Scanner
or import:
import java.util.Scanner;
import java.util.*;

java.lang is imported automatically:
System, String, Math, Integer.
Scanner/Random/ArrayList are in java.util.
NumberFormat/DecimalFormat are in java.text.

String is immutable.
replace returns new String.
charAt(index), index starts at 0.

Math methods are static:
Math.sqrt, Math.cos, Math.PI.

Random:
new Random()
nextInt(10) gives 0..9.
nextFloat() gives [0, 1).

NumberFormat:
getCurrencyInstance, getPercentInstance.

DecimalFormat:
new DecimalFormat("0.####").
```
