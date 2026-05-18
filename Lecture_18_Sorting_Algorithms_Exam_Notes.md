# Lecture 18: Sorting Algorithms - Exam Notes

> 范围：`Lecture/18_Sorting.pdf`，主要是 Bubble Sort、Insertion Sort、Selection Sort、running time / Big-O、Merge Sort、Quick Sort。
> 读法：先看第 0 节速查，再重点看第 2-4 节三个 simple sorts、第 6 节 Merge Sort、第 7 节 Quick Sort。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Sorting 必背

| Algorithm | 核心思想 | Best | Average | Worst | Extra space | 稳定性 |
|---|---|---:|---:|---:|---:|---|
| `Bubble Sort` | adjacent compare + swap，每 pass 放好一个最小/最大 | `O(n^2)` in lecture version | `O(n^2)` | `O(n^2)` | `O(1)` | stable if swap only when `>` |
| `Insertion Sort` | 维护前面 sorted region，把 key 插入正确位置 | `O(n)` if optimized and already sorted | `O(n^2)` | `O(n^2)` | `O(1)` | stable |
| `Selection Sort` | 每 pass 找剩余最小值，swap 到当前位置 | `O(n^2)` | `O(n^2)` | `O(n^2)` | `O(1)` | usually not stable |
| `Merge Sort` | divide into halves, sort halves, merge | `O(n log n)` | `O(n log n)` | `O(n log n)` | `O(n)` | stable if merge carefully |
| `Quick Sort` | pick pivot, partition, recursively sort sublists | `O(n log n)` | often `O(n log n)` | `O(n^2)` | `O(log n)` recursion average | not necessarily stable |

课程 slides 明确给出的重点：

```text
Bubble / Insertion simple sorts: O(n^2)
Merge Sort: O(n log n)
Quick Sort worst case: O(n^2)
Quick Sort best case: O(n log n)
Partition step: O(N)
```

### Big-O 必背

| 表达式 | Tightest Big-O |
|---|---|
| `3n^3 + 90n^2 - 2n + 5` | `O(n^3)` |
| `2n^2` | `O(n^2)` |
| `n + log n` | `O(n)` |
| `n log n + n` | `O(n log n)` |

Big-O 规则：

```text
Ignore constants.
Ignore lower-order terms.
Use the tightest useful bound.
```

---

## 1. What is Sorting?

`Sorting` 是：

```text
arrange a sequence of items in a particular order
```

本 lecture 默认：

```text
ascending order
```

常见 sorting criteria：

- numbers ascending。
- strings alphabetical。
- objects by last name / score / date。

本讲把 sorting algorithms 分两类：

1. Simple sorting algorithms：
   - Bubble Sort
   - Insertion Sort
   - Selection Sort
2. Recursive sorting algorithms：
   - Merge Sort
   - Quick Sort

---

## 2. Bubble Sort

### 2.1 核心思想

Lecture 里的 Bubble Sort 从 bottom to top 比较 adjacent elements。

第一 pass：

```text
compare adjacent elements from bottom to top
swap if necessary
put the smallest element in the right position
```

第二 pass：

```text
put the 2nd smallest element in the right position
```

不断重复，直到只需要检查 bottom two items。

如果用 array index 理解 slides 的版本：

- `i = 1` 时，目标是把最小值放到 index `0`。
- inner loop 从 `list.length - 1` 往 `i` 扫。
- 比较 `list[j-1]` 和 `list[j]`。
- 如果左边大于右边，就 swap。

### 2.2 直观例子

初始：

```text
2 4 6 1 3 5
```

从 bottom/right side 往 top/left side 比较：

```text
compare 3 and 5 -> ok
compare 1 and 3 -> ok
compare 6 and 1 -> swap
2 4 1 6 3 5

compare 4 and 1 -> swap
2 1 4 6 3 5

compare 2 and 1 -> swap
1 2 4 6 3 5
```

第一 pass 后：

```text
1 is in the right position.
```

### 2.3 Java template

```java
public static void bubbleSort(int[] list) {
    for (int i = 1; i < list.length; i++) {
        for (int j = list.length - 1; j >= i; j--) {
            if (list[j - 1] > list[j]) {
                int temp = list[j - 1];
                list[j - 1] = list[j];
                list[j] = temp;
            }
        }
    }
}
```

### 2.4 Bubble Sort 复杂度

比较次数大约：

```text
(n - 1) + (n - 2) + ... + 1 = n(n - 1)/2 = O(n^2)
```

所以：

```text
Bubble Sort = O(n^2)
```

---

## 3. Insertion Sort

### 3.1 核心思想

`Insertion Sort` 维护一个 sorted region。

第 `i` pass 后：

```text
first i elements are sorted
```

流程：

1. 假设第一个 element 已经 sorted。
2. 取下一个 unsorted element 作为 `key`。
3. 在 sorted region 中从右往左找正确位置。
4. 比 `key` 大的 elements 往右 shift。
5. 把 `key` 放到空出来的位置。

### 3.2 直观例子

初始：

```text
2 4 6 1 3 5
```

前三个已经局部 sorted：

```text
[2 4 6] 1 3 5
```

插入 `1`：

```text
key = 1
6 > 1 -> shift 6 right
4 > 1 -> shift 4 right
2 > 1 -> shift 2 right
put 1 at front
```

结果：

```text
[1 2 4 6] 3 5
```

再插入 `3`：

```text
6 > 3 -> shift
4 > 3 -> shift
2 > 3 -> stop
put 3 after 2
```

结果：

```text
[1 2 3 4 6] 5
```

### 3.3 Java template

```java
public static void insertionSort(int[] list) {
    for (int i = 1; i < list.length; i++) {
        int key = list[i];
        int j = i;

        while (j > 0 && list[j - 1] > key) {
            list[j] = list[j - 1];
            j--;
        }

        list[j] = key;
    }
}
```

### 3.4 Insertion Sort 复杂度

Worst case：

```text
array is reverse sorted
```

每个 `key` 都要往前移动很多格：

```text
1 + 2 + ... + (n - 1) = O(n^2)
```

Best case：

```text
array already sorted
```

每次 while condition 很快 fail。优化版是 `O(n)`。

Lecture 中 simple sorts 重点记：

```text
Insertion Sort = O(n^2)
```

---

## 4. Selection Sort

### 4.1 核心思想

第 `i` pass：

```text
put the ith smallest element in the ith position
```

流程：

1. 从 unsorted region 中找 minimum。
2. 把 minimum 和 unsorted region 的第一个位置 swap。
3. sorted region 增加一个。

### 4.2 直观例子

初始：

```text
2 4 6 1 3 5
```

Pass 1：

```text
minimum = 1
swap with first element 2
1 4 6 2 3 5
```

Pass 2：

```text
unsorted = 4 6 2 3 5
minimum = 2
swap with 4
1 2 6 4 3 5
```

Pass 3：

```text
unsorted = 6 4 3 5
minimum = 3
swap with 6
1 2 3 4 6 5
```

### 4.3 Java template

```java
public static void selectionSort(int[] list) {
    for (int i = 0; i < list.length - 1; i++) {
        int minimum = i;

        for (int j = i + 1; j < list.length; j++) {
            if (list[j] < list[minimum]) {
                minimum = j;
            }
        }

        int temp = list[i];
        list[i] = list[minimum];
        list[minimum] = temp;
    }
}
```

### 4.4 Selection Sort 复杂度

不管 array 是否已经 sorted，仍然要找 minimum：

```text
(n - 1) + (n - 2) + ... + 1 = O(n^2)
```

所以：

```text
Selection Sort = O(n^2)
```

优点：

```text
number of swaps is small: at most n - 1
```

---

## 5. Running Time and Big-O

### 5.1 Running time analysis

Running time 问：

```text
How many operations does the algorithm take in terms of input size n?
```

三种情况：

| Case | Meaning |
|---|---|
| best case | minimum time over all inputs of size `n` |
| average case | average time over all inputs of size `n` |
| worst case | maximum time over all inputs of size `n` |

通常最常用：

```text
worst case analysis
```

因为它提供 guarantee。

### 5.2 Asymptotic analysis

`Asymptotic analysis` 看：

```text
growth of T(n) as n -> infinity
```

忽略：

- machine-dependent constants。
- leading constants。
- lower-order terms。

例子：

```text
3n^3 + 90n^2 - 2n + 5 = O(n^3)
```

因为当 `n` 很大时，`n^3` 主导增长。

### 5.3 Big-O definition

课件定义：

```text
f(n) = O(g(n))
if there exist constants c and n0 such that
0 <= f(n) <= c * g(n) for all n >= n0
```

直觉：

```text
Eventually, f(n) is bounded above by a constant multiple of g(n).
```

### 5.4 Tightest bound

课件例子：

```text
2n^2 = O(n^2)
2n^2 = O(n^3)
2n^2 = O(n^4)
```

都 technically true。

但通常：

```text
use the tightest one
```

所以写：

```text
2n^2 = O(n^2)
```

不能写：

```text
2n^2 = O(n)
```

因为 `n` 不可能 upper bound `n^2` 的增长。

---

## 6. Merge Sort

### 6.1 Divide-and-conquer

`Merge Sort` 是 divide-and-conquer：

1. Divide：把 list 分成 two sub-lists。
2. Conquer：分别 sort 两个 sub-lists。
3. Combine：把两个 sorted sub-lists merge 成一个 sorted list。

核心思想：

```text
Sorting the whole list is hard.
Sorting smaller lists and merging them is easier.
```

### 6.2 例子

初始：

```text
5 7 3 1 6 4 2 8
```

Divide：

```text
5 7 3 1 | 6 4 2 8
5 7 | 3 1 | 6 4 | 2 8
5 | 7 | 3 | 1 | 6 | 4 | 2 | 8
```

Merge sorted pairs：

```text
5 7
1 3
4 6
2 8
```

Merge sorted halves：

```text
1 3 5 7
2 4 6 8
```

Final merge：

```text
1 2 3 4 5 6 7 8
```

### 6.3 Merge step 怎么做？

给两个 sorted lists：

```text
A = 1 3 5 7
B = 2 4 6 8
```

用两个 pointers：

```text
iFirst = 0
iSecond = 0
```

每次比较 A 当前值和 B 当前值，较小者写入 result。

```text
compare 1 and 2 -> take 1
compare 3 and 2 -> take 2
compare 3 and 4 -> take 3
...
```

### 6.4 Merge Sort template

```java
public static void mergeSort(int[] list) {
    if (list.length <= 1) {
        return;
    }

    int[] first = new int[list.length / 2];
    int[] second = new int[list.length - first.length];

    for (int i = 0; i < first.length; i++) {
        first[i] = list[i];
    }

    for (int i = 0; i < second.length; i++) {
        second[i] = list[first.length + i];
    }

    mergeSort(first);
    mergeSort(second);

    merge(first, second, list);
}
```

Merge helper：

```java
private static void merge(int[] first, int[] second, int[] result) {
    int iFirst = 0;
    int iSecond = 0;
    int i = 0;

    while (iFirst < first.length && iSecond < second.length) {
        if (first[iFirst] <= second[iSecond]) {
            result[i++] = first[iFirst++];
        } else {
            result[i++] = second[iSecond++];
        }
    }

    while (iFirst < first.length) {
        result[i++] = first[iFirst++];
    }

    while (iSecond < second.length) {
        result[i++] = second[iSecond++];
    }
}
```

### 6.5 Merge Sort analysis

课件推导：

```text
T(1) = c
T(n) = 2T(n/2) + kn
```

解释：

- `2T(n/2)`：sort two halves。
- `kn`：merge two sorted halves takes linear time。

展开：

```text
T(n) = kn + 2T(n/2)
T(n) = kn + 2[kn/2 + 2T(n/4)]
T(n) = 2kn + 4T(n/4)
T(n) = i kn + 2^i T(n / 2^i)
```

停止条件：

```text
n / 2^i = 1
i = log2 n
```

代回：

```text
T(n) = kn log n + cn = O(n log n)
```

所以：

```text
Merge Sort = O(n log n)
```

---

## 7. Quick Sort

### 7.1 核心思想

`Quick Sort` 也是 divide-and-conquer。

流程：

1. Pick an element as `pivot`。
2. Partition list into:
   - elements `<= pivot`
   - elements `> pivot`
3. Put pivot in the right position。
4. Recursively sort the two sublists。

### 7.2 Partition 例子

初始：

```text
5 7 1 3 4 6 2 8
```

选：

```text
pivot = 5
```

Partition 后：

```text
4 2 1 3 5 6 7 8
```

满足：

```text
left of pivot <= 5
right of pivot > 5
```

注意：

```text
Partition does not fully sort the left and right sides.
It only puts pivot in final correct position.
```

之后分别 sort：

```text
4 2 1 3
6 7 8
```

### 7.3 Two-pointer partition 直觉

课件描述：

1. Scan from left to right。
2. Stop when number `> pivot`。
3. Scan from right to left。
4. Stop when number `<= pivot`。
5. 如果 left pointer 还在 right pointer 左边，就 swap。
6. 当 pointers cross，put pivot in correct position。

对于：

```text
5 7 1 3 4 6 2 8
```

left 找到 `7`，因为 `7 > 5`。

right 找到 `2`，因为 `2 <= 5`。

swap：

```text
5 2 1 3 4 6 7 8
```

继续直到 pivot 放好。

Partition time：

```text
O(N)
```

因为每个 pointer 总共扫过 array 一次左右。

### 7.4 Quick Sort template

```java
private static void qsort(int[] list, int i, int j) {
    if (i >= j) {
        return;
    }

    if (i + 1 == j) {
        if (list[i] > list[j]) {
            swap(list, i, j);
        }
        return;
    }

    int pos = findPivot(list, i, j);

    if (pos == i) {
        qsort(list, i + 1, j);
    } else if (pos == j) {
        qsort(list, i, j - 1);
    } else {
        qsort(list, i, pos - 1);
        qsort(list, pos + 1, j);
    }
}
```

Partition skeleton：

```java
private static int findPivot(int[] list, int i, int j) {
    int pivot = list[i];
    int left = i + 1;
    int right = j;

    while (left < right) {
        while (left <= j && list[left] <= pivot) {
            left++;
        }

        while (right > i && list[right] > pivot) {
            right--;
        }

        if (left < right) {
            swap(list, left, right);
            left++;
            right--;
        }
    }

    // put pivot into its final position
    swap(list, i, right);
    return right;
}
```

不同 implementation 的 boundary details 可能不同；考试重点是 partition concept。

### 7.5 Quick Sort analysis

Best case：

```text
pivot partitions the array evenly
```

每层总 work：

```text
O(N)
```

层数：

```text
O(log N)
```

所以：

```text
O(N log N)
```

Worst case：

```text
pivot is always on the boundary
```

每次只排除一个 pivot：

```text
N + (N - 1) + (N - 2) + ... + 1 = O(N^2)
```

所以：

```text
Quick Sort worst case = O(N^2)
Quick Sort best case = O(N log N)
```

### 7.6 In-place sorting

课件提到：

```text
In-place sorting: sorting occurs within the original array, no new array is needed.
```

Quick Sort 的 partition 通常是 in-place：

- 不创建新的 left/right arrays。
- 通过 swaps 在原 array 内调整。

Merge Sort 常见 version 需要 extra arrays，所以不是 strictly in-place。

---

## 8. Algorithm Comparison

### 8.1 Simple sorts 对比

| Algorithm | 每一轮保证 | 适合记忆 |
|---|---|---|
| Bubble Sort | 一个最小/最大元素被 bubble 到正确位置 | adjacent swap |
| Insertion Sort | 前面 sorted region 增长 | 像整理扑克牌 |
| Selection Sort | 找到剩余 minimum 放到当前位置 | select minimum then swap |

### 8.2 Recursive sorts 对比

| Algorithm | Divide | Combine / Partition |
|---|---|---|
| Merge Sort | split into halves | merge two sorted halves |
| Quick Sort | partition by pivot | pivot placed in final position, then recurse |

### 8.3 考试优先级

如果题目让手算：

1. 明确当前 pass/round。
2. 标出 sorted region 或 pivot。
3. 每次 swap/shift 写清楚。
4. 最后检查 ascending order。

如果题目问 complexity：

1. Simple sorts：`O(n^2)`。
2. Merge Sort：`O(n log n)`。
3. Quick Sort：best `O(n log n)`，worst `O(n^2)`。
4. Partition：`O(n)`。

---

## 9. 考试答题模板

### 9.1 Bubble Sort trace

```text
Pass 1:
compare adjacent elements from bottom to top.
swap if left > right.
After pass 1, the smallest element is at index 0.

Pass 2:
repeat from bottom to index 1.
After pass 2, the second smallest is at index 1.
```

### 9.2 Insertion Sort trace

```text
At pass i:
key = list[i].
The region list[0..i-1] is already sorted.
Shift elements larger than key one position right.
Insert key into the empty position.
```

### 9.3 Selection Sort trace

```text
At pass i:
find minimum in list[i..n-1].
swap it with list[i].
Now list[0..i] is sorted.
```

### 9.4 Merge Sort explanation

```text
Split the list into two halves.
Recursively sort each half.
Merge the two sorted halves by repeatedly taking the smaller front element.
T(n) = 2T(n/2) + O(n) = O(n log n).
```

### 9.5 Quick Sort explanation

```text
Choose pivot.
Partition array so left side <= pivot and right side > pivot.
Put pivot in final position.
Recursively quicksort left and right subarrays.
Partition is O(n).
Best case O(n log n), worst case O(n^2).
```

---

## 10. 最容易错的点

### 10.1 Bubble Sort 方向

Lecture 版本是：

```text
start comparison from bottom to top
smallest element moves to the front/left
```

很多教材从 front to back，把 largest bubble 到 end。

两种都叫 Bubble Sort，但考试 trace 要跟题目/课件方向一致。

### 10.2 Insertion Sort 是 shift，不是一直 swap

课件 code：

```java
while (j > 0 && list[j - 1] > key) {
    list[j] = list[j - 1];
    j--;
}
list[j] = key;
```

它是把大元素右移，最后放 `key`。

### 10.3 Selection Sort 每轮找 minimum

不要看到前两个顺序不对就 swap。

Selection Sort 是先扫描完整 unsorted region 找 minimum，再 swap 一次。

### 10.4 Big-O 要 tightest

```text
2n^2 = O(n^3)
```

technically true，但考试通常要：

```text
O(n^2)
```

### 10.5 Merge Sort 的 merge 要两个 halves 都 sorted

不能直接把 unsorted halves 合并。

流程必须是：

```text
sort first half
sort second half
then merge
```

### 10.6 Quick Sort partition 不等于完全排序

Partition 后只保证：

```text
left <= pivot < right
```

left side 内部不一定 sorted。

right side 内部也不一定 sorted。

还需要 recursive calls。

---

## 11. 最后 30 秒记忆版

```text
Sorting = arrange items in order; lecture assumes ascending.

Bubble Sort:
compare adjacent, swap, each pass fixes one smallest element.
O(n^2).

Insertion Sort:
maintain sorted prefix, insert key into correct position by shifting.
O(n^2), best optimized case O(n).

Selection Sort:
find minimum in unsorted region, swap to current position.
O(n^2).

Big-O:
ignore constants and lower terms.
use tightest useful bound.
3n^3 + 90n^2 - 2n + 5 = O(n^3).

Merge Sort:
divide into halves, sort halves, merge.
T(n) = 2T(n/2) + O(n) = O(n log n).

Quick Sort:
choose pivot, partition, recursively sort left/right.
partition = O(n).
best = O(n log n), worst = O(n^2).

In-place sorting means sorting inside original array without new array.
```
