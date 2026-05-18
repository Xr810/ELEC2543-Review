# Lecture 9: Methods - Exam Notes

> 范围：`Lecture/09_Method.pdf`，主要是 method header/body、return statement、method overloading、parameter passing、objects/arrays as parameters、command-line arguments、variable length parameter lists。
> 读法：先看第 0 节速查，再重点看第 4 节 parameter passing、第 5 节 object reference tracing、第 7 节 varargs。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Methods 必背

| 问题 | 答案 |
|---|---|
| method declaration 从什么开始？ | method header |
| method header 包含什么？ | return type、method name、parameter list |
| parameter list 包含什么？ | 每个 parameter 的 type 和 name |
| formal parameter 是什么？ | method declaration/header 中的 parameter name |
| actual parameter 是什么？ | method call 时传进去的 expression/value |
| method body 是什么？ | `{ ... }` 中的 statements |
| local data 生命周期 | method call 时创建，method 结束时销毁 |
| `return` expression 要符合什么？ | method return type |
| `void` method 是否 return value？ | no |
| method overloading 是什么？ | same method name, multiple definitions |
| overloaded methods 靠什么区分？ | method signature |
| signature 包含什么？ | number、type、order of parameters |
| return type 是否属于 signature？ | no |
| overloaded methods 能否只靠 return type 不同？ | 不能 |
| constructors 能否 overload？ | yes |

### Parameter Passing 必背

| 情况 | 结果 |
|---|---|
| primitive parameter | pass by value，formal parameter gets copy |
| object parameter | reference value is copied；formal and actual refer to same object |
| object state changed inside method | outside can see change |
| formal reference reassigned inside method | outside original reference not changed |
| array parameter | array reference copied；formal and actual are aliases to same array |
| array element changed inside method | original array changes |
| individual array element passed | behaves like that element's type |

### Varargs 必背

| 问题 | 答案 |
|---|---|
| syntax | `int ... list` |
| inside method behaves like | array |
| use length | `list.length` |
| varargs position | must be last formal parameter |
| one method can have how many varargs lists？ | at most one |
| call with no variable args allowed？ | yes, length is `0` |

---

## 1. Method Header and Body

### 1.1 Method header

Lecture example：

```java
char calc(int num1, int num2, String message)
```

Parts：

| Part | Example |
|---|---|
| return type | `char` |
| method name | `calc` |
| parameter list | `int num1, int num2, String message` |

`num1`、`num2`、`message` are formal parameters。

### 1.2 Method body

```java
char calc(int num1, int num2, String message) {
    int sum = num1 + num2;
    char result = message.charAt(sum);
    return result;
}
```

Inside body：

- `sum` is local data。
- `result` is local data。
- local data is created each time method is called。
- local data is destroyed when method finishes executing。

### 1.3 Return statement

```java
return expression;
```

Rule：

```text
return expression must conform to return type.
```

Examples：

```java
int f() {
    return 3;      // correct
}

char g() {
    return 'A';    // correct
}

void h() {
    return;        // optional early exit, no value
}
```

Wrong：

```java
int f() {
    return "hello"; // wrong
}

void h() {
    return 3;       // wrong
}
```

---

## 2. Method Overloading

### 2.1 Definition

`Method overloading` means giving a single method name multiple definitions。

Example：

```java
float tryMe(int x) {
    return x + .375f;
}

float tryMe(int x, float y) {
    return x * y;
}
```

Call：

```java
result = tryMe(25, 4.32f);
```

The compiler chooses the second version because argument list is `(int, float)`。

### 2.2 Signature

Signature includes：

```text
number, type, and order of parameters
```

Signature does not include return type。

Valid overloads：

```java
void print(int x) {}
void print(double x) {}
void print(int x, double y) {}
void print(double y, int x) {}
```

Invalid overload：

```java
int value(int x) { return x; }
double value(int x) { return x; } // wrong: only return type differs
```

### 2.3 `println` is overloaded

The `println` method is overloaded：

```java
println(String s)
println(int i)
println(double d)
```

So both are legal：

```java
System.out.println("The total is:");
System.out.println(total);
```

### 2.4 Constructors can be overloaded

Constructors can have multiple parameter lists：

```java
public Point() {
    x = 0;
    y = 0;
}

public Point(int xValue, int yValue) {
    x = xValue;
    y = yValue;
}
```

This provides multiple ways to initialize a new object。

---

## 3. Actual vs Formal Parameters

When a method is called, actual parameters are copied into formal parameters。

Example：

```java
ch = obj.calc(25, count, "Hello");
```

Method：

```java
char calc(int num1, int num2, String message)
```

Mapping：

| Actual parameter | Formal parameter |
|---|---|
| `25` | `num1` |
| `count` value | `num2` |
| `"Hello"` reference | `message` |

Important：

```text
Passing parameters is similar to an assignment statement.
```

---

## 4. Primitive Parameters

Primitive values are passed by value。

Meaning：

```text
formal parameter receives a copy of the actual value.
```

Example：

```java
public void change(int x) {
    x = 999;
}

int a = 111;
change(a);
System.out.println(a); // 111
```

Why？

- `a` holds `111`。
- method call copies `111` into `x`。
- changing `x` does not change `a`。

Exam answer sentence：

```text
Changing a primitive formal parameter has no permanent effect on the actual variable.
```

---

## 5. Objects as Parameters

### 5.1 Important model

Lecture says object parameter is considered passed by reference, meaning the formal and actual parameters become aliases of each other。

More precise Java model：

```text
The object reference value is copied. The formal parameter and actual parameter refer to the same object.
```

For this course, the key practical rule is：

```text
If the method changes the object's internal state, the change is visible outside.
If the method reassigns the formal reference to a new object, the caller's reference is not changed.
```

### 5.2 Lecture classes

```java
public class Num {
    private int value;

    public Num(int update) {
        value = update;
    }

    public void setValue(int update) {
        value = update;
    }

    public String toString() {
        return value + "";
    }
}
```

Original tester：

```java
int a1 = 111;
Num a2 = new Num(222);
Num a3 = new Num(444);

modifier.changeValues(a1, a2, a3);
```

Original modifier：

```java
public void changeValues(int f1, Num f2, Num f3) {
    f1 = 999;
    f2.setValue(888);
    f3 = new Num(777);
}
```

### 5.3 Trace original program

Before call：

```text
a1 -> 111
a2 -> Num(222)
a3 -> Num(444)
```

At method entry：

```text
f1 = 111
f2 -> same Num object as a2
f3 -> same Num object as a3
```

After:

```java
f1 = 999;
```

Only local copy changes：

```text
a1 still 111
f1 becomes 999
```

After:

```java
f2.setValue(888);
```

This changes the object shared by `a2` and `f2`：

```text
a2 now sees Num(888)
```

After:

```java
f3 = new Num(777);
```

Only local reference `f3` points to new object：

```text
a3 still points to original Num(444)
f3 points to new Num(777)
```

Final output outside：

```text
After calling changeValues:
a1  a2  a3
111 888 444
```

### 5.4 Most important distinction

Object state mutation：

```java
f2.setValue(888);
```

Permanent effect outside because same object。

Reference reassignment：

```java
f3 = new Num(777);
```

No effect on caller reference because only formal parameter is reassigned。

---

## 6. Updated Parameter Exercise

Lecture updated method：

```java
public void changeValues(int a1, Num a2, Num a3) {
    System.out.println("Before changing the values:");
    System.out.println(a1 + "\t" + a2 + "\t" + a3 + "\n");

    a2.setValue(a1);
    a1 = 999;
    Num a4 = a2;
    a2 = a3;
    a3 = a4;

    System.out.println("After changing the values:");
    System.out.println(a1 + "\t" + a2 + "\t" + a3 + "\n");
}
```

Caller before：

```java
int a1 = 111;
Num a2 = new Num(222);
Num a3 = new Num(444);
```

### 6.1 Step-by-step trace

At method start：

```text
local a1 = 111
local a2 -> same object as caller a2, value 222
local a3 -> same object as caller a3, value 444
```

Execute：

```java
a2.setValue(a1);
```

The object referenced by caller `a2` changes from `222` to `111`。

Now：

```text
caller a2 -> Num(111)
local a2  -> Num(111)
caller a3 -> Num(444)
local a3  -> Num(444)
```

Execute：

```java
a1 = 999;
```

Only local primitive changes：

```text
caller a1 still 111
local a1 becomes 999
```

Execute：

```java
Num a4 = a2;
```

`a4` points to same object as local `a2`：

```text
a4 -> Num(111)
```

Execute：

```java
a2 = a3;
```

Local `a2` now points to object value `444`。

Execute：

```java
a3 = a4;
```

Local `a3` now points to object value `111`。

### 6.2 Inside method output

Inside after changes：

```text
a1  a2  a3
999 444 111
```

### 6.3 Outside after method returns

Caller variables：

```text
caller a1 remains 111
caller a2 still points to original a2 object, but its value was changed to 111
caller a3 still points to original a3 object, value 444
```

Outside output：

```text
After calling changeValues:
a1  a2  a3
111 111 444
```

Garbage?

```text
No new Num object is created in the updated method, so no garbage is created by this method.
```

---

## 7. Arrays as Parameters

An entire array can be passed as a parameter。

Because array is an object：

```text
the reference to the array is passed/copied, making formal and actual parameters aliases of the same array.
```

### 7.1 Example

```java
public static void changeElement(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        arr[i] *= 2;
    }
}
```

Caller：

```java
int[] list = {0, 10, 20, 30, 40};
changeElement(list);
```

After call：

```text
list = {0, 20, 40, 60, 80}
```

Reason：

- `arr` and `list` refer to the same array object。
- modifying `arr[i]` modifies the original array object。

### 7.2 Individual array element

An individual array element can be passed too：

```java
changeOne(list[2]);
```

If `list[2]` is an `int`, then it behaves like passing an `int` value。

Changing the formal parameter inside `changeOne` will not modify `list[2]` unless the method is given the whole array or another mutable object reference。

---

## 8. Command-Line Arguments

The main method signature：

```java
public static void main(String[] args)
```

`args` is an array of `String` objects。

These values come from command-line arguments。

Example command：

```text
java NameTag Howdy John
```

Then：

```java
args[0] // "Howdy"
args[1] // "John"
```

Program：

```java
public class NameTag {
    public static void main(String[] args) {
        System.out.println();
        System.out.println("     " + args[0]);
        System.out.println("My name is " + args[1]);
    }
}
```

Output：

```text
Howdy
My name is John
```

If no argument is provided：

```text
args.length == 0
```

Then accessing `args[0]` causes：

```text
ArrayIndexOutOfBoundsException
```

---

## 9. Variable Length Parameter Lists

### 9.1 Why varargs?

Sometimes a method should process a different amount of data each time。

Example：

```java
mean1 = average(42, 69, 37);
mean2 = average(35, 43, 93, 23, 40, 21, 75);
```

### 9.2 Syntax

```java
public double average(int ... list)
```

`int ... list` means:

- any number of `int` arguments。
- inside method, `list` behaves like an array。

Example：

```java
public static double average(int ... list) {
    double sum = 0;

    if (list.length == 0) {
        return sum;
    }

    for (int value : list) {
        sum += value;
    }

    return sum / list.length;
}
```

Calls：

```java
average();                    // list.length == 0
average(3, 4);                // list.length == 2
average(3, 4, 10, 20, 30);    // list.length == 5
```

### 9.3 Varargs with other parameters

Varargs must come last：

```java
public static double sumDividedBy(int v, int ... list) {
    double sum = 0;

    for (int value : list) {
        sum += value;
    }

    return sum / v;
}
```

Valid call：

```java
sumDividedBy(4, 5, 6);
sumDividedBy(4, 5, 6, 7);
```

Invalid pattern：

```java
public void bad(int ... nums, String name) // wrong
```

Also invalid：

```java
public void bad(int ... nums, double ... values) // wrong
```

Because a single method cannot accept two sets of varying parameters。

---

## 10. 考试答题模板

### 10.1 问：return type 是否属于 method signature？

```text
No. A method signature includes the method name plus the number, type, and order of parameters. Return type is not part of the signature, so overloaded methods cannot differ only by return type.
```

### 10.2 问：primitive parameter 为什么不会改外面的变量？

```text
Primitive data is passed by value. The formal parameter receives a copy of the actual value, so assigning a new value to the formal parameter only changes the local copy.
```

### 10.3 问：object parameter 为什么有时会改外面，有时不会？

```text
The formal parameter and actual parameter refer to the same object, so changing the object's internal state is visible outside. However, reassigning the formal parameter to another object only changes the local reference, not the caller's reference.
```

### 10.4 问：array parameter 改 element 是否影响 original？

```text
Yes. Arrays are objects. Passing an array copies the array reference, so the formal parameter and actual parameter are aliases of the same array object. Changing an element changes the original array.
```

---

## 11. 最容易错的点

1. return type is not part of method signature。
2. overloaded methods 不能只 return type 不同。
3. formal parameter 是 method header 里的变量；actual parameter 是 call 里传入的值。
4. primitive parameter 改 formal 不影响 caller。
5. object parameter 改 object state 会影响 caller。
6. object parameter 重新赋值 formal reference 不影响 caller。
7. array parameter 改 element 会影响 original array。
8. `args` 是 `String[]`；没有 command-line args 时 `args.length == 0`。
9. varargs inside method behaves like array。
10. varargs must be last, and only one varargs list is allowed。

---

## 12. 最后 30 秒记忆版

```text
Method signature = name + parameter number/type/order
Return type NOT included

Primitive parameter:
copy value -> changing formal does not affect actual

Object parameter:
copy reference -> same object
change state -> outside sees it
reassign formal reference -> outside does not change

Array parameter:
same array object -> arr[i] changes original

Varargs:
int ... list
inside method: list behaves like array
must be last parameter
```
