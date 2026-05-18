# Lecture 11: Variables and Object Comparison - Exam Notes

> 范围：`Lecture/11_Object_Comparison.pdf`，主要是 float comparison、char comparison、String `equals` / `compareTo`、lexicographic ordering、object `==`、overriding `equals`。
> 读法：先看第 0 节速查，再重点看第 3 节 String comparison、第 4 节 object comparison、第 5 节 `equals` override。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Comparison 必背

| 比较对象 | 正确方法 | 重点 |
|---|---|---|
| floats/doubles | tolerance | avoid exact equality |
| chars | relational operators | based on Unicode values |
| Strings exact equality | `s1.equals(s2)` | same characters same order |
| Strings ordering | `s1.compareTo(s2)` | lexicographic ordering |
| object aliases | `obj1 == obj2` | true only if same object reference |
| object content equality | override `equals` | default `equals` behaves like `==` |

### `compareTo` return value

For:

```java
name1.compareTo(name2)
```

| Return | Meaning |
|---:|---|
| `< 0` | `name1` comes before `name2` |
| `0` | same characters |
| `> 0` | `name1` comes after `name2` |

### Lexicographic ordering 必背

| Rule | Example |
|---|---|
| uppercase letters come before lowercase letters | `"Great"` comes before `"fantastic"` |
| shorter prefix comes before longer string | `"book"` comes before `"bookcase"` |
| based on character set ordering | Unicode |

---

## 1. Comparing Floating-Point Values

Floating-point values are often not compared using exact equality。

Instead, use a tolerance：

```java
if (Math.abs(f1 - f2) < TOLERANCE) {
    System.out.println("Essentially equal");
}
```

Example：

```java
final double TOLERANCE = 0.000001;

if (Math.abs(d1 - d2) < TOLERANCE) {
    System.out.println("Essentially equal");
}
```

Meaning：

```text
If the difference between two floating point values is less than tolerance, they are considered equal.
```

Why？

Floating-point calculations can have tiny rounding errors。

So this may be risky：

```java
if (d1 == d2)
```

Better：

```java
if (Math.abs(d1 - d2) < TOLERANCE)
```

---

## 2. Comparing Characters

Java character data is based on the Unicode character set。

Unicode assigns numeric values to characters。

Therefore, Java can use relational operators on `char`：

```java
'A' < 'B'  // true
'a' > 'Z'  // true
```

This is why Lecture 8 letter counting works：

```java
current >= 'A' && current <= 'Z'
```

and:

```java
upper[current - 'A']++;
```

Exam note：

```text
Character comparison is numeric comparison of Unicode values.
```

---

## 3. Comparing Strings

### 3.1 Exact String equality: `equals`

Use:

```java
if (name1.equals(name2)) {
    System.out.println("Same name");
}
```

`equals` returns a boolean。

It checks whether two strings contain exactly the same characters in the same order。

### 3.2 Do not use relational operators for String ordering

This is wrong：

```java
if (name1 < name2) // wrong
```

Java does not use `<`, `>`, `<=`, `>=` to compare `String` objects。

### 3.3 Use `compareTo` for order

```java
int result = name1.compareTo(name2);
```

Rules：

```text
result < 0: name1 is less than name2
result == 0: name1 and name2 are equal
result > 0: name1 is greater than name2
```

Example：

```java
if (name1.compareTo(name2) < 0) {
    System.out.println(name1 + " comes first");
} else if (name1.compareTo(name2) == 0) {
    System.out.println("Same name");
} else {
    System.out.println(name2 + " comes first");
}
```

### 3.4 Lexicographic ordering

String ordering is based on character set ordering。

This is called：

```text
lexicographic ordering
```

Important rules from lecture：

1. Uppercase letters come before lowercase letters in Unicode。
2. Short strings come before longer strings with same prefix。

Examples：

```text
"Great" comes before "fantastic"
```

because uppercase `G` comes before lowercase `f` in Unicode。

```text
"book" comes before "bookcase"
```

because `"book"` is shorter and is a prefix of `"bookcase"`。

### 3.5 Common trace examples

```java
"apple".compareTo("banana") < 0
```

True because `a` comes before `b`。

```java
"Zoo".compareTo("apple") < 0
```

True because uppercase `Z` comes before lowercase `a` in Unicode。

```java
"book".compareTo("bookcase") < 0
```

True because shorter prefix comes first。

---

## 4. Comparing Objects

### 4.1 `==` with objects

The `==` operator can be applied to objects。

But it checks whether two references are aliases of each other。

Meaning：

```text
obj1 == obj2 is true only if obj1 and obj2 refer to the exact same object.
```

Example：

```java
Die d1 = new Die();
Die d2 = new Die();

System.out.println(d1 == d2); // false
```

Even if both dice have same face value，they are different objects。

Alias example：

```java
Die d3 = d1;
System.out.println(d1 == d3); // true
```

because `d1` and `d3` refer to same object。

### 4.2 Default `equals`

The `equals` method is defined by default to have same semantics as `==`。

Default `equals` does not compare instance variables。

So unless a class overrides `equals`：

```java
obj1.equals(obj2)
```

behaves like：

```java
obj1 == obj2
```

### 4.3 Overriding `equals`

When writing a class, you can redefine `equals` to return true under appropriate conditions。

`String` class does this：

```java
"abc".equals(new String("abc")) // true
```

because `String.equals` compares characters。

---

## 5. `DieWithEquals` Example

Lecture class：

```java
public class DieWithEquals {
    private final int MAX = 6;
    private int faceValue;

    public DieWithEquals() {
        faceValue = 1;
    }

    public boolean equals(DieWithEquals d) {
        return d.faceValue == faceValue;
    }
}
```

This `equals` says：

```text
Two DieWithEquals objects are equal if their faceValue values are equal.
```

Driver：

```java
DieWithEquals die1, die2;
die1 = new DieWithEquals();
die2 = new DieWithEquals();

if (die1 == die2) {
    System.out.println("die1 == die2");
} else {
    System.out.println("die1 =/= die2");
}

if (die1.equals(die2)) {
    System.out.println("die1 is equal to die2");
} else {
    System.out.println("die1 is not equal to die2");
}
```

Trace：

- `die1` and `die2` are two different objects。
- so `die1 == die2` is false。
- both constructors set `faceValue = 1`。
- `die1.equals(die2)` compares `faceValue` and returns true。

Output：

```text
die1 =/= die2
die1 is equal to die2
```

---

## 6. Student `equals` Exercise

Question：

```java
public class Student {
    private String lastname;
    private String firstname;
    private int age;
}
```

Define `equals` so that `s1.equals(s2)` returns true if both students have same `lastname`、`firstname`、`age`。

Lecture answer：

```java
public class Student {
    private String lastname;
    private String firstname;
    private int age;

    public boolean equals(Student s) {
        if ((lastname.equals(s.lastname)) &&
            (firstname.equals(s.firstname)) &&
            (s.age == age)) {
            return true;
        } else {
            return false;
        }
    }
}
```

Shorter equivalent：

```java
public boolean equals(Student s) {
    return lastname.equals(s.lastname)
        && firstname.equals(s.firstname)
        && age == s.age;
}
```

Important：

- Use `equals` for `String` fields。
- Use `==` for primitive `int age`。

Potential null issue not covered by lecture：

```text
If lastname or firstname could be null, the above code may throw NullPointerException. Lecture assumes non-null strings.
```

---

## 7. Choosing the Right Comparison

### 7.1 Cheat table

| Code | Meaning |
|---|---|
| `a == b` for primitives | compare primitive values |
| `a == b` for objects | compare references / aliases |
| `s1.equals(s2)` | compare String contents |
| `s1.compareTo(s2)` | compare String order |
| `Math.abs(f1 - f2) < TOLERANCE` | compare floating-point values approximately |
| `obj1.equals(obj2)` | depends on class's equals definition |

### 7.2 Example decision

If asked whether two `Student` objects represent same student：

```java
s1 == s2
```

checks whether same object。

```java
s1.equals(s2)
```

can check same lastname, firstname, and age if `equals` is overridden。

---

## 8. 考试答题模板

### 8.1 问：why use tolerance for float comparison?

```text
Floating-point values may contain small rounding errors, so exact equality may fail even when the values should be considered equal. A tolerance checks whether the absolute difference is small enough.
```

### 8.2 问：what does String `compareTo` return?

```text
compareTo returns zero if two strings are equal, a negative value if the receiver comes before the argument, and a positive value if the receiver comes after the argument in lexicographic ordering.
```

### 8.3 问：what does `==` mean for objects?

```text
For objects, == returns true only if the two references are aliases of the same object. It does not compare instance variables.
```

### 8.4 问：why override `equals`?

```text
The default equals method has the same semantics as ==. To compare object contents or instance variables, a class must redefine equals with the desired equality condition.
```

---

## 9. 最容易错的点

1. Floats should usually be compared with tolerance。
2. `String` exact equality uses `.equals()`。
3. `String` ordering uses `.compareTo()`，not `<` or `>`。
4. `compareTo` negative means receiver comes first。
5. Uppercase letters come before lowercase letters in Unicode。
6. `"book"` comes before `"bookcase"`。
7. Object `==` compares references, not contents。
8. Default object `equals` behaves like `==` unless overridden。
9. In custom `equals`, compare `String` fields with `.equals()` and primitive fields with `==`。

---

## 10. 最后 30 秒记忆版

```text
float:
Math.abs(f1 - f2) < TOLERANCE

char:
Unicode order, relational operators OK

String:
s1.equals(s2)      -> same characters
s1.compareTo(s2)   -> order
<0 receiver first, 0 same, >0 receiver after

Object:
== checks same reference
default equals same as ==
override equals to compare instance variables
```
