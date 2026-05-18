# Lecture 3: Introduction to Classes - Exam Notes

> 范围：`Lecture/03_Intro_Class.pdf`，主要是 class/object、state/behavior、Die class、constructors、`toString()`、instance data、local data、UML class diagram。
> 读法：先看第 0 节速查，再重点看第 2 节 Die class、第 4 节 constructor、第 5 节 `toString()`、第 7 节 UML。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Classes 必背

| 问题 | 答案 |
|---|---|
| Java program 起点在哪里？ | class containing `main` method |
| OOP 设计核心 | define classes that represent objects |
| object 有什么？ | state + behaviors |
| state 由什么定义？ | attributes / instance variables |
| behavior 由什么定义？ | operations / methods |
| class 是什么？ | blueprint for objects |
| object 是什么？ | instance of a class |
| class 可以包含什么？ | data declarations + method declarations |
| instance data 是什么？ | class level variable；each object has its own copy |
| local data 是什么？ | method 内声明的 variable，只在 method 内可用 |
| constructor 是什么？ | special method used to set up object when created |
| constructor 名字 | same as class name |
| constructor return type | none，连 `void` 都不能写 |
| default constructor | no-argument constructor provided if programmer defines none |
| constructors 可以 overload 吗？ | 可以，parameter list different |
| `toString()` return type | `String` |
| `toString()` 什么时候自动调用？ | object concatenated with string or passed to `println` |
| UML `+` | public |
| UML `-` | private |
| UML `#` | protected |

---

## 1. Why Classes?

### 1.1 `System.out.println()` 不需要 instantiation 的原因

`System` 是 `java.lang` package 中的 class。

`java.lang.*` is imported by default，所以不用显式 import。

`System.out` 是 `System` class 的 static member field，type 是 `PrintStream`。

因此：

```java
System.out.println("Hello");
```

可以直接通过 class name 使用，不需要：

```java
System sys = new System();
```

这个问题会在 Lecture 10 static modifier 中详细解释。

### 1.2 Writing classes

Java 中：

- class containing `main` 是 program starting point。
- 真正 OOP program 会定义多个 classes。
- 每个 class 表示一个 concept / object type。
- 一个 program 可以由多个 `.java` files 组成。

重点：

```text
The class that contains main is just the starting point.
```

业务逻辑通常放在其他 classes 里。

### 1.3 Class represents state and behavior

Object：

```text
state + behaviors
```

Class 中：

```text
state -> data declarations / attributes / instance variables
behaviors -> method declarations / operations
```

例子：six-sided die

| Object idea | Java class design |
|---|---|
| current face showing | `faceValue` instance variable |
| can be rolled | `roll()` method |

---

## 2. The `Die` Class

### 2.1 Die class state

Lecture 例子：

```java
private final int MAX = 6;
private int faceValue;
```

含义：

- `MAX` 是 maximum face value。
- `faceValue` 是 current value showing on the die。

`faceValue` 是 instance variable。

每个 `Die` object 有自己的 `faceValue`。

### 2.2 Die constructor

```java
public Die() {
    faceValue = 1;
}
```

作用：

```text
Sets the initial face value when object is created.
```

当执行：

```java
Die die1 = new Die();
```

就会调用 constructor。

### 2.3 `roll()` method

```java
public int roll() {
    faceValue = (int)(Math.random() * MAX) + 1;
    return faceValue;
}
```

解释：

```text
Math.random() gives a double in [0, 1).
Math.random() * 6 gives [0, 6).
(int) truncates to 0, 1, 2, 3, 4, 5.
+1 gives 1 to 6.
```

所以 `roll()`：

1. changes state。
2. returns new face value。

### 2.4 Accessor and mutator

Accessor：

```java
public int getFaceValue() {
    return faceValue;
}
```

Mutator：

```java
public void setFaceValue(int value) {
    faceValue = value;
}
```

命名：

```text
getX -> accessor
setX -> mutator
```

### 2.5 `toString()`

```java
public String toString() {
    String result = Integer.toString(faceValue);
    return result;
}
```

作用：

```text
Returns a string representation of this die.
```

如果写：

```java
System.out.println("Die One: " + die1);
```

Java 会自动调用：

```java
die1.toString()
```

---

## 3. Using a User-Defined Class

### 3.1 `RollingDice` driver

`RollingDice` 是 driver program，用来创建和使用 `Die` objects。

核心结构：

```java
public class RollingDice {
    public static void main(String[] args) {
        Die die1, die2;
        int sum;

        die1 = new Die();
        die2 = new Die();

        die1.roll();
        die2.roll();

        System.out.println("Die One: " + die1 + ", Die Two: " + die2);
    }
}
```

### 3.2 Object reference variables

```java
Die die1, die2;
```

只是声明 references。

真正 object 创建在：

```java
die1 = new Die();
die2 = new Die();
```

### 3.3 Method invocation

通过 object 调用 method：

```java
die1.roll();
die2.setFaceValue(4);
int value = die1.getFaceValue();
```

格式：

```text
objectReference.methodName(arguments)
```

### 3.4 Each object has own state

如果：

```java
Die die1 = new Die();
Die die2 = new Die();
die1.roll();
die2.setFaceValue(4);
```

`die1.faceValue` 和 `die2.faceValue` 独立。

Changing one object does not automatically change the other object。

---

## 4. Data Scope and Instance Data

### 4.1 Data scope

`scope` 是 data 可以被 referenced 的 area。

| Data location | Scope |
|---|---|
| class level | all methods in that class |
| inside method | only that method |

### 4.2 Local data

```java
public String toString() {
    String result = Integer.toString(faceValue);
    return result;
}
```

`result` 是 local data。

只能在 `toString()` 内使用。

Method 执行完：

```text
local variables are destroyed.
```

### 4.3 Instance data

```java
private int faceValue;
```

`faceValue` 是 instance data。

特点：

- declared at class level。
- each object has its own copy。
- exists as long as object exists。
- all methods in class can access it。

### 4.4 Class declares type, object reserves memory

课件重点：

```text
A class declares the type of the data, but it does not reserve memory space for it.
Each time a Die object is created, a new faceValue variable is created as well.
```

直觉：

```text
Class is blueprint.
Object is actual house.
```

Blueprint 本身不占每栋房子的房间空间。

---

## 5. Constructors

### 5.1 Constructor rules

Constructor：

```text
special method used to set up an object when initially created
```

Rules：

1. Same name as class。
2. No return type specified, not even `void`。
3. Usually initializes instance variables。
4. Programmer does not have to define one。
5. If no constructor is defined, Java provides default no-argument constructor。
6. Multiple constructors can be defined if parameter lists differ。

### 5.2 Common constructor mistake

错误：

```java
public void Die() {
    faceValue = 1;
}
```

这不是 constructor。

它是一个 return type 为 `void`、名字叫 `Die` 的普通 method。

正确：

```java
public Die() {
    faceValue = 1;
}
```

### 5.3 Random initial face constructor

Test Your Understanding：

```java
public class Die {
    private int faceValue;

    public Die() {
        faceValue = (int)(Math.random() * 6) + 1;
    }
}
```

### 5.4 Constructor with parameters

Movie example：

```java
public Movie(String theTitle, String theDirector) {
    title = theTitle;
    director = theDirector;
}
```

作用：

```text
initializes instance variables using parameters.
```

### 5.5 Constructor overloading

可以有多个 constructors：

```java
public Die() {
    faceValue = 1;
}

public Die(int value) {
    faceValue = value;
}
```

它们 parameter list 不同，所以合法。

---

## 6. `toString()` Method

### 6.1 Why `toString()` matters

如果 class 没有 useful `toString()`，打印 object 可能得到：

```text
Student@1fee6fc
Student@1eed786
```

这是默认 object representation，不是我们想要的 state。

### 6.2 Define `toString()`

Example：

```java
public String toString() {
    return id + " " + name + " " + city;
}
```

如果：

```java
System.out.println(student);
```

会自动调用：

```java
student.toString()
```

### 6.3 `toString()` return type

必须是：

```java
String
```

不是 `void`。

错误：

```java
public void toString() { } // wrong for overriding Object.toString
```

正确：

```java
public String toString() {
    return "...";
}
```

---

## 7. Define a Class Template

### 7.1 Basic class template

```java
public class ClassName {
    private int field1;
    private String field2;

    public ClassName(int field1, String field2) {
        this.field1 = field1;
        this.field2 = field2;
    }

    public int getField1() {
        return field1;
    }

    public void setField1(int newValue) {
        field1 = newValue;
    }

    public String toString() {
        return field1 + " " + field2;
    }
}
```

### 7.2 Getter / setter example

For `Child` with instance data `age`：

```java
public int getAge() {
    return age;
}

public void setAge(int newAge) {
    age = newAge;
}
```

### 7.3 Class does not always need `main`

A class can be designed only as object blueprint。

For example：

```java
public class Die {
    // no main method needed
}
```

Another driver class can use it：

```java
public class RollingDice {
    public static void main(String[] args) {
        Die die = new Die();
    }
}
```

---

## 8. UML Class Diagram

### 8.1 UML purpose

UML class diagram is graphical notation to visualize object-oriented systems。

It can show：

- classes。
- attributes。
- operations / methods。
- relationships among objects/classes。

### 8.2 UML class notation

A class is shown as rectangle with up to three sections：

```text
-------------------------
ClassName
-------------------------
attributes
-------------------------
operations
-------------------------
```

Example：

```text
-------------------------
Die
-------------------------
- MAX : int
- faceValue : int
-------------------------
+ Die()
+ roll() : int
+ setFaceValue(value : int) : void
+ getFaceValue() : int
+ toString() : String
-------------------------
```

### 8.3 Visibility in UML

| Symbol | Java visibility |
|---|---|
| `+` | public |
| `-` | private |
| `#` | protected |

### 8.4 Method signature

Method signature involves：

```text
method name + parameter list
```

UML operation can show return type：

```text
+ roll() : int
+ setFaceValue(value : int) : void
```

---

## 9. 考试答题模板

### 9.1 Define class from description

Steps：

```text
1. Identify attributes -> instance variables.
2. Identify operations -> methods.
3. Make instance variables private.
4. Write constructor to initialize state.
5. Add getters/setters if needed.
6. Add toString if object needs useful printing.
```

### 9.2 Constructor question

Prompt：

```text
Write a constructor for Movie that initializes title and director.
```

Answer：

```java
public Movie(String theTitle, String theDirector) {
    title = theTitle;
    director = theDirector;
}
```

### 9.3 Getter/setter question

```java
public int getAge() {
    return age;
}

public void setAge(int newAge) {
    age = newAge;
}
```

### 9.4 UML question

Checklist：

```text
Class name at top.
Attributes in middle with visibility and type.
Methods at bottom with visibility, parameters, and return type.
Use + for public, - for private, # for protected.
```

---

## 10. 最容易错的点

### 10.1 Declaration 不等于 object creation

```java
Die die1;
```

没有创建 object。

要：

```java
die1 = new Die();
```

### 10.2 Constructor 不能写 return type

错误：

```java
public void Die() { }
```

正确：

```java
public Die() { }
```

### 10.3 Instance data 和 local data 混淆

`faceValue`：

```java
private int faceValue;
```

是 instance data。

`result`：

```java
String result = ...
```

在 method 内，是 local data。

### 10.4 `toString()` 不 print，应该 return String

更好的 `toString()`：

```java
public String toString() {
    return faceValue + "";
}
```

不要写成只 `System.out.println(...)`。

### 10.5 每个 object 有自己的 instance variables

`die1.faceValue` 和 `die2.faceValue` 是不同 copy。

Method definitions shared，但 data space separate。

---

## 11. 最后 30 秒记忆版

```text
Class = blueprint.
Object = instance.
Object has state + behaviors.
State = attributes / instance variables.
Behavior = methods.

Class contains data declarations + method declarations.

Instance data:
declared at class level, each object has own copy.

Local data:
declared inside method, exists only while method runs.

Constructor:
same name as class, no return type, initializes object.
Default constructor exists only if no constructor is defined.
Constructors can be overloaded.

toString():
returns String representation.
Automatically called by println or string concatenation.

UML:
+ public
- private
# protected
class rectangle: name / attributes / operations.
```
