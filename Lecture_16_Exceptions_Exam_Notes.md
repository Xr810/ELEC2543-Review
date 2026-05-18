# Lecture 16: Exceptions - Exam Notes

> 范围：`Lecture/16_Exception.pdf`，主要是 exception handling、`try-catch-finally`、exception propagation、checked/unchecked exceptions、custom exception、I/O exceptions。
> 读法：先看第 0 节速查，再重点看第 2 节 `try-catch` 流程、第 3 节 propagation、第 4 节 checked vs unchecked。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Exceptions 必背

| 问题 | 答案 |
|---|---|
| `exception` 是什么？ | 描述 unusual or erroneous situation 的 object |
| exception 由谁产生？ | program throws it |
| exception 可以由谁处理？ | same place catch，或 caller/higher level catch |
| normal execution flow vs exception flow | 正常代码一条线；exception 发生后跳到 matching handler |
| `Error` 应不应该 catch？ | 通常不应该，代表 unrecoverable situation |
| 不 catch exception 会怎样？ | program terminates and prints exception message + call stack trace |
| `try` 后面可以接几个 `catch`？ | one or more |
| `catch` 是什么？ | exception handler |
| exception 发生后 try block 剩余代码会继续吗？ | 不会，直接跳到 matching catch |
| `finally` 什么时候执行？ | 无论 exception 是否发生、是否 handled，通常都会执行 |
| exception propagation 是什么？ | exception 没在当前 method 处理，就沿 method call hierarchy 往上抛 |
| 所有 exception/error 的 root class | `Throwable` |
| custom checked exception 通常 extends 什么？ | `Exception` |
| `throw` 用来做什么？ | 实际抛出一个 exception object |
| `throws` 用来做什么？ | 在 method header 声明 method 可能 throw/propagate checked exception |
| `IOException` 是 checked 还是 unchecked？ | checked |

### Checked vs Unchecked

| 类型 | 是否必须处理或声明 | 例子 |
|---|---|---|
| checked exception | 必须 catch 或 `throws` | `ClassNotFoundException`, `NoSuchMethodException`, `IOException`, custom `extends Exception` |
| unchecked exception | 不要求显式处理 | `NullPointerException`, `IndexOutOfBoundsException`, `ArithmeticException` |
| error | 不要求，也通常不 catch | serious unrecoverable problems |

---

## 1. Exception Handling 基本概念

### 1.1 Exception 是 object

Java 中 exception 不是单纯的 error message，而是 object。

它描述：

```text
an unusual or erroneous situation
```

例子：

```java
int result = 10 / 0;
```

会产生：

```text
ArithmeticException: / by zero
```

### 1.2 Exception handling 的三种选择

当 exception 可能发生时，program 可以：

1. ignore it：不处理。
2. handle it where it occurs：在发生处 catch。
3. handle it at another place：让它 propagate 到 higher-level method 处理。

设计重点：

```text
Where to handle an exception is a design decision.
```

不是所有 exception 都应该在最底层 method 处理。有时 caller 更知道该怎么恢复。

### 1.3 Uncaught exception 的结果

如果 exception 没有被 catch：

```text
program terminates
```

并打印：

- exception type。
- exception message。
- call stack trace。

例子：

```java
public class Zero {
    public static void main(String[] args) {
        int numerator = 10;
        int denominator = 0;
        System.out.println(numerator / denominator);
        System.out.println("This text will not be printed.");
    }
}
```

输出重点：

```text
Exception in thread "main" java.lang.ArithmeticException: / by zero
at Zero.main(Zero.java:...)
```

第二个 `println` 不会执行。

### 1.4 Call stack trace 怎么读？

Stack trace 表示 method call trail。

如果看到：

```text
at ExceptionScope.level3(ExceptionScope.java:54)
at ExceptionScope.level2(ExceptionScope.java:41)
at ExceptionScope.level1(ExceptionScope.java:18)
at Propagation.main(Propagation.java:17)
```

读法：

1. exception 在 `level3` 第 54 行发生。
2. `level3` 是被 `level2` 调用的。
3. `level2` 是被 `level1` 调用的。
4. `level1` 是被 `main` 调用的。

最上面通常是 exception actual location。

---

## 2. The `try-catch` Statement

### 2.1 基本结构

```java
try {
    // code that may throw exceptions
} catch (ExceptionType name) {
    // handler
}
```

可以有多个 `catch`：

```java
try {
    // risky code
} catch (StringIndexOutOfBoundsException ex) {
    // handle invalid string index
} catch (NumberFormatException ex) {
    // handle parse error
}
```

### 2.2 执行流程

如果 try block 中没有 exception：

1. 顺序执行 try block。
2. 跳过所有 catch。
3. 继续 try-catch 后面的代码。

如果 try block 中有 exception：

1. exception 发生那一行立刻停止正常执行。
2. try block 中剩下的 statements 不执行。
3. Java 找第一个 matching catch。
4. 执行该 catch。
5. catch 后继续 try-catch statement 后面的代码。

如果没有 matching catch：

```text
exception propagates to the caller.
```

### 2.3 `ProductCodes` 例子

核心任务：

- 输入 product code。
- 从 `code.charAt(9)` 读 zone。
- 从 `code.substring(3, 7)` parse district。
- 如果 zone 是 `R` 且 district > 2000，则 banned++。

可能错误：

| 代码 | 可能 exception | 原因 |
|---|---|---|
| `code.charAt(9)` | `StringIndexOutOfBoundsException` | string 太短 |
| `Integer.parseInt(...)` | `NumberFormatException` | substring 不是 numeric |

简化版：

```java
while (!code.equals("XXX")) {
    try {
        zone = code.charAt(9);
        district = Integer.parseInt(code.substring(3, 7));
        valid++;

        if (zone == 'R' && district > 2000) {
            banned++;
        }
    } catch (StringIndexOutOfBoundsException exception) {
        System.out.println("Improper code length: " + code);
    } catch (NumberFormatException exception) {
        System.out.println("District is not numeric: " + code);
    }

    code = scan.nextLine();
}
```

注意 `valid++` 的位置：

```text
valid++ only happens after charAt and parseInt succeed.
```

所以 invalid length / non-numeric district 不计入 valid。

### 2.4 Multiple catch 的顺序

原则：

```text
Put more specific exception types before more general exception types.
```

错误例子：

```java
try {
    // ...
} catch (Exception ex) {
    // catches everything
} catch (NumberFormatException ex) {
    // unreachable
}
```

如果先 catch `Exception`，后面的 child exception handler 永远到不了。

---

## 3. The `finally` Clause

### 3.1 `finally` 是什么？

`finally` 是 optional block：

```java
try {
    // risky code
} catch (Exception ex) {
    // handler
} finally {
    // cleanup code
}
```

作用：

```text
Execute important cleanup code no matter what.
```

常见用途：

- close file。
- close network connection。
- release resource。
- cleanup temporary state。

### 3.2 什么时候执行？

| 情况 | `finally` 执行吗？ |
|---|---|
| try block 没有 exception | yes |
| exception 被 catch | yes |
| exception 没有 matching catch，继续 propagate | yes，通常先执行 finally 再 propagate |

课程重点：

```text
Java finally block is always executed whether exception is handled or not.
```

实际 Java 中极端情况如 JVM crash / `System.exit` 不讨论，考试按 lecture 规则记。

---

## 4. Exception Propagation

### 4.1 Propagation 是什么？

如果 exception 在当前 method 没有被 handle，它会往 caller 传：

```text
level3 throws -> level2 -> level1 -> main
```

直到：

- 被某个 higher-level method catch。
- 或一路到 `main` 仍未 catch，program terminates。

### 4.2 Propagation example

结构：

```java
main() calls level1()
level1() calls level2()
level2() calls level3()
level3() divides by zero
```

`level3()`：

```java
public void level3() {
    int numerator = 10, denominator = 0;
    int result = numerator / denominator;
}
```

`level3` 没有 catch，所以 exception 传给 `level2`。

`level2` 也没有 catch，所以传给 `level1`。

`level1` 有：

```java
try {
    level2();
} catch (ArithmeticException problem) {
    System.out.println(problem.getMessage());
    problem.printStackTrace();
}
```

因此 `level1` handles it。

### 4.3 Propagation 后哪些代码不会执行？

如果 `level3` 中 exception 出现在：

```java
int result = numerator / denominator;
System.out.println("Level 3 ending.");
```

那么：

```text
"Level 3 ending." will not print.
```

如果 `level2` 调用：

```java
level3();
System.out.println("Level 2 ending.");
```

因为 exception 从 `level3` 直接 propagates out of `level2`：

```text
"Level 2 ending." will not print.
```

但 `level1` catch 后，catch 后面的：

```java
System.out.println("Level 1 ending.");
```

会继续执行。

---

## 5. Exception Class Hierarchy

### 5.1 `Throwable` root

Java exception/error hierarchy 的 root 是：

```java
Throwable
```

下面主要分：

```text
Throwable
  -> Error
  -> Exception
       -> RuntimeException
```

### 5.2 `Error`

`Error` 表示严重、通常 unrecoverable 的问题。

课件重点：

```text
Errors should not be caught.
Errors do not require a throws clause.
```

### 5.3 `Exception`

大多数 application-level recoverable problems 属于 `Exception`。

如果自定义 checked exception，通常：

```java
public class OutOfRangeException extends Exception {
    public OutOfRangeException(String message) {
        super(message);
    }
}
```

### 5.4 Checked exceptions

Checked exception 的规则：

```text
A checked exception must either be caught or listed in the throws clause.
```

如果 method 可能 throw checked exception：

```java
public static void main(String[] args) throws IOException {
    // file operations
}
```

或者：

```java
try {
    // file operations
} catch (IOException ex) {
    // handle it
}
```

如果两者都没有，compiler error。

### 5.5 Unchecked exceptions

Unchecked exceptions 不要求 explicit handling。

课件重点：

```text
The only unchecked exceptions in Java are RuntimeException or its descendants.
```

例子：

| Exception | Checked? |
|---|---|
| `NullPointerException` | unchecked |
| `IndexOutOfBoundsException` | unchecked |
| `ArithmeticException` | unchecked |
| `ClassNotFoundException` | checked |
| `NoSuchMethodException` | checked |

---

## 6. The `throw` Statement

### 6.1 `throw` vs `throws`

| Keyword | 位置 | 作用 |
|---|---|---|
| `throw` | method body inside statements | actually throws an exception object |
| `throws` | method header | declares method may throw/propagate exception |

例子：

```java
public static void main(String[] args) throws OutOfRangeException {
    if (value < MIN || value > MAX) {
        throw new OutOfRangeException("Input value is out of range.");
    }
}
```

### 6.2 Custom exception

```java
public class OutOfRangeException extends Exception {
    public OutOfRangeException(String message) {
        super(message);
    }
}
```

这里 `super(message)` 调用 `Exception` 的 constructor，把 message 存进去。

之后可以：

```java
throw new OutOfRangeException("Too High");
```

### 6.3 `throw` 后面的 unreachable code

Quick Check：

```java
System.out.println("Before throw");
throw new OutOfRangeException("Too High");
System.out.println("After throw");
```

问题：

```text
The throw is not conditional and always occurs.
The second println statement can never be reached.
```

所以 compile-time 可能报 unreachable statement。

正确设计通常把 `throw` 放在 condition 里：

```java
if (value > MAX) {
    throw new OutOfRangeException("Too High");
}
```

---

## 7. I/O Exceptions

### 7.1 Stream

`stream` 是：

```text
a sequence of bytes that flow from a source to a destination
```

Program 可以：

- read from input stream。
- write to output stream。

Java standard I/O streams：

| Stream | Meaning |
|---|---|
| `System.out` | standard output |
| `System.in` | standard input |
| `System.err` | standard error |

### 7.2 `IOException`

I/O operations 可能 throw `IOException`，例如：

- file might not exist。
- program may not find the file。
- file content not in expected format。
- output file cannot be created/written。

重点：

```text
IOException is a checked exception.
```

所以必须：

```java
throws IOException
```

或：

```java
try-catch IOException
```

### 7.3 Writing text file with `PrintWriter`

课程示例用：

```java
import java.io.*;
import java.util.Random;

public static void main(String[] args) throws IOException {
    String fileName = "test.txt";
    PrintWriter outFile = new PrintWriter(fileName);

    outFile.println("some data");

    outFile.close();
}
```

重点：

```text
Output streams should be closed explicitly.
```

如果忘记 close：

- buffer 中的数据可能没有完全写出。
- file resource 没释放。

---

## 8. 考试答题模板

### 8.1 写 `try-catch`

```java
try {
    // risky statement 1
    // risky statement 2
} catch (SpecificException ex) {
    // handle specific case
} catch (AnotherException ex) {
    // handle another case
}
```

说明：

```text
When an exception occurs in the try block,
control immediately jumps to the first matching catch clause.
```

### 8.2 写 `finally`

```java
try {
    // open/use resource
} catch (IOException ex) {
    // handle I/O problem
} finally {
    // close resource
}
```

说明：

```text
finally is used for cleanup and executes whether or not an exception is handled.
```

### 8.3 写 custom checked exception

```java
public class MyException extends Exception {
    public MyException(String message) {
        super(message);
    }
}
```

使用：

```java
public void check(int x) throws MyException {
    if (x < 0) {
        throw new MyException("x cannot be negative");
    }
}
```

### 8.4 解释 propagation

```text
If a method does not catch an exception, the exception propagates
to the method that called it. This continues up the call hierarchy
until the exception is caught or reaches main.
```

### 8.5 判断 checked / unchecked

```text
RuntimeException and descendants are unchecked.
IOException and custom exceptions extending Exception are checked.
Checked exceptions must be caught or declared with throws.
```

---

## 9. 最容易错的点

### 9.1 `throw` 和 `throws` 混淆

错误：

```java
throws new Exception();
```

正确：

```java
throw new Exception();
```

Method header 用：

```java
public void f() throws Exception
```

### 9.2 Exception 发生后 try 剩余代码不执行

```java
try {
    int x = 10 / 0;
    System.out.println("after"); // not executed
} catch (ArithmeticException ex) {
    System.out.println("caught");
}
```

输出：

```text
caught
```

### 9.3 Checked exception 不处理会 compile error

```java
PrintWriter out = new PrintWriter("test.txt");
```

可能 throw checked exception。

需要：

```java
throws IOException
```

或 `try-catch`。

### 9.4 Catch order 从 specific 到 general

错误：

```java
catch (Exception ex) { }
catch (IOException ex) { }
```

正确：

```java
catch (IOException ex) { }
catch (Exception ex) { }
```

### 9.5 `finally` 不是 catch

`finally` 不负责处理 exception。

它只是 cleanup。

如果 exception 没有被 catch，`finally` 执行完后 exception 仍然会继续 propagate。

---

## 10. 最后 30 秒记忆版

```text
Exception = object describing unusual/error situation.
Thrown by program, caught and handled by another part.

Uncaught exception terminates program and prints stack trace.
Error usually means unrecoverable; do not catch normally.

try block contains risky code.
catch handles matching exception type.
Exception jumps immediately to first matching catch.
finally runs for cleanup whether exception is handled or not.

Propagation = uncaught exception moves up caller chain.

Throwable is root.
Exception and Error are under Throwable.
RuntimeException descendants are unchecked.
Checked exceptions must be caught or declared with throws.

throw = actually throw an exception object.
throws = method header declaration.

IOException is checked.
Output streams should be closed explicitly.
```
