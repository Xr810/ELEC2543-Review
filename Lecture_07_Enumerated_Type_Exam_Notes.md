# Lecture 7: Enumerated Type - Exam Notes

> 范围：`Lecture/07_Enum.pdf`，主要是 `enum`、`ordinal()`、`name()`、enum as class、enum constructor、enum instance variable、`values()`。
> 读法：先看第 0 节速查，再重点看第 2 节 `ordinal()` / `name()`、第 3 节 enum as class。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Enumerated Type 必背

| 问题 | 答案 |
|---|---|
| `enum` 用来做什么？ | 定义一个 new type，它的值只能从列出的固定值中选 |
| 适合什么场景？ | variable 只有少数几个可能值，例如 seasons、flavors、directions |
| 定义 season | `enum Season {WINTER, SPRING, SUMMER, FALL}` |
| enum value 如何访问？ | 通过 type name，例如 `Season.SPRING` |
| `ordinal()` 返回什么？ | enum value 的 internal integer position，从 `0` 开始 |
| `Season.SPRING.ordinal()` | `1` |
| `name()` 返回什么？ | enum value 的 exact name，类型是 `String` |
| `Season.SPRING.name()` | `"SPRING"` |
| enum 是否是 class？ | 是，enum 是 special kind of class |
| enum 里面能不能有 constructor？ | 可以 |
| enum 里面能不能有 methods / instance variables？ | 可以 |
| enum 是否能 new？ | 不应该由外部 `new`，possible objects 已经在 enum declaration 中列出 |
| `values()` 返回什么？ | all possible enum values，返回一个 enum array |
| enum constructor 通常是什么 visibility？ | 不写 `public`，enum constructor 只服务 enum constants |

### IceCream 例子必背

```java
enum Flavor {COFFEE, VANILLA, STRAWBERRY, CHOCOLATE, MINT}

Flavor cone1 = Flavor.MINT;
Flavor cone2 = Flavor.COFFEE;
Flavor cone3 = cone1;
```

| Expression | Value |
|---|---|
| `cone1` | `MINT` |
| `cone1.ordinal()` | `4` |
| `cone1.name()` | `"MINT"` |
| `cone2` | `COFFEE` |
| `cone2.ordinal()` | `0` |
| `cone3` | `MINT` |
| `cone3.ordinal()` | `4` |

---

## 1. What Is an Enumerated Type?

`Enumerated Type` 让 programmer 可以定义一个新的 type，并且把这个 type 的所有 possible values 一次列出来。

最典型例子：

```java
enum Season {WINTER, SPRING, SUMMER, FALL}
```

这行代码的意思：

1. 定义了一个新的 type：`Season`。
2. `Season` variable 只能存四个值之一。
3. 这四个值是 `Season.WINTER`、`Season.SPRING`、`Season.SUMMER`、`Season.FALL`。

使用方式：

```java
Season time;
time = Season.SPRING;
```

注意：

```java
time = SPRING;        // usually wrong outside enum context
time = "SPRING";      // wrong: String is not Season
time = Season.SPRING; // correct
```

### 1.1 为什么不用 `int` 或 `String`？

如果用 `int`：

```java
int season = 2;
```

问题是：

- `2` 到底代表 spring 还是 summer 不直观。
- `season = 99;` 也能 compile，但 logically invalid。

如果用 `String`：

```java
String season = "SPRING";
```

问题是：

- `"Spring"`、`"spring"`、`"SPRNG"` 都可能造成错误。
- compiler 不能帮你限制 legal values。

用 enum：

```java
Season season = Season.SPRING;
```

优点：

- possible values 固定。
- readable。
- compiler 可以帮你检查 type。

---

## 2. `ordinal()` and `name()`

### 2.1 `ordinal()`

每个 enum value 内部都有一个 integer position，叫 `ordinal value`。

规则：

```text
first value  -> ordinal 0
second value -> ordinal 1
third value  -> ordinal 2
...
```

Example：

```java
enum Season {WINTER, SPRING, SUMMER, FALL}
```

| Value | ordinal |
|---|---:|
| `Season.WINTER` | `0` |
| `Season.SPRING` | `1` |
| `Season.SUMMER` | `2` |
| `Season.FALL` | `3` |

So：

```java
Season time = Season.SPRING;
System.out.println(time.ordinal()); // 1
```

考试易错：

```text
ordinal starts from 0, not 1.
```

### 2.2 `name()`

`name()` returns the exact declared name of the enum constant。

```java
Season time = Season.SPRING;
System.out.println(time.name()); // SPRING
```

`name()` 的 return type 是 `String`。

所以：

```java
String s = time.name();
```

### 2.3 Printing enum directly

如果直接 print enum value：

```java
System.out.println(time);
```

通常输出 enum constant name：

```text
SPRING
```

也就是说，在课件例子里：

```java
System.out.println("cone1 value: " + cone1);
```

输出：

```text
cone1 value: MINT
```

---

## 3. IceCream Example

Lecture 例子：

```java
public class IceCream {
    enum Flavor {COFFEE, VANILLA, STRAWBERRY, CHOCOLATE, MINT}

    public static void main(String[] args) {
        Flavor cone1, cone2, cone3;

        cone1 = Flavor.MINT;
        cone2 = Flavor.COFFEE;

        System.out.println("cone1 value: " + cone1);
        System.out.println("cone1 ordinal: " + cone1.ordinal());
        System.out.println("cone1 name: " + cone1.name());

        System.out.println("cone2 value: " + cone2);
        System.out.println("cone2 ordinal: " + cone2.ordinal());
        System.out.println("cone2 name: " + cone2.name());

        cone3 = cone1;

        System.out.println("cone3 value: " + cone3);
        System.out.println("cone3 ordinal: " + cone3.ordinal());
        System.out.println("cone3 name: " + cone3.name());
    }
}
```

### 3.1 Ordinal table

```java
enum Flavor {COFFEE, VANILLA, STRAWBERRY, CHOCOLATE, MINT}
```

| Value | ordinal |
|---|---:|
| `COFFEE` | `0` |
| `VANILLA` | `1` |
| `STRAWBERRY` | `2` |
| `CHOCOLATE` | `3` |
| `MINT` | `4` |

### 3.2 Output

```text
cone1 value: MINT
cone1 ordinal: 4
cone1 name: MINT

cone2 value: COFFEE
cone2 ordinal: 0
cone2 name: COFFEE

cone3 value: MINT
cone3 ordinal: 4
cone3 name: MINT
```

### 3.3 `cone3 = cone1` 怎么理解？

`Flavor` 是 reference-like enum type，但 enum constants 是固定 objects。

```java
cone3 = cone1;
```

意思是：

- `cone3` now refers to same enum constant as `cone1`。
- `cone1` is `Flavor.MINT`。
- therefore `cone3` is also `Flavor.MINT`。

---

## 4. Enumerated Type as Class

Lecture 重点：

```text
Enumerated type is a special kind of class.
```

这意味着 enum 可以包含：

1. constants。
2. instance variables。
3. constructors。
4. methods。

但是 enum 的 possible objects 已经在 enum definition 的开头定义好了。

### 4.1 Season with constructor

Lecture 例子：

```java
public enum Season {
    WINTER("December through February"),
    SPRING("March through May"),
    SUMMER("June through August"),
    FALL("September through November");

    private String span;

    Season(String months) {
        span = months;
    }

    public String getSpan() {
        return span;
    }
}
```

每个 enum constant 都像是调用 constructor 创建好的 object：

```java
WINTER("December through February")
SPRING("March through May")
SUMMER("June through August")
FALL("September through November")
```

`span` 是 instance variable，每个 enum value 都有自己的 `span`。

### 4.2 Semicolon after enum constants

如果 enum 只有 constants：

```java
enum Season {WINTER, SPRING, SUMMER, FALL}
```

可以简单写。

如果 enum 后面还有 variables / constructors / methods：

```java
public enum Season {
    WINTER("December through February"),
    SPRING("March through May"),
    SUMMER("June through August"),
    FALL("September through November");

    private String span;
}
```

注意 constants list 后面需要 `;`。

---

## 5. `values()` Method

Every enum type contains a method called `values()`。

它返回所有 enum values，类型是 enum array。

Example：

```java
for (Season time : Season.values()) {
    System.out.println(time + "\t" + time.getSpan());
}
```

`Season.values()` 相当于：

```java
{Season.WINTER, Season.SPRING, Season.SUMMER, Season.FALL}
```

输出：

```text
WINTER  December through February
SPRING  March through May
SUMMER  June through August
FALL    September through November
```

考试重点：

- `values()` 是 enum type 的 method。
- return value 可以被 enhanced for loop 遍历。
- `time` 依次拿到每个 enum constant。

---

## 6. Exam-Style Tracing

### 6.1 Basic ordinal tracing

```java
enum Direction {NORTH, EAST, SOUTH, WEST}

Direction d = Direction.SOUTH;
System.out.println(d);
System.out.println(d.ordinal());
System.out.println(d.name());
```

Output：

```text
SOUTH
2
SOUTH
```

Reason：

| Value | ordinal |
|---|---:|
| `NORTH` | `0` |
| `EAST` | `1` |
| `SOUTH` | `2` |
| `WEST` | `3` |

### 6.2 Enum with method

```java
enum Level {
    LOW(1), MEDIUM(2), HIGH(3);

    private int code;

    Level(int code) {
        this.code = code;
    }

    public int getCode() {
        return code;
    }
}
```

Trace：

```java
Level x = Level.HIGH;
System.out.println(x.ordinal());
System.out.println(x.getCode());
```

Output：

```text
2
3
```

注意：

- `ordinal()` depends on declaration order。
- `getCode()` returns the instance variable value passed to constructor。
- 两者不一定一样。

---

## 7. 考试答题模板

### 7.1 问：什么是 enumerated type？

可答：

```text
An enumerated type defines a new type whose values are restricted to a fixed list of named constants. It is useful when a variable should only take a few possible values, such as seasons.
```

中文理解：

```text
enum 就是把所有合法值列出来，让变量只能取这些值，避免用 int/String 造成不清楚或非法值。
```

### 7.2 问：`ordinal()` 和 `name()` 区别？

可答：

```text
ordinal() returns the integer position of the enum constant, starting from 0. name() returns the declared name of the enum constant as a String.
```

### 7.3 问：enum 为什么可以有 constructor？

可答：

```text
An enum is a special kind of class. Each enum constant is an object of that enum type, so the enum can define instance variables, constructors, and methods to store extra information for each constant.
```

---

## 8. 最容易错的点

1. `ordinal()` 从 `0` 开始，不是从 `1` 开始。
2. enum value 要写成 `Type.VALUE`，例如 `Season.SPRING`。
3. `name()` returns a `String`，不是 enum 本身。
4. enum constants declaration order 会影响 `ordinal()`。
5. enum 可以有 constructor / methods / instance variables。
6. enum constants 后面如果还有 fields/methods，要加 `;`。
7. `values()` 返回的是 array，所以 enhanced for loop 很常见。

---

## 9. 最后 30 秒记忆版

```text
enum Season {WINTER, SPRING, SUMMER, FALL}

Season.SPRING.ordinal() -> 1
Season.SPRING.name()    -> "SPRING"

enum is a special class:
- fixed constants
- can have fields
- can have constructor
- can have methods
- values() returns all constants as array
```
