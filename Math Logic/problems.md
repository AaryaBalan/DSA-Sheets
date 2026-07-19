# Math Logic Problems

Welcome to the Math Logic Problems section! Here you will find various data structure and algorithm problems that require mathematical observations, pattern recognition, logical reasoning, and optimization techniques.

These problems focus on understanding the hidden mathematical structure behind a problem instead of relying only on brute-force approaches. You will learn how to derive formulas, identify patterns, use number properties, and convert mathematical insights into efficient algorithms.


# 264. Ugly Number II

## Problem Statement

An **ugly number** is a positive integer whose **only prime factors** are:

- 2
- 3
- 5

Given an integer `n`, return the **nth ugly number**.

Example:

```
Input: n = 10

Output: 12

Sequence:
1,2,3,4,5,6,8,9,10,12
```

---

# Understanding the Problem

Before thinking about algorithms, let's understand what an ugly number is.

Examples of ugly numbers:

```
1
2 = 2
3 = 3
4 = 2 × 2
5 = 5
6 = 2 × 3
8 = 2 × 2 × 2
9 = 3 × 3
10 = 2 × 5
12 = 2 × 2 × 3
15 = 3 × 5
```

These are ugly because they contain **only** the prime factors:

```
2
3
5
```

---

Examples of numbers that are NOT ugly

```
7
14 = 2 × 7
21 = 3 × 7
22 = 2 × 11
```

They contain another prime factor besides 2, 3 and 5.

---

# First Thought (Brute Force)

A natural idea is:

```
Start from 1.

Check every number.

If it is ugly,
    store it.

Stop after finding n ugly numbers.
```

For example

```
1 ✓
2 ✓
3 ✓
4 ✓
5 ✓
6 ✓
7 ✗
8 ✓
9 ✓
10 ✓
11 ✗
12 ✓
```

Eventually we'll reach the nth ugly number.

### Problem

The 1690th ugly number is

```
2123366400
```

This means we'd check billions of numbers.

This approach is far too slow.

---

# The Important Observation

Instead of asking

> Is this number ugly?

ask

> How are ugly numbers generated?

Look at the sequence

```
1
2
3
4
5
6
8
9
10
12
15
16
18
20
24
...
```

Notice

```
2 = 1 × 2

3 = 1 × 3

4 = 2 × 2

5 = 1 × 5

6 = 2 × 3

8 = 4 × 2

9 = 3 × 3

10 = 5 × 2

12 = 6 × 2
```

Every ugly number comes from multiplying a previous ugly number by

```
2
or
3
or
5
```

This is the key observation.

---

# Thinking in Reverse

Instead of checking every integer

```
1
2
3
4
5
6
7
8
9
10
...
```

Generate only

```
Ugly Number × 2

Ugly Number × 3

Ugly Number × 5
```

Then always choose the smallest.

Now we never generate bad numbers like

```
7
11
13
17
```

---

# Visualizing the Process

Suppose we already know

```
1
2
3
4
5
```

Now generate three lists.

Multiply everything by 2

```
2
4
6
8
10
...
```

Multiply everything by 3

```
3
6
9
12
15
...
```

Multiply everything by 5

```
5
10
15
20
25
...
```

The next ugly number is simply

```
minimum(
2-list,
3-list,
5-list
)
```

---

# How do we know where to multiply next?

Suppose

```
Ugly numbers

1
2
3
4
5
```

We keep three pointers.

```
i -> next multiple of 2

j -> next multiple of 3

k -> next multiple of 5
```

Initially

```
i = 0
j = 0
k = 0
```

meaning

```
2 × ugly[0]

3 × ugly[0]

5 × ugly[0]
```

which are

```
2
3
5
```

Take the minimum.

Append it.

Move only the pointer that produced it.

---

# Why Three Pointers?

Imagine

```
Current ugly numbers

1
2
3
4
5
```

Pointer for 2

```
2 × 1 = 2
```

After using 2

move pointer

```
2 × 2 = 4
```

After using 4

move pointer

```
2 × 3 = 6
```

The pointer always points to the next unused multiplication.

The same happens for

```
3

and

5
```

---

# Duplicate Problem

Consider

```
6
```

It can be produced in two ways.

```
2 × 3

3 × 2
```

If we move only one pointer,

we'll produce

```
6

6
```

duplicate.

Therefore

```
if minimum == multiple_of_2
    move pointer2

if minimum == multiple_of_3
    move pointer3

if minimum == multiple_of_5
    move pointer5
```

Notice

**three independent `if`s**

NOT

```
if
elif
elif
```

This removes duplicates.

---

# Dry Run

Suppose

```
n = 10
```

Initially

```
Ugly

[1]

i = 0
j = 0
k = 0
```

---

### Step 1

Candidates

```
2 × 1 = 2

3 × 1 = 3

5 × 1 = 5
```

Minimum

```
2
```

Append

```
1 2
```

Move

```
i
```

Now

```
i = 1
```

---

### Step 2

Candidates

```
2 × 2 = 4

3 × 1 = 3

5 × 1 = 5
```

Minimum

```
3
```

Append

```
1 2 3
```

Move

```
j
```

---

### Step 3

Candidates

```
2 × 2 = 4

3 × 2 = 6

5 × 1 = 5
```

Minimum

```
4
```

Append

```
1 2 3 4
```

Move

```
i
```

---

### Step 4

Candidates

```
2 × 3 = 6

3 × 2 = 6

5 × 1 = 5
```

Minimum

```
5
```

Append

```
1 2 3 4 5
```

Move

```
k
```

---

### Step 5

Candidates

```
2 × 3 = 6

3 × 2 = 6

5 × 2 = 10
```

Minimum

```
6
```

Append

```
1 2 3 4 5 6
```

Now

```
6

came from

2 × 3

and

3 × 2
```

So move both

```
i++

j++
```

This avoids another 6 later.

Continue similarly until

```
1
2
3
4
5
6
8
9
10
12
```

The 10th ugly number is

```
12
```

---

# Algorithm

```
Create list

ugly = [1]

Create three pointers

i = 0
j = 0
k = 0

Repeat until size becomes n

    next2 = ugly[i] × 2
    next3 = ugly[j] × 3
    next5 = ugly[k] × 5

    minimum = min(next2,next3,next5)

    append minimum

    if minimum == next2
        i++

    if minimum == next3
        j++

    if minimum == next5
        k++

Return last element.
```

---

# Python Solution

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:

        ugly = [1]

        i = j = k = 0

        while len(ugly) < n:

            next2 = ugly[i] * 2
            next3 = ugly[j] * 3
            next5 = ugly[k] * 5

            nxt = min(next2, next3, next5)

            ugly.append(nxt)

            if nxt == next2:
                i += 1

            if nxt == next3:
                j += 1

            if nxt == next5:
                k += 1

        return ugly[-1]
```

---

# Time Complexity

Each iteration adds exactly one ugly number.

```
Total iterations = n
```

Time Complexity

```
O(n)
```

Space Complexity

```
O(n)
```

---

# Why Does This Work?

Every ugly number is obtained by multiplying a previous ugly number by

```
2

or

3

or

5
```

Therefore, generating candidates from previous ugly numbers guarantees that:

- Every generated number is ugly.
- No ugly number is skipped.
- By always taking the smallest candidate, the sequence remains sorted.

The three pointers ensure that each multiplication stream (×2, ×3, ×5) progresses independently, and advancing all pointers that produce the current minimum prevents duplicates.

---

# How to Build This Intuition

Many beginners think like this:

```
Take a number.

Check if it satisfies the condition.

If yes,
keep it.
```

This is called a **verification approach**.

Experienced problem solvers often ask a different question:

> Can I generate only the valid answers instead of checking every possible answer?

For this problem:

```
Verification

1
2
3
4
5
6
7
8
9
10
...
```

becomes

```
Generation

1

↓

2
3
5

↓

4
6
10

↓

8
9
12
15

...
```

This avoids ever considering invalid numbers like 7, 11, or 13.

---

# Pattern to Remember

Whenever a problem asks you to generate a sequence:

1. Don't immediately think **"How do I check each number?"**
2. Instead ask:
   - Can every valid answer be built from previous valid answers?
   - Is there a recurrence relation?
   - Can I maintain pointers or indices instead of scanning everything?
3. If each new answer depends on earlier answers, it is often a sign that **Dynamic Programming** or a **pointer-based generation** technique can be used.

This change in mindset—from **verification** to **construction**—is one of the most valuable habits in algorithmic problem solving.