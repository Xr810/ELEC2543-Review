# Lecture 12: Wrapper Classes - Exam Notes

> 范围：`Lecture/12_Wrapper_Class.pdf`，主要是 wrapper classes、object vs primitive、immutability、autoboxing、unboxing、wrapper static methods、`Integer ==` cache。
> 读法：先看第 0 节速查，再重点看第 3 节 boxing/unboxing tracing、第 5 节 `Integer ==`。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Wrapper Class Table 必背

| Primitive Type | Wrapper Class |
|---|---|
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

### Wrapper 必背

| 问题 | 答案 |
|---|---|
| wrapper classes 在哪个 package？ | `java.lang` |
| 为什么需要 wrapper class？ | some methods/classes require objects, not primitive values |
| wrapper object 是否 mutable？ | immutable, like `String` |
| old creation style | `Integer age = new Integer(40);` |
| preferred/value style | `Integer age = Integer.valueOf(40);` |
| autoboxing 是什么？ | primitive automatically converted to wrapper object |
| unboxing 是什么？ | wrapper object automatically converted to primitive value |
| `Integer.parseInt(str)` 做什么？ | convert integer stored in String to `int` |
| useful constants | `Integer.MIN_VALUE`, `Integer.MAX_VALUE` |
| `Integer` objects 能否简单用 `==` 比 value？ | 不应该；`==` compares references |
| `Integer` cache range in lecture | `-128` to `127` |

### In-Class Exercise final values

Code：

```java
Integer intObj1, intObj2;
int i1, i2;
i1 = 1000;
intObj1 = i1;
intObj2 = intObj1;
i2 = intObj2 + 1;
intObj2 = intObj2 + i2;
```

Final values：

| Variable | Value |
|---|---:|
| `i1` | `1000` |
| `i2` | `1001` |
| `intObj1` | `1000` |
| `intObj2` | `2001` |

Conversions：

| Line | Boxing? | Unboxing? | New Integer object? |
|---:|---|---|---|
| 3 `i1 = 1000` | no | no | no |
| 4 `intObj1 = i1` | yes | no | yes |
| 5 `intObj2 = intObj1` | no | no | no new object |
| 6 `i2 = intObj2 + 1` | no | yes | no |
| 7 `intObj2 = intObj2 + i2` | yes | yes | yes |

---

## 1. What Are Wrapper Classes?

The `java.lang` package contains wrapper classes corresponding to each primitive type。

A primitive value is not an object：

```java
int x = 40;
```

A wrapper object wraps a primitive-like value inside an object：

```java
Integer obj = Integer.valueOf(40);
```

### 1.1 Why wrapper classes?

Some situations require objects。

Examples：

- collection classes such as `ArrayList<Integer>`。
- methods that require object parameters。
- utility class methods like `Integer.parseInt(...)`。

Primitive values can be transformed into wrapper objects。

Example from Lecture 8：

```java
ArrayList<Integer> intList = new ArrayList<Integer>();
intList.add(5);
```

`ArrayList` stores object references, so `5` is autoboxed into an `Integer` object。

---

## 2. Creating Wrapper Objects

### 2.1 Constructor style

Lecture shows：

```java
Integer age = new Integer(40);
```

This creates an `Integer` object containing value `40`。

### 2.2 `valueOf` style

Lecture also shows：

```java
Integer age = Integer.valueOf(40);
```

This returns an `Integer` object representing `40`。

Important for cache：

```text
Autoboxing uses Integer.valueOf(), which caches small integers from -128 to 127.
```

### 2.3 Wrapper objects are immutable

Wrapper objects are immutable like `String` objects。

Meaning：

```text
Once an Integer object is created, its stored value cannot be changed.
```

If you write：

```java
Integer x = Integer.valueOf(10);
x = x + 1;
```

What happens：

1. `x` is unboxed to `int 10`。
2. `10 + 1` computes `11`。
3. `11` is boxed into an `Integer` object。
4. `x` now refers to that object。

Original `Integer(10)` object is not modified。

---

## 3. Autoboxing and Unboxing

### 3.1 Autoboxing

Autoboxing is automatic conversion from primitive value to corresponding wrapper object。

Example：

```java
Integer obj;
int num = 42;
obj = num;
```

`num` is primitive `int`。

`obj` is `Integer` reference。

Java automatically boxes：

```java
obj = Integer.valueOf(num);
```

Conceptually：

```text
primitive int 42 -> Integer object containing 42
```

### 3.2 Unboxing

Unboxing is automatic conversion from wrapper object to corresponding primitive value。

Example：

```java
Integer obj = Integer.valueOf(40);
int num = obj;
```

Java automatically unboxes：

```java
int num = obj.intValue();
```

Conceptually：

```text
Integer object containing 40 -> primitive int 40
```

### 3.3 In expressions

Wrapper objects are often unboxed in arithmetic expressions：

```java
Integer x = Integer.valueOf(100);
int y = x + 1;
```

`x + 1` requires primitive arithmetic, so `x` is unboxed。

If assigning result back to wrapper：

```java
Integer z = x + 1;
```

Then：

1. `x` unboxed。
2. arithmetic computed。
3. result boxed into `Integer`。

---

## 4. In-Class Exercise Trace

Code：

```java
1. Integer intObj1, intObj2;
2. int i1, i2;
3. i1 = 1000;
4. intObj1 = i1;
5. intObj2 = intObj1;
6. i2 = intObj2 + 1;
7. intObj2 = intObj2 + i2;
```

### 4.1 Line-by-line

Line 1：

```java
Integer intObj1, intObj2;
```

Creates two reference variables。

No `Integer` objects yet。

Line 2：

```java
int i1, i2;
```

Creates two primitive variables。

Line 3：

```java
i1 = 1000;
```

Primitive assignment。

```text
i1 = 1000
```

No boxing/unboxing。

Line 4：

```java
intObj1 = i1;
```

Left side is `Integer` reference, right side is `int`。

Autoboxing occurs。

Equivalent：

```java
intObj1 = Integer.valueOf(i1);
```

Now：

```text
intObj1 -> Integer(1000)
```

New `Integer` object is created for 1000 because 1000 is outside common cache range。

Line 5：

```java
intObj2 = intObj1;
```

Reference assignment。

No boxing/unboxing。

No new object。

Now：

```text
intObj1 -> Integer(1000)
intObj2 -> same Integer(1000)
```

Line 6：

```java
i2 = intObj2 + 1;
```

Arithmetic requires primitive values。

`intObj2` is unboxed：

```text
1000 + 1 = 1001
```

Then primitive assignment：

```text
i2 = 1001
```

Unboxing occurs。

No new `Integer` object needed for `i2` because `i2` is primitive。

Line 7：

```java
intObj2 = intObj2 + i2;
```

Right side arithmetic：

1. `intObj2` unboxed from `Integer(1000)` to `int 1000`。
2. `i2` is primitive `1001`。
3. result is `2001`。

Left side is `Integer` reference, so result is boxed：

```text
intObj2 -> Integer(2001)
```

New object is created for `2001`。

`intObj1` still refers to old `Integer(1000)`。

### 4.2 Final state

```text
i1      = 1000
i2      = 1001
intObj1 = 1000
intObj2 = 2001
```

Important reference picture：

```text
intObj1 -> Integer(1000)
intObj2 -> Integer(2001)
```

After line 7 they no longer refer to the same object。

---

## 5. Wrapper Class Static Methods and Constants

Wrapper classes contain static methods that help manage associated primitive types。

### 5.1 `parseInt`

```java
String str = "123";
int num = Integer.parseInt(str);
```

Result：

```text
num = 123
```

Important：

```text
Integer.parseInt returns primitive int, not Integer object.
```

### 5.2 Constants

Wrapper classes often contain useful constants。

Examples：

```java
Integer.MIN_VALUE
Integer.MAX_VALUE
```

These hold the smallest and largest `int` values。

---

## 6. More About `==` with `Integer`

### 6.1 `==` compares references for objects

For object references：

```java
obj1 == obj2
```

checks whether they refer to the same object。

So for `Integer` objects, do not rely on `==` to compare numeric values。

Use：

```java
intObj1.equals(intObj2)
```

or unbox to primitive carefully。

### 6.2 Cache exception

Lecture note：

```text
When Integer is between -128 and 127, references will be the same as autoboxing uses Integer.valueOf() which caches small integers.
```

Example：

```java
Integer a = 100;
Integer b = 100;
System.out.println(a == b); // often true because cached
```

But：

```java
Integer x = 1000;
Integer y = 1000;
System.out.println(x == y); // false in normal autoboxing behavior
```

because 1000 is outside `-128` to `127` cache range。

Important exam conclusion：

```text
Use equals to compare Integer values. Do not use == unless the question specifically asks about references/cache behavior.
```

### 6.3 `new Integer(...)` always creates a new object

Conceptually：

```java
Integer a = new Integer(100);
Integer b = new Integer(100);
System.out.println(a == b); // false
```

Even though values are same, references are different。

---

## 7. Wrapper Classes and `ArrayList`

`ArrayList` stores object references。

This is why we write：

```java
ArrayList<Integer> intList = new ArrayList<Integer>();
```

not:

```java
ArrayList<int> intList; // wrong
```

Adding：

```java
intList.add(5);
```

Autoboxing converts `5` to `Integer.valueOf(5)`。

Retrieving：

```java
int x = intList.get(0);
```

Unboxing converts `Integer` back to `int`。

---

## 8. 考试答题模板

### 8.1 问：why use wrapper classes?

```text
Wrapper classes allow primitive values to be represented as objects, which is necessary in contexts where objects are required, such as collections or methods expecting object parameters.
```

### 8.2 问：what is autoboxing?

```text
Autoboxing is the automatic conversion of a primitive value to its corresponding wrapper object, such as int to Integer.
```

### 8.3 问：what is unboxing?

```text
Unboxing is the automatic conversion of a wrapper object to its corresponding primitive value, such as Integer to int.
```

### 8.4 问：why is `Integer == Integer` dangerous?

```text
Because Integer is an object type and == compares object references, not numeric values. Small cached values from -128 to 127 may make == appear to work, but outside that range separate Integer objects may not be the same reference.
```

---

## 9. 最容易错的点

1. `Integer` is wrapper class；`int` is primitive。
2. Wrapper classes are in `java.lang`。
3. Wrapper objects are immutable。
4. Autoboxing：primitive to wrapper。
5. Unboxing：wrapper to primitive。
6. `Integer.parseInt("123")` returns primitive `int`。
7. `Integer.MIN_VALUE` and `Integer.MAX_VALUE` are useful constants。
8. `ArrayList<int>` is wrong；use `ArrayList<Integer>`。
9. `Integer == Integer` compares references。
10. `-128` to `127` cache can make `==` tricky。
11. In the exercise，line 4 boxes，line 6 unboxes，line 7 unboxes and boxes。

---

## 10. 最后 30 秒记忆版

```text
Primitive -> Wrapper:
int -> Integer
char -> Character
boolean -> Boolean

Autoboxing:
Integer obj = 42;

Unboxing:
int x = obj;

Wrapper immutable like String.

Integer exercise:
i1=1000
i2=1001
intObj1=1000
intObj2=2001

Integer == compares references.
Autoboxing uses valueOf(); cache -128 to 127.
Use equals for values.
```
