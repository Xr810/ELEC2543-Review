# Lecture 8: Arrays and ArrayList - Exam Notes

> 范围：`Lecture/08_Array.pdf`，主要是 array declaration、array indexing、bounds checking、arrays of objects、2D arrays、array examples、dynamic resizing、`ArrayList`。
> 读法：先看第 0 节速查，再重点看第 2 节 bounds、第 3 节 object arrays、第 5 节 DVDCollection、第 7 节 ArrayList。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Arrays 必背

| 问题 | 答案 |
|---|---|
| array 是什么？ | ordered list of values |
| array of size `N` index 范围 | `0` to `N - 1` |
| `scores[2]` 是第几个 element？ | third value |
| array element type 可以是什么？ | primitive type 或 object reference |
| Java 中 array 本身是否是 object？ | yes，must be instantiated |
| 声明 array | `int[] scores;` 或 `int scores[];` |
| instantiate array | `scores = new int[10];` |
| C-style 错误写法 | `int scores[10];` |
| 初始化 array | `char[] vowels = {'A', 'E', 'I', 'O', 'U'};` |
| array size 是否固定？ | fixed once created |
| array length | `scores.length` |
| `length` 是 largest index 吗？ | 不是，是 number of elements |
| largest valid index | `array.length - 1` |
| 越界 exception | `ArrayIndexOutOfBoundsException` |
| enhanced for 适合什么？ | 从头到尾处理所有 elements |
| enhanced for 不适合什么？ | 需要 index、倒序、修改原 array index-specific logic 时 |

### Object Arrays 必背

| 问题 | 答案 |
|---|---|
| `String[] words = new String[5];` 创建了几个 `String` objects？ | 0 个，只创建 5 个 references |
| object array 初始值 | `null` references |
| object array 每个 object 是否要 separately instantiated？ | yes |
| `words[0]` 未赋值时是什么？ | `null` |
| 调用 `words[0].length()` 会怎样？ | `NullPointerException` |

### ArrayList 必背

| 问题 | 答案 |
|---|---|
| package | `java.util` |
| array size | fixed |
| `ArrayList` size | grows and shrinks as needed |
| declare generic ArrayList | `ArrayList<Integer> intList;` |
| instantiate | `intList = new ArrayList<Integer>();` |
| add at end | `add(element)` |
| add at position | `add(position, element)` |
| remove by index | `remove(pos)` |
| remove by object | `remove(element)` |
| get element | `get(pos)` |
| number of elements | `size()` |
| first occurrence index | `indexOf(object)` |
| not found result | `-1` |

---

## 1. Array Basic Concept

An array is an ordered list of values。

Example:

```text
scores
index:  0   1   2   3   4   5   6   7   8   9
value: 79  87  94  82  67  98  87  81  74  91
```

Important rules：

1. The entire array has a single name：`scores`。
2. Each value has a numeric index。
3. Index starts from `0`。
4. Array of size `N` has valid indexes `0` through `N - 1`。

So：

```java
scores[2]
```

means the third value, which is `94` in the diagram。

### 1.1 Array element

The values held in an array are called `array elements`。

All elements in the same array have the same element type：

```java
int[] scores;
char[] letters;
String[] words;
DVD[] collection;
```

Element type can be：

- primitive type：`int`, `double`, `char`, `boolean`
- object reference type：`String`, `DVD`, `Coin`

### 1.2 Array itself is an object

In Java：

```text
array itself is an object and must be instantiated.
```

This is why declaration and creation are separate ideas：

```java
int[] scores;          // declaration
scores = new int[10];  // instantiation
```

---

## 2. Declaring and Using Arrays

### 2.1 Declaration

Both are legal：

```java
int[] scores;
int scores[];
```

Course style usually prefers：

```java
int[] scores;
```

因为 `int[]` 更清楚地表示 type is array of int。

### 2.2 Instantiation

Correct：

```java
scores = new int[10];
```

Wrong in Java：

```java
int scores[10]; // not Java declaration syntax
```

Declare and instantiate together：

```java
float[] prices = new float[500];
char[] codes = new char[1750];
```

### 2.3 Initializer list

可以直接给 initial values：

```java
char[] vowels = {'A', 'E', 'I', 'O', 'U'};
```

This creates an array of length `5`：

```java
vowels.length // 5
```

Valid indexes：

```text
0, 1, 2, 3, 4
```

### 2.4 Using index

Array element 可以像普通 variable 一样使用：

```java
scores[2] = 100;
int x = scores[2] + 5;
System.out.println(scores[2]);
```

Example：

```java
int[] list = new int[15];

for (int index = 0; index < list.length; index++) {
    list[index] = index * 10;
}

list[5] = 999;
```

Output：

```text
0  10  20  30  40  999  60  70  80  90  100  110  120  130  140
```

---

## 3. Looping Through Arrays

### 3.1 Normal `for` loop

Use when you need index：

```java
for (int i = 0; i < scores.length; i++) {
    System.out.println(scores[i]);
}
```

This is best when：

- need to modify `scores[i]`。
- need reverse order。
- need compare neighboring elements。
- need index value itself。

### 3.2 Enhanced `for` loop

Iterator version：

```java
for (int score : scores) {
    System.out.println(score);
}
```

This is appropriate when：

```text
processing all array elements from lowest index to highest index
```

It is concise for reading values。

### 3.3 Enhanced for limitation

This does not change original array values：

```java
for (int item : items) {
    item = item * 2;
}
```

Reason：

- `item` is a copy of each element value for primitive array。
- assigning `item` does not assign `items[index]`。

Correct way to double each array element：

```java
for (int index = 0; index < items.length; index++) {
    items[index] += items[index];
}
```

---

## 4. Bounds Checking and `length`

### 4.1 Fixed size

Once an array is created, it has a fixed size。

```java
int[] scores = new int[10];
```

This array always has 10 elements。

You cannot append an 11th element directly。

### 4.2 Valid indexes

For:

```java
int[] scores = new int[10];
```

Valid：

```java
scores[0]
scores[1]
...
scores[9]
```

Invalid：

```java
scores[10]
scores[-1]
```

Invalid access throws：

```text
ArrayIndexOutOfBoundsException
```

### 4.3 `length`

Every array object has a public constant：

```java
scores.length
```

Important distinction：

```text
length = number of elements
largest valid index = length - 1
```

Example：

```java
double[] numbers = new double[10];
System.out.println(numbers.length); // 10
```

Reverse order loop：

```java
for (int index = numbers.length - 1; index >= 0; index--) {
    System.out.print(numbers[index] + "  ");
}
```

---

## 5. Arrays of Objects

### 5.1 Object array declaration

Example：

```java
String[] words = new String[5];
```

This creates an array that can hold 5 `String` references。

It does not create 5 `String` objects。

Initial state：

```text
words[0] -> null
words[1] -> null
words[2] -> null
words[3] -> null
words[4] -> null
```

### 5.2 Null reference

This prints `null`：

```java
System.out.println(words[0]);
```

But this would cause `NullPointerException`：

```java
System.out.println(words[0].length());
```

Reason：

```text
words[0] does not refer to a String object yet.
```

### 5.3 Instantiate each object separately

```java
words[0] = new String("friendship");
words[1] = new String("loyalty");
words[2] = new String("honor");
```

Now:

```text
words[0] -> "friendship"
words[1] -> "loyalty"
words[2] -> "honor"
words[3] -> null
words[4] -> null
```

Exam sentence：

```text
An array of objects stores object references. Each object stored in the array must be instantiated separately.
```

---

## 6. Two-Dimensional Arrays

In Java, a two-dimensional array is an array of arrays。

Declaration：

```java
int[][] scores = new int[12][50];
```

Meaning：

- `scores` has 12 rows。
- each row has 50 columns。
- first index selects row。
- second index selects element inside that row。

Access：

```java
int value = scores[3][6];
```

This means：

```text
row index 3, column index 6
```

The array stored in one row can be specified using one index：

```java
scores[0]
```

means first row。

### 6.1 Common loop pattern

```java
for (int row = 0; row < scores.length; row++) {
    for (int col = 0; col < scores[row].length; col++) {
        System.out.println(scores[row][col]);
    }
}
```

Why `scores[row].length`？

Because in Java, each row is itself an array。

---

## 7. Key Examples from Lecture

### 7.1 `BasicArray`

Core code：

```java
final int LIMIT = 15, MULTIPLE = 10;
int[] list = new int[LIMIT];

for (int index = 0; index < LIMIT; index++) {
    list[index] = index * MULTIPLE;
}

list[5] = 999;

for (int value : list) {
    System.out.print(value + "  ");
}
```

Output：

```text
0  10  20  30  40  999  60  70  80  90  100  110  120  130  140
```

Trace：

- indexes `0` to `14` are filled with `index * 10`。
- `list[5]` originally `50`。
- then changed to `999`。

### 7.2 Quick Check: ages

Write an array declaration to represent ages of 100 children：

```java
int[] ages = new int[100];
```

Print each value in an array named `values`：

```java
for (int value : values) {
    System.out.println(value);
}
```

### 7.3 `ReverseOrder`

Key idea：

1. read 10 numbers into array。
2. print from `length - 1` down to `0`。

Core code：

```java
double[] numbers = new double[10];

for (int index = 0; index < numbers.length; index++) {
    numbers[index] = scan.nextDouble();
}

for (int index = numbers.length - 1; index >= 0; index--) {
    System.out.print(numbers[index] + "  ");
}
```

Most important line：

```java
int index = numbers.length - 1
```

because last valid index is `length - 1`。

### 7.4 `LetterCount`

Purpose：

```text
count uppercase and lowercase letters in a sentence
```

Arrays：

```java
final int NUMCHARS = 26;
int[] upper = new int[NUMCHARS];
int[] lower = new int[NUMCHARS];
int other = 0;
```

Core logic：

```java
for (int ch = 0; ch < line.length(); ch++) {
    current = line.charAt(ch);

    if (current >= 'A' && current <= 'Z') {
        upper[current - 'A']++;
    } else if (current >= 'a' && current <= 'z') {
        lower[current - 'a']++;
    } else {
        other++;
    }
}
```

Key trick：

```java
current - 'A'
current - 'a'
```

Because characters have numeric Unicode values。

Examples：

| Character | Expression | Index |
|---|---|---:|
| `'A'` | `'A' - 'A'` | `0` |
| `'B'` | `'B' - 'A'` | `1` |
| `'Z'` | `'Z' - 'A'` | `25` |
| `'a'` | `'a' - 'a'` | `0` |
| `'z'` | `'z' - 'a'` | `25` |

Printing back:

```java
System.out.print((char) (letter + 'A'));
System.out.print((char) (letter + 'a'));
```

---

## 8. Dynamic Capacity Example: `DVDCollection`

### 8.1 Big idea

Array size is fixed, but we can simulate growth by：

1. creating a larger array。
2. copying old references into new array。
3. making the instance variable refer to new array。

### 8.2 Fields

```java
private DVD[] collection;
private int count;
private double totalCost;
```

Meanings：

| Field | Meaning |
|---|---|
| `collection` | array storing `DVD` references |
| `count` | number of actual DVDs stored |
| `totalCost` | total cost of all DVDs |

Important：

```text
collection.length is capacity.
count is actual number of used elements.
```

They are not the same。

### 8.3 Constructor

```java
public DVDCollection() {
    collection = new DVD[100];
    count = 0;
    totalCost = 0.0;
}
```

This creates capacity for 100 references, but initially stores 0 DVDs。

### 8.4 `addDVD`

```java
public void addDVD(String title, String director, int year,
                   double cost, boolean bluRay) {
    if (count == collection.length) {
        increaseSize();
    }

    collection[count] = new DVD(title, director, year, cost, bluRay);
    totalCost += cost;
    count++;
}
```

Trace：

1. If array is full, grow it。
2. Put new DVD at index `count`。
3. Add cost to total。
4. Increment `count`。

Why store at `collection[count]`？

- if `count == 0`, next free slot is index 0。
- if `count == 5`, used indexes are `0` to `4`; next free slot is index 5。

### 8.5 `increaseSize`

```java
private void increaseSize() {
    DVD[] temp = new DVD[collection.length * 2];

    for (int dvd = 0; dvd < collection.length; dvd++) {
        temp[dvd] = collection[dvd];
    }

    collection = temp;
}
```

Important：

- It copies references, not deep copies of DVD objects。
- Old array becomes unreachable if no other reference points to it。
- New capacity is double。

### 8.6 `toString`

The report loops only through actual stored elements：

```java
for (int dvd = 0; dvd < count; dvd++) {
    report += collection[dvd].toString() + "\n";
}
```

Why not `dvd < collection.length`？

Because many slots after `count - 1` are unused `null` references。

---

## 9. In-Class Exercises

### 9.1 Double every array element

Question：

```text
Write code that sets each element of an array called items to twice the value in that array element.
```

Answer：

```java
for (int index = 0; index < items.length; index++) {
    items[index] += items[index];
}
```

Equivalent：

```java
items[index] = items[index] * 2;
```

### 9.2 `averageArray`

Question：

```text
Write a method called averageArray that accepts an array of integers and returns the average.
```

Answer：

```java
public float averageArray(int[] values) {
    float sum = 0;

    for (int index = 0; index < values.length; index++) {
        sum += values[index];
    }

    return sum / values.length;
}
```

Exam note：

- `values` is an array parameter。
- use `values.length`。
- `sum` is `float` so average can be fractional。

Potential edge case：

```text
If values.length is 0, this code divides by zero. The lecture answer assumes non-empty array.
```

---

## 10. The `ArrayList` Class

### 10.1 What is `ArrayList`?

`ArrayList` is part of the `java.util` package。

```java
import java.util.ArrayList;
```

It is similar to array because：

- stores a list of values/references。
- each element can be accessed by numeric index。

It is different from array because：

- it grows and shrinks as needed。
- inserting/removing shifts elements automatically。

### 10.2 Declaration and instantiation

With generics：

```java
ArrayList<Integer> intList;
intList = new ArrayList<Integer>();
```

or together：

```java
ArrayList<Integer> intList = new ArrayList<Integer>();
```

Raw type shown in lecture：

```java
ArrayList objList;
```

But generic type is preferred：

```java
ArrayList<String> names = new ArrayList<String>();
```

### 10.3 Common methods

| Method | Meaning |
|---|---|
| `add(element)` | add at end |
| `add(position, element)` | insert at given index |
| `remove(pos)` | remove element at index; returns removed element |
| `remove(element)` | remove first occurrence using `equals`; returns boolean |
| `get(pos)` | retrieve element at index |
| `size()` | number of elements |
| `indexOf(object)` | first occurrence index, or `-1` if absent |

### 10.4 Insert/remove shifts indexes

Example：

```java
ArrayList<Integer> intList = new ArrayList<Integer>();

for (int i = 0; i < 5; i++) {
    intList.add(i * 5);
}
```

Now：

```text
[0, 5, 10, 15, 20]
```

Remove index 2：

```java
intList.remove(2);
```

Now：

```text
[0, 5, 15, 20]
```

The element `10` is removed, and elements after it shift left。

Find index：

```java
int location = intList.indexOf(15); // 2
```

Insert at location：

```java
intList.add(location, 100);
```

Now：

```text
[0, 5, 100, 15, 20]
```

### 10.5 `ArrayList<Integer>` and wrapper classes

`ArrayList` stores object references。

It cannot directly store primitive `int` as raw primitive values。

So we use：

```java
ArrayList<Integer>
```

`Integer` is the wrapper class for primitive `int`。

This is covered in Lecture 12。

---

## 11. Array vs ArrayList

| Feature | Array | `ArrayList` |
|---|---|---|
| Size | fixed after creation | grows/shrinks |
| Syntax access | `arr[i]` | `list.get(i)` |
| Number of elements | `arr.length` | `list.size()` |
| Element type | primitive or reference | object references |
| Add/remove | manual shifting/copying | built-in `add` / `remove` |
| 2D simple syntax | `int[][]` | possible but more complex |
| Import needed | no | `java.util.ArrayList` |

Exam quick distinction：

```text
array.length is a field/constant, no parentheses.
ArrayList.size() is a method, needs parentheses.
```

---

## 12. 考试答题模板

### 12.1 问：array 越界会怎样？

```text
Java performs automatic bounds checking. If an index is outside 0 to length - 1, the interpreter throws ArrayIndexOutOfBoundsException.
```

### 12.2 问：`String[] words = new String[5];` 创建了什么？

```text
It creates an array object that stores five String references. It does not create five String objects. Initially all five references are null.
```

### 12.3 问：array 如何扩容？

```text
An array itself cannot change size. To increase capacity, create a larger array, copy the existing elements/references into it, and assign the array reference to the new array.
```

### 12.4 问：ArrayList 插入时发生什么？

```text
When an element is inserted into an ArrayList, later elements move aside and their indexes increase. When an element is removed, later elements collapse left and their indexes decrease.
```

---

## 13. 最容易错的点

1. `array.length` 没有 parentheses；`ArrayList.size()` 有 parentheses。
2. last valid array index 是 `length - 1`。
3. `int scores[10];` 不是 Java array declaration。
4. object array 创建的是 references，不是 objects。
5. object array 初始元素是 `null`。
6. `count` 和 `collection.length` 不一样：`count` 是实际数量，`length` 是 capacity。
7. enhanced for loop 适合读取，不适合需要 index 的修改。
8. `ArrayList.remove(2)` 是 remove index 2；`remove(new Integer(2))` 才是 remove object value 2。
9. `indexOf` 找不到返回 `-1`。
10. 2D array 在 Java 是 array of arrays。

---

## 14. 最后 30 秒记忆版

```text
Array:
int[] a = new int[10];
valid index: 0 to a.length - 1
a.length is number of elements
out of bounds -> ArrayIndexOutOfBoundsException

Object array:
String[] words = new String[5];
creates 5 null references, not 5 String objects

ArrayList:
import java.util.ArrayList;
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(x), list.add(i, x), list.remove(i), list.get(i), list.size(), list.indexOf(x)
```
