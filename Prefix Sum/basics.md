# Prefix Sum — Complete Beginner to Intermediate Guide

> **Prerequisites:** Arrays, Loops, Basic Arithmetic
>
> **Difficulty:** Beginner → Intermediate
>
> **Time Required:** 2–3 Days
>
> **Interview Importance:** ⭐⭐⭐⭐⭐

---

# Table of Contents

- [1. What is Prefix Sum?](#1-what-is-prefix-sum)
- [2. Why Do We Need Prefix Sum?](#2-why-do-we-need-prefix-sum)
- [3. Prerequisites](#3-prerequisites)
- [4. Understanding the Core Idea](#4-understanding-the-core-idea)
- [5. Building a Prefix Sum Array](#5-building-a-prefix-sum-array)
- [6. Range Sum Queries](#6-range-sum-queries)
- [7. Formula](#7-formula)
- [8. Python Implementation](#8-python-implementation)
- [9. Dry Run](#9-dry-run)
- [10. Time Complexity](#10-time-complexity)
- [11. When to Use Prefix Sum](#11-when-should-you-think-of-prefix-sum)
- [12. Common Mistakes](#12-common-mistakes)
- [13. Prefix Sum Variations](#13-prefix-sum-variations)
- [14. Learning Roadmap](#14-learning-roadmap)
- [15. Practice Problems](#15-practice-problems)

---

# 1. What is Prefix Sum?

A **Prefix Sum** is a technique where we precompute cumulative sums of an array so that we can answer range sum queries efficiently.

Instead of calculating the sum every time, we calculate it once and reuse it.

---

## Example

Original Array

```
3 2 5 1 4
```

Prefix Sum Array

```
3 5 10 11 15
```

Notice

```
3
3+2 = 5
3+2+5 = 10
3+2+5+1 = 11
3+2+5+1+4 = 15
```

Every position stores the sum from index **0** to **i**.

---

# 2. Why Do We Need Prefix Sum?

Suppose the array is

```
3 2 5 1 4
```

Now imagine someone asks

```
Sum from index 1 to 3
```

You compute

```
2+5+1 = 8
```

Now they ask

```
Sum from 0 to 4

Sum from 2 to 4

Sum from 1 to 2

Sum from 3 to 4
```

If there are **10,000** such queries, repeatedly looping over the array becomes slow.

Prefix Sum solves this problem.

---

# 3. Prerequisites

Before learning Prefix Sum, you should know:

## Arrays

```python
nums = [3,2,5,1,4]
```

---

## Loops

```python
for i in range(len(nums)):
    print(nums[i])
```

or

```python
for num in nums:
    print(num)
```

---

## Basic Addition

```
3+2 = 5

5+5 =10

10+1=11

11+4=15
```

That's all Prefix Sum does.

---

# 4. Understanding the Core Idea

Instead of repeatedly finding sums,

We calculate cumulative sums once.

Think of it as storing progress.

Example

```
Array

3 2 5 1 4
```

Build

```
3

3+2

3+2+5

3+2+5+1

3+2+5+1+4
```

Result

```
3 5 10 11 15
```

This new array is called the Prefix Sum Array.

---

# 5. Building a Prefix Sum Array

Given

```
Array

3 2 5 1 4
```

Step 1

```
prefix[0]=3
```

Current

```
3 _ _ _ _
```

---

Step 2

```
prefix[1]=3+2=5
```

```
3 5 _ _ _
```

---

Step 3

```
5+5=10
```

```
3 5 10 _ _
```

---

Step 4

```
10+1=11
```

```
3 5 10 11 _
```

---

Step 5

```
11+4=15
```

Final

```
3 5 10 11 15
```

---

# 6. Range Sum Queries

Suppose

```
Array

3 2 5 1 4
```

Find

```
Sum from index 2 to 4
```

Normal method

```
5+1+4=10
```

Need to loop.

---

Using Prefix Sum

```
Prefix

3 5 10 11 15
```

Take

```
prefix[4]=15
```

Subtract

```
prefix[1]=5
```

Answer

```
15-5=10
```

No loop required.

---

# 7. Formula

For any range

```
L → R
```

The sum is

```
prefix[R] - prefix[L-1]
```

Special case

If

```
L=0
```

then

```
sum = prefix[R]
```

because there is nothing before index 0.

---

## Example

Array

```
4 7 2 9 5
```

Prefix

```
4 11 13 22 27
```

Find

```
sum(2,4)
```

Calculation

```
27-11=16
```

Verification

```
2+9+5=16
```

Correct.

---

# 8. Python Implementation

```python
nums = [3,2,5,1,4]

prefix = [0] * len(nums)

prefix[0] = nums[0]

for i in range(1, len(nums)):
    prefix[i] = prefix[i-1] + nums[i]

print(prefix)
```

Output

```python
[3, 5, 10, 11, 15]
```

---

## Query Function

```python
def range_sum(prefix, left, right):
    if left == 0:
        return prefix[right]
    return prefix[right] - prefix[left-1]
```

Example

```python
nums = [3,2,5,1,4]

prefix = [3,5,10,11,15]

print(range_sum(prefix,2,4))
```

Output

```
10
```

---

# 9. Dry Run

Given

```
nums

3 2 5 1
```

Initially

```
prefix

0 0 0 0
```

---

Iteration 1

```
prefix[0]=3
```

```
3 0 0 0
```

---

Iteration 2

```
3+2=5
```

```
3 5 0 0
```

---

Iteration 3

```
5+5=10
```

```
3 5 10 0
```

---

Iteration 4

```
10+1=11
```

```
3 5 10 11
```

Finished.

---

# 10. Time Complexity

## Without Prefix Sum

Each query

```
O(n)
```

If

```
q
```

queries exist

Overall

```
O(n × q)
```

---

## With Prefix Sum

Building

```
O(n)
```

Each query

```
O(1)
```

Overall

```
O(n + q)
```

Huge improvement.

---

# 11. When Should You Think of Prefix Sum?

Whenever the problem involves

- Sum of subarrays
- Multiple range queries
- Running totals
- Cumulative sums
- Prefix counts
- Frequency till index i
- Balance calculations

The biggest clue is

> **Many queries on contiguous ranges.**

---

# 12. Common Mistakes

### Mistake 1

Using

```
prefix[L]
```

instead of

```
prefix[L-1]
```

---

### Mistake 2

Forgetting

```
L==0
```

---

### Mistake 3

Incorrect initialization

Wrong

```python
prefix = [0] * n
```

without assigning

```python
prefix[0]
```

---

### Mistake 4

Confusing Prefix Sum with Sliding Window.

They solve different types of problems.

---

# 13. Prefix Sum Variations

## 1D Prefix Sum

```
3 2 5 1 4
```

Most common.

---

## Prefix Count

Instead of storing sums,

Store counts.

Example

```
Even numbers

Odd numbers

Zeros

Ones
```

---

## Prefix XOR

Instead of

```
+
```

Use

```
XOR
```

Common in bit manipulation problems.

---

## Prefix Product

Less common.

Stores cumulative multiplication.

---

## 2D Prefix Sum

For matrices.

Example

```
1 2 3

4 5 6

7 8 9
```

Allows rectangle sum queries.

---

## Prefix Sum + HashMap ⭐⭐⭐⭐⭐

One of the most important interview patterns.

Used in

- Subarray Sum Equals K
- Binary Subarrays With Sum
- Continuous Subarray Sum

---

## Difference Array

Instead of answering range queries,

Efficiently update ranges.

Very useful in Competitive Programming.

---

# 14. Learning Roadmap

```
Arrays
        ↓
Prefix Sum Basics
        ↓
Range Sum Queries
        ↓
Prefix Count
        ↓
Prefix XOR
        ↓
2D Prefix Sum
        ↓
Difference Array
        ↓
Prefix Sum + HashMap
        ↓
Fenwick Tree (Binary Indexed Tree)
        ↓
Segment Tree
```

---

# 15. Practice Problems

## Beginner

### LeetCode 1480

Running Sum of 1D Array

Learn

- Building Prefix Sum

---

### LeetCode 303

Range Sum Query – Immutable

Learn

- Multiple Queries

---

### LeetCode 724

Find Pivot Index

Learn

- Left Sum
- Right Sum

---

### LeetCode 1732

Find the Highest Altitude

Learn

- Running Prefix

---

### LeetCode 2574

Left and Right Sum Differences

Learn

- Prefix + Suffix

---

## Intermediate

### Subarray Sum Equals K ⭐⭐⭐⭐⭐

Pattern

Prefix Sum + HashMap

---

### Continuous Subarray Sum

Pattern

Prefix + Modulo

---

### Binary Subarrays With Sum

Pattern

Prefix + Frequency Map

---

### Contiguous Array

Pattern

Prefix Balance

---

### Maximum Size Subarray Sum Equals K

Pattern

Prefix + Earliest Occurrence

---

# Cheat Sheet

## Build Prefix

```python
prefix = [0] * len(nums)
prefix[0] = nums[0]

for i in range(1, len(nums)):
    prefix[i] = prefix[i-1] + nums[i]
```

---

## Query

```python
if left == 0:
    answer = prefix[right]
else:
    answer = prefix[right] - prefix[left-1]
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Build Prefix | O(n) |
| Single Query | O(1) |
| Space | O(n) |

---

# Key Takeaways

- Prefix Sum stores cumulative sums.
- Build once in **O(n)**.
- Answer every range sum query in **O(1)**.
- Best suited for **multiple contiguous range queries**.
- Master Prefix Sum before learning:
  - Prefix Sum + HashMap
  - Difference Arrays
  - Fenwick Trees
  - Segment Trees

---

> **Golden Rule:**  
> If a problem asks for the sum (or count) of many contiguous ranges, think **Prefix Sum** first.