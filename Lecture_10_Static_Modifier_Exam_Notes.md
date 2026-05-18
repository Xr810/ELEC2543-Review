# Lecture 10: Static Modifier - Exam Notes

> 范围：`Lecture/10_Static_Modifier.pdf`，主要是 `static` modifier、static/class methods、static variables/class variables、`MyMath`、`Slogan` counter。
> 读法：先看第 0 节速查，再重点看第 2 节 static method 限制、第 3 节 static variable shared copy、第 4 节 Slogan trace。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Static 必背

| 问题 | 答案 |
|---|---|
| `static` 可以修饰什么？ | variables 和 methods |
| `static` 把 member 关联到谁？ | class itself，而不是某个 object |
| static method 还叫什么？ | class method |
| static variable 还叫什么？ | class variable |
| static method 如何调用？ | usually using class name, e.g. `Math.cos(...)` |
| `main` 为什么是 static？ | Java interpreter can invoke it without creating object |
| static method 能否 directly reference instance variables？ | 不能 |
| 为什么？ | instance variables do not exist until an object exists |
| static method 可以 reference 什么？ | static variables、local variables、other static methods |
| static variable 有几份 copy？ | one copy shared by all objects |
| static variable 什么时候创建 memory？ | when class is first referenced |
| 一个 object 改 static variable，其他 objects 看得到吗？ | yes，因为 shared one copy |

### `Slogan` 例子必背

```java
private static int count = 0;

public Slogan(String str) {
    phrase = str;
    count++;
}

public static int getCount() {
    return count;
}
```

创建 5 个 `Slogan` objects 后：

```text
Slogans created: 5
```

---

## 1. What Does `static` Mean?

`static` is a modifier。

It can be used to declare a property of：

- variable
- method

Core idea：

```text
static associates the method or variable with the class rather than with an object of that class.
```

Without `static`：

```java
private String phrase;
```

Each object has its own `phrase`。

With `static`：

```java
private static int count;
```

The class has one shared `count`。

### 1.1 Modifier order

Modifiers can technically be interchanged：

```java
public static int getCount()
static public int getCount()
```

But convention：

```text
visibility modifier first
```

So write：

```java
public static
private static
```

---

## 2. Static/Class Methods

### 2.1 Calling static methods

Static methods are invoked using class names。

Examples：

```java
Math.sqrt(16)
Math.cos(90)
Integer.parseInt("123")
Slogan.getCount()
```

You do not need：

```java
Math m = new Math(); // not how Math is used
```

### 2.2 `main` is static

```java
public static void main(String[] args)
```

`main` is static because the Java interpreter invokes it without first creating an object of the class。

### 2.3 Static methods cannot directly use instance variables

Example wrong pattern：

```java
public class Example {
    private int value;

    public static void printValue() {
        System.out.println(value); // wrong
    }
}
```

Reason：

```text
value belongs to an object. A static method belongs to the class and may run when no object exists.
```

Correct options：

1. Use an object：

```java
public static void printValue(Example obj) {
    System.out.println(obj.value);
}
```

2. Make the variable static if it is truly class-level：

```java
private static int value;

public static void printValue() {
    System.out.println(value);
}
```

### 2.4 Static method can use local variables

```java
public static boolean isEven(int n) {
    return (n % 2) == 0;
}
```

`n` is local/formal parameter，so static method can use it。

### 2.5 Static method can call another static method

Lecture `MyMath` example：

```java
public class MyMath {
    public static boolean isEven(int n) {
        return (n % 2) == 0;
    }

    public static boolean isPrime(int n) {
        if (n <= 1) return false;
        if (n == 2) return true;
        if (isEven(n)) return false;

        for (int i = 3; i <= Math.sqrt(n) + 1; i++) {
            if (n % i == 0) {
                return false;
            }
        }

        return true;
    }
}
```

Important line：

```java
if (isEven(n)) return false;
```

`isPrime` can call `isEven` directly because both are static methods in same class。

Could also write：

```java
if (MyMath.isEven(n)) return false;
```

---

## 3. Static Variables / Class Variables

### 3.1 Normal instance variables

Normally each object has its own data space。

```java
public class Student {
    private String name;
}
```

If you create three students：

```text
s1.name
s2.name
s3.name
```

Each object has its own `name`。

### 3.2 Static variable

If variable is declared static：

```java
private static float price;
```

Only one copy exists。

All objects instantiated from the class share that static variable。

### 3.3 Changing static variable

If one object changes static variable：

```text
all other objects see the changed value
```

Because there is only one class-level copy。

### 3.4 Memory timing

Lecture point：

```text
Memory space for a static variable is created when the class is first referenced.
```

This is different from instance variable，which exists as part of object after object creation。

---

## 4. `Slogan` Counter Example

### 4.1 `Slogan` class

```java
public class Slogan {
    private String phrase;
    private static int count = 0;

    public Slogan(String str) {
        phrase = str;
        count++;
    }

    public String toString() {
        return phrase;
    }

    public static int getCount() {
        return count;
    }
}
```

Fields：

| Field | Type | Meaning |
|---|---|---|
| `phrase` | instance variable | each Slogan object has its own phrase |
| `count` | static variable | one shared count for whole class |

### 4.2 Constructor effect

Every time this runs：

```java
new Slogan("...");
```

constructor executes：

```java
count++;
```

So `count` tracks how many `Slogan` objects have been created。

### 4.3 Driver trace

```java
Slogan obj;

obj = new Slogan("Remember the Alamo.");
System.out.println(obj);

obj = new Slogan("Don't Worry. Be Happy.");
System.out.println(obj);

obj = new Slogan("Live Free or Die.");
System.out.println(obj);

obj = new Slogan("Talk is Cheap.");
System.out.println(obj);

obj = new Slogan("Write Once, Run Anywhere.");
System.out.println(obj);

System.out.println("Slogans created: " + Slogan.getCount());
```

Trace table：

| Step | Action | `count` |
|---:|---|---:|
| initial | class first referenced | `0` |
| 1 | create first Slogan | `1` |
| 2 | create second Slogan | `2` |
| 3 | create third Slogan | `3` |
| 4 | create fourth Slogan | `4` |
| 5 | create fifth Slogan | `5` |

Output：

```text
Remember the Alamo.
Don't Worry. Be Happy.
Live Free or Die.
Talk is Cheap.
Write Once, Run Anywhere.

Slogans created: 5
```

### 4.4 Why call `Slogan.getCount()`?

Because `getCount` is static：

```java
public static int getCount()
```

Preferred call：

```java
Slogan.getCount()
```

This shows clearly that the method belongs to the class。

---

## 5. Static vs Instance

| Feature | Instance member | Static member |
|---|---|---|
| Belongs to | object | class |
| Number of copies | one per object | one shared copy |
| Access style | `obj.member` | `ClassName.member` |
| Exists when | object is created | class is first referenced |
| Can directly access instance data? | instance methods can | static methods cannot |
| Example | `phrase` | `count` |

### 5.1 Memory picture

For:

```java
Slogan a = new Slogan("A");
Slogan b = new Slogan("B");
```

Conceptually：

```text
Slogan class:
    static count = 2

a object:
    phrase = "A"

b object:
    phrase = "B"
```

There are two `phrase` variables, but one shared `count`。

---

## 6. Common Exam Traps

### 6.1 Static method using instance variable

Question：

```java
public class Test {
    private int x = 3;

    public static void show() {
        System.out.println(x);
    }
}
```

Answer：

```text
Compilation error. A static method cannot directly reference an instance variable.
```

### 6.2 Instance method using static variable

```java
public class Test {
    private static int count = 0;

    public void add() {
        count++;
    }
}
```

This is allowed。

Instance methods can access static variables。

### 6.3 Static method using static variable

```java
public class Test {
    private static int count = 0;

    public static int getCount() {
        return count;
    }
}
```

This is allowed。

---

## 7. 考试答题模板

### 7.1 问：what does `static` mean?

```text
static associates a variable or method with the class rather than with individual objects of the class.
```

### 7.2 问：why cannot static methods directly reference instance variables?

```text
Instance variables belong to objects and do not exist until an object is created. A static method belongs to the class and can be invoked without creating an object, so it cannot directly refer to instance variables.
```

### 7.3 问：what is a static variable?

```text
A static variable, also called a class variable, has only one shared copy for the entire class. All objects of the class share that variable.
```

---

## 8. 最容易错的点

1. `static` belongs to class, not object。
2. static method 不能直接访问 instance variables。
3. static method 可以访问 static variables、local variables、static methods。
4. `main` is static because no object is created before program starts。
5. static variable 只有一份 shared copy。
6. `count++` in constructor is a common way to count created objects。
7. Call static method with class name：`Slogan.getCount()`。
8. `Math` methods are static。

---

## 9. 最后 30 秒记忆版

```text
static = class-level

static method:
- call by ClassName.method()
- cannot directly use instance variables
- can use static variables, local variables, static methods

static variable:
- one shared copy
- all objects see same value

Slogan:
private static int count = 0;
constructor -> count++;
5 objects -> getCount() returns 5
```
