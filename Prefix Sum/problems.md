# Prefix Sum Problems

Welcome to the prefix sum problems section! Here you will find various data structure and algorithm problems related to Prefix Sum, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [1371. Find the Longest Substring Containing Vowels in Even Counts](#1371-find-the-longest-substring-containing-vowels-in-even-counts)
- [1314. Matrix Block Sum](#1314-matrix-block-sum)
- [3152. Special Array II](#3152-special-array-ii)
- [2055. Plates Between Candles](#2055-plates-between-candles)
- [2121. Intervals Between Identical Elements](#2121-intervals-between-identical-elements)
- [3381. Maximum Subarray Sum With Length Divisible by K](#3381-maximum-subarray-sum-with-length-divisible-by-k)
- [1124. Longest Well-Performing Interval](#1124-longest-well-performing-interval)
- [238. Product of Array Except Self](#238-product-of-array-except-self)
- [304. Range Sum Query 2D — Immutable](#304-range-sum-query-2d--immutable)

<br><br><br><br><br>

---

# 1371. Find the Longest Substring Containing Vowels in Even Counts

> **Difficulty:** Medium  
> **Pattern:** Prefix State + Bitmask + HashMap

---

# Problem

Given a string `s`, return the length of the **longest substring** in which **each vowel (`a`, `e`, `i`, `o`, `u`) appears an even number of times**.

Only the vowels matter. Consonants can appear any number of times.

---

# Key Observation

The problem **does not care about the exact count** of each vowel.

It only cares whether the count is:

- Even
- Odd

This immediately suggests thinking in terms of **parity** instead of frequency.

Instead of storing:

```
a = 14
e = 9
i = 22
```

we only need:

```
a → Even
e → Odd
i → Even
```

This reduces each vowel to **two possible states**.

---

# Step 1 — Think in Terms of Parity

Parity has only two values.

| Count | Parity |
| ----- | ------ |
| 0     | Even   |
| 1     | Odd    |
| 2     | Even   |
| 3     | Odd    |
| 4     | Even   |

Represent parity using one bit.

```
0 → Even
1 → Odd
```

---

# Step 2 — Represent Every Vowel Using One Bit

Assign one bit to each vowel.

| Vowel | Bit Position |
| ----- | ------------ |
| a     | 0            |
| e     | 1            |
| i     | 2            |
| o     | 3            |
| u     | 4            |

The current parity of all vowels can now be stored in a **5-bit integer**.

Example:

```
00000
```

means

```
a → Even
e → Even
i → Even
o → Even
u → Even
```

---

# Step 3 — Why XOR?

Whenever a vowel appears, its parity flips.

Example for `a`:

```
Even
↓

Odd
↓

Even
↓

Odd
```

This is exactly the behavior of XOR.

```
0 ^ 1 = 1
1 ^ 1 = 0
```

Therefore,

Whenever a vowel appears,

```python
mask ^= (1 << bit)
```

toggles that vowel's parity.

Example

```
Current mask

00000

Read 'e'

↓

00010

Read another 'e'

↓

00000
```

---

# Step 4 — Prefix State

Instead of storing prefix sums, we store a **running parity state**.

Initially

```
00000
```

because every vowel has appeared zero times (even).

As we traverse the string, this mask changes whenever a vowel is encountered.

Example

| Character | Mask  |
| --------- | ----- |
| Start     | 00000 |
| a         | 00001 |
| e         | 00011 |
| a         | 00010 |
| i         | 00110 |

This mask represents the parity of every vowel up to the current index.

---

# Step 5 — The Most Important Observation

Suppose the same mask appears twice.

```
Index 2

10110
```

Later

```
Index 10

10110
```

Since the parity state is identical,

every vowel must have changed parity an **even number of times** between these two indices.

Therefore,

the substring between them contains:

- Even `a`
- Even `e`
- Even `i`
- Even `o`
- Even `u`

Hence, it is a valid substring.

---

# Mathematical Proof

Let

```
PrefixState(i)
```

represent the parity state up to index `i`.

If

```
PrefixState(i) == PrefixState(j)
```

then

```
PrefixState(i) XOR PrefixState(j)

=

00000
```

The parity contributed by the substring is

```
PrefixState(j)
XOR
PrefixState(i)
```

which equals

```
00000
```

meaning every vowel appears an even number of times.

---

# Why Store the First Occurrence?

Suppose the same mask appears at

```
Index 2
```

again at

```
Index 8
```

and again at

```
Index 15
```

Using the earliest occurrence gives

```
15 - 2 = 13
```

Using the later occurrence gives

```
15 - 8 = 7
```

To maximize substring length, always store **the first occurrence** of every mask.

---

# Algorithm

1. Create a bitmask initialized to `0`.
2. Store the first occurrence of mask `0` at index `-1`.
3. Traverse the string.
4. If the current character is a vowel:
   - Flip its corresponding bit.
5. If the current mask has appeared before:
   - Update the maximum length.
6. Otherwise:
   - Store its index.

---

# Dry Run

Input

```
eleet
```

Initial state

```
Mask = 00000

Map

00000 → -1
```

---

### Index 0

Character

```
e
```

Flip bit

```
00000

↓

00010
```

Not seen before.

Store

```
00010 → 0
```

---

### Index 1

Character

```
l
```

Mask remains

```
00010
```

Already seen at

```
0
```

Length

```
1 - 0 = 1
```

Answer

```
1
```

---

### Index 2

Character

```
e
```

Flip bit

```
00010

↓

00000
```

Seen before at

```
-1
```

Length

```
2 - (-1) = 3
```

Substring

```
ele
```

---

### Index 3

Character

```
e
```

Mask

```
00010
```

Seen before at

```
0
```

Length

```
3
```

---

### Index 4

Character

```
t
```

Mask remains

```
00010
```

Seen before at

```
0
```

Length

```
4
```

Answer

```
4
```

Substring

```
leet
```

---

# Python Solution

```python
class Solution:
    def findTheLongestSubstring(self, s: str) -> int:

        vowel_bit = {
            "a": 0,
            "e": 1,
            "i": 2,
            "o": 3,
            "u": 4
        }

        mask = 0

        first_occurrence = {
            0: -1
        }

        longest = 0

        for i, ch in enumerate(s):

            if ch in vowel_bit:
                mask ^= (1 << vowel_bit[ch])

            if mask in first_occurrence:
                longest = max(longest, i - first_occurrence[mask])
            else:
                first_occurrence[mask] = i

        return longest
```

---

# Code Explanation

### Step 1

```python
vowel_bit = {
    "a": 0,
    "e": 1,
    "i": 2,
    "o": 3,
    "u": 4
}
```

Assign one bit to each vowel.

---

### Step 2

```python
mask = 0
```

Initially every vowel has appeared zero times.

```
00000
```

---

### Step 3

```python
first_occurrence = {
    0: -1
}
```

Store the initial state before processing any characters.

This allows substrings starting from index `0`.

---

### Step 4

```python
for i, ch in enumerate(s):
```

Traverse the string once.

---

### Step 5

```python
if ch in vowel_bit:
    mask ^= (1 << vowel_bit[ch])
```

Whenever a vowel appears,

flip its parity.

---

### Step 6

```python
if mask in first_occurrence:
```

If the current parity state has appeared before,

the substring between those indices has even counts for every vowel.

---

### Step 7

```python
longest = max(longest, i - first_occurrence[mask])
```

Update the answer.

---

### Step 8

```python
else:
    first_occurrence[mask] = i
```

Store only the earliest occurrence.

Earlier indices always produce longer substrings.

---

# Complexity Analysis

## Time Complexity

Each character is processed once.

```
O(n)
```

---

## Space Complexity

There are only

```
2⁵ = 32
```

possible parity states.

Therefore,

```
O(32)

=

O(1)
```

constant space.

---

# Interview Thought Process

Instead of jumping directly to an algorithm, ask the following questions:

```
Does the problem care about exact counts?

↓

No

↓

Does it only care about even/odd?

↓

Yes

↓

Think Parity

↓

Can parity be represented using bits?

↓

Yes

↓

Track Prefix State

↓

Store the first occurrence

↓

Repeated state?

↓

Valid substring
```

---

# Recognition Pattern

Whenever a problem contains words like

- Even
- Odd
- Parity
- Toggle
- Flip

consider using

- Bitmask
- XOR
- Prefix State
- HashMap

These problems often reduce to tracking a small state rather than full frequencies.

---

# Key Takeaways

- Ignore exact counts when only parity matters.
- Represent parity using bits.
- XOR naturally models parity changes.
- Store the earliest occurrence of each prefix state.
- If the same state appears again, the substring between them has even occurrences for every tracked character.

---

# Mental Formula

```text
Exact Count?
      │
      ├── Yes
      │       ↓
      │   Frequency / Prefix Sum
      │
      └── No
              │
              ▼
      Only Even / Odd?
              │
              ▼
           Parity
              │
              ▼
           XOR Flip
              │
              ▼
        Prefix State
              │
              ▼
 Store First Occurrence of State
              │
              ▼
      State Repeats Again
              │
              ▼
 Longest Valid Substring Found
```

<br/><br/><br/><br/><br/>

---

# 1314. Matrix Block Sum

> **Difficulty:** Medium  
> **Pattern:** 2D Prefix Sum (Integral Image)

---

# Problem

Given an `m × n` matrix `mat` and an integer `k`, return a new matrix `answer` where:

```
answer[i][j]
```

contains the sum of **all elements within a square of radius `k` centered at `(i, j)`**.

Only **valid positions inside the matrix** are considered.

---

# Understanding the Problem

Before thinking about algorithms, understand **what the problem is asking**.

For every cell:

1. Imagine drawing a square centered at that cell.
2. The square extends `k` cells in every direction.
3. Add every valid element inside that square.
4. Store the sum in the answer matrix.

---

# Example

Input

```text
mat =
[
 [1,2,3],
 [4,5,6],
 [7,8,9]
]

k = 1
```

---

## Center Cell

Find

```
answer[1][1]
```

Current position

```text
1 2 3
4 5 6
7 8 9
```

Since

```
k = 1
```

the square becomes

```text
1 2 3
4 5 6
7 8 9
```

Sum

```text
1+2+3
+4+5+6
+7+8+9
=
45
```

Therefore

```text
answer[1][1] = 45
```

---

## Top Left Corner

Find

```
answer[0][0]
```

Current

```text
[1] 2 3
 4 5 6
 7 8 9
```

Moving one row up or one column left goes outside the matrix.

Ignore invalid positions.

Valid square

```text
1 2
4 5
```

Sum

```text
1+2+4+5 = 12
```

Therefore

```text
answer[0][0] = 12
```

---

## Top Middle

Find

```
answer[0][1]
```

Current

```text
1 [2] 3
4  5  6
7  8  9
```

Valid square

```text
1 2 3
4 5 6
```

Sum

```text
1+2+3+4+5+6 = 21
```

---

## Bottom Left

Find

```
answer[2][0]
```

Current

```text
1 2 3
4 5 6
7 8 9
```

Valid square

```text
4 5
7 8
```

Sum

```text
4+5+7+8 = 24
```

---

## Final Answer

```text
12 21 16
27 45 33
24 39 28
```

---

# What Is the Problem Really Asking?

For every cell,

draw a square of radius `k`.

```text
□□□□□□□□□

□ □ □

□ ■ □

□ □ □

□□□□□□□□□
```

The black cell is the current position.

Every valid cell inside the square contributes to the answer.

---

# Brute Force Solution

The most straightforward solution is:

For every cell

- Visit every row inside the square
- Visit every column inside the square
- Add every value

Pseudo-code

```text
for every cell

    for every nearby row

        for every nearby column

            add value
```

---

## Time Complexity

Each cell visits

```
(2k+1)²
```

cells.

Overall complexity

```text
O(m × n × (2k+1)²)
```

This becomes slow for larger matrices.

---

# Interview Observation

Notice something important.

Every answer is simply the sum of **one rectangle**.

Example

```
answer[0][1]
```

means

```text
1 2 3
4 5 6
```

Another cell corresponds to another rectangle.

So the real question becomes

> Can we answer rectangle sums quickly?

This naturally leads to **2D Prefix Sum**.

---

# 2D Prefix Sum

Instead of repeatedly summing rectangles,

precompute cumulative sums.

The prefix matrix stores

```
prefix[i][j]
```

=

Sum of every element from

```
(0,0)
```

to

```
(i-1,j-1)
```

Using an extra row and column makes boundary handling much easier.

---

## Building the Prefix Matrix

For every cell

```python
prefix[i][j] =
mat[i-1][j-1]
+ prefix[i-1][j]
+ prefix[i][j-1]
- prefix[i-1][j-1]
```

Why subtract?

Because the top-left rectangle gets counted twice.

---

### Visualization

Suppose

```text
A B
C D
```

We want

```
A+B+C+D
```

Adding

```
Top

+

Left
```

counts

```
A
```

twice.

Therefore

```
Top

+

Left

-

TopLeft

+

Current
```

---

# Rectangle Sum Formula

Suppose we want the rectangle

```text
(r1,c1)

↓

(r2,c2)
```

Using prefix sums,

the rectangle sum is

```text
prefix[r2+1][c2+1]

-

prefix[r1][c2+1]

-

prefix[r2+1][c1]

+

prefix[r1][c1]
```

This computes any rectangle in **O(1)**.

---

# Algorithm

1. Build a `(rows+1) × (cols+1)` prefix matrix.
2. For every cell:
   - Compute the block boundaries.
   - Clamp boundaries within the matrix.
   - Use the rectangle formula.
3. Store the result.

---

# Python Solution

```python
class Solution:
    def matrixBlockSum(self, mat: List[List[int]], k: int) -> List[List[int]]:

        rows = len(mat)
        cols = len(mat[0])

        # Extra row and column simplify boundary handling
        prefix = [[0] * (cols + 1) for _ in range(rows + 1)]

        # Build 2D prefix sum
        for i in range(1, rows + 1):
            for j in range(1, cols + 1):
                prefix[i][j] = (
                    mat[i - 1][j - 1]
                    + prefix[i - 1][j]
                    + prefix[i][j - 1]
                    - prefix[i - 1][j - 1]
                )

        answer = [[0] * cols for _ in range(rows)]

        for i in range(rows):
            for j in range(cols):

                r1 = max(0, i - k)
                c1 = max(0, j - k)

                r2 = min(rows - 1, i + k)
                c2 = min(cols - 1, j + k)

                answer[i][j] = (
                    prefix[r2 + 1][c2 + 1]
                    - prefix[r1][c2 + 1]
                    - prefix[r2 + 1][c1]
                    + prefix[r1][c1]
                )

        return answer
```

---

# Code Walkthrough

## Step 1

```python
rows = len(mat)
cols = len(mat[0])
```

Store matrix dimensions.

---

## Step 2

```python
prefix = [[0]*(cols+1) for _ in range(rows+1)]
```

Create an extra row and column.

This removes special cases when accessing boundaries.

---

## Step 3

Build the prefix matrix.

```python
prefix[i][j] =
mat[i-1][j-1]
+ prefix[i-1][j]
+ prefix[i][j-1]
- prefix[i-1][j-1]
```

Each entry stores the sum from

```
(0,0)

↓

(i-1,j-1)
```

---

## Step 4

For every cell,

compute the square boundaries.

```python
r1 = max(0, i-k)
c1 = max(0, j-k)

r2 = min(rows-1, i+k)
c2 = min(cols-1, j+k)
```

These ensure the square never goes outside the matrix.

---

## Step 5

Compute the rectangle sum.

```python
answer[i][j] = (
    prefix[r2+1][c2+1]
    - prefix[r1][c2+1]
    - prefix[r2+1][c1]
    + prefix[r1][c1]
)
```

Each query now takes **constant time**.

---

# Dry Run

Input

```text
1 2 3
4 5 6
7 8 9

k = 1
```

Find

```
answer[0][0]
```

Boundary

```
Top = 0
Left = 0
Bottom = 1
Right = 1
```

Rectangle

```text
1 2
4 5
```

Rectangle formula returns

```
12
```

---

Find

```
answer[1][1]
```

Boundary

```
Top = 0
Left = 0
Bottom = 2
Right = 2
```

Rectangle

```text
1 2 3
4 5 6
7 8 9
```

Result

```
45
```

---

# Complexity Analysis

## Building Prefix Matrix

```text
O(m × n)
```

---

## Computing Every Answer

Each rectangle query is

```text
O(1)
```

There are

```
m × n
```

queries.

Overall

```text
O(m × n)
```

---

## Space Complexity

Prefix matrix

```text
(rows+1) × (cols+1)
```

Therefore

```text
O(m × n)
```

---

# Recognition Pattern

Whenever you encounter repeated questions like:

- Sum of a rectangle
- Sum inside a submatrix
- Matrix region sum
- Square region sum
- Multiple rectangle queries

think immediately of:

```text
2D Prefix Sum
```

---

# Key Takeaways

- Every block is simply a rectangle.
- Brute force repeatedly computes rectangle sums.
- Precompute a 2D prefix matrix once.
- Use the rectangle formula to answer each query in **O(1)**.
- Total complexity improves from

```text
O(m × n × (2k+1)²)

↓

O(m × n)
```

---

# Mental Flowchart

```text
Read Problem
      │
      ▼
Need rectangle sums repeatedly?
      │
      ├── No → Brute Force
      │
      ▼
Many rectangle queries?
      │
      ▼
Build 2D Prefix Sum
      │
      ▼
Use Rectangle Formula
      │
      ▼
Answer Each Query in O(1)
```

<br/><br/><br/><br/><br/>

---

# 3152. Special Array II

**Difficulty:** Medium

**Pattern:** Prefix Sum + Range Query

---

# Problem Statement

You are given:

- An integer array `nums`
- Multiple queries

Each query is

```text
[from, to]
```

For every query,

check whether the subarray

```text
nums[from...to]
```

is **Special**.

Return

```text
True
```

if special,

otherwise

```text
False
```

---

# What is a Special Array?

A subarray is special if

> Every adjacent pair has different parity.

Parity means

- Even
- Odd

---

## Example

```text
[3,4,1,2]
```

Check every adjacent pair.

```
3 4
```

Odd → Even ✅

```
4 1
```

Even → Odd ✅

```
1 2
```

Odd → Even ✅

Everything alternates.

So

```text
Special
```

---

## Example

```text
[3,4,2]
```

Check

```
3 4
```

Odd → Even ✅

```
4 2
```

Even → Even ❌

Not special.

---

# Understanding the Question

Suppose

```text
nums = [3,4,1,2,6]
```

Query

```text
[0,4]
```

Subarray

```text
3 4 1 2 6
```

Pairs

```
3 4

different
```

```
4 1

different
```

```
1 2

different
```

```
2 6

same parity

even even ❌
```

Hence

```text
False
```

---

Another query

```text
[1,3]
```

Subarray

```text
4 1 2
```

Pairs

```
4 1

different
```

```
1 2

different
```

No bad pair.

Answer

```text
True
```

---

# First Thought (Brute Force)

For every query

Traverse every adjacent pair.

```
for every query

    for every adjacent pair

        if same parity

             False
```

---

# Complexity

Suppose

```
n =100000

queries =100000
```

Worst case

```
100000 ×100000
```

Impossible.

Need preprocessing.

---

# Important Observation

The query never asks

```text
How many even?

How many odd?
```

It only asks

```text
Is there any bad adjacent pair?
```

That's the key observation.

---

# What is a Bad Pair?

Two adjacent numbers

having same parity.

Example

```
2 4
```

Even Even ❌

```
3 5
```

Odd Odd ❌

These are

```text
Bad Pairs
```

---

# Good Pair

```
2 3
```

Even Odd ✅

```
7 8
```

Odd Even ✅

---

# New Way of Thinking

Instead of checking

```
Whole subarray
```

Let's mark only

```text
bad positions
```

Example

```
nums

3 4 1 2 6
```

Adjacent pairs

```
3 4

good
```

```
4 1

good
```

```
1 2

good
```

```
2 6

BAD
```

So create

```
bad

0 0 0 1
```

Meaning

```
Index

0

represents

(3,4)
```

```
Index

1

represents

(4,1)
```

etc.

---

# Now the Question Changes

Original question

```
Is subarray special?
```

becomes

```
Is there any BAD pair inside this range?
```

That's much easier.

---

# Prefix Sum Idea

Suppose

```
bad

0 0 0 1
```

Build prefix.

```
prefix

0 0 0 1
```

Meaning

```
prefix[i]

=

Number of bad pairs till i
```

---

# Example

```
bad

0 1 0 1
```

Prefix

```
0 1 1 2
```

Now suppose query asks

```
Check pair positions

1

to

3
```

Bad pairs

```
prefix[3]-prefix[0]

=

2-0

=

2
```

There are

2 bad pairs.

Hence

```text
False
```

---

# Mapping Query to Bad Array

This is the trick.

Suppose

```
nums

3 4 1 2 6
```

Query

```
[1,4]
```

Subarray

```
4 1 2 6
```

Adjacent pairs

```
4 1

1 2

2 6
```

These correspond to

bad array indices

```
1

2

3
```

Notice

```
Left = l

Right = r-1
```

Always.

---

# Algorithm

## Step 1

Create

```
bad
```

For every adjacent pair

```
if same parity

bad[i]=1

else

bad[i]=0
```

---

## Step 2

Build prefix sum.

```
prefix[i]

=

bad[0]+...

+bad[i]
```

---

## Step 3

For every query

Need bad pairs

between

```
l

and

r-1
```

If count

```
==0
```

Answer

```
True
```

Else

```
False
```

---

# Dry Run

Input

```text
nums = [4,3,1,6]

queries

[0,2]

[2,3]
```

---

## Build Bad Array

Pair

```
4 3
```

Different

```
0
```

Pair

```
3 1
```

Same parity

```
1
```

Pair

```
1 6
```

Different

```
0
```

Bad

```
0 1 0
```

---

## Prefix

```
0 1 1
```

---

# Query 1

```
[0,2]
```

Need bad pairs

```
0

to

1
```

Count

```
prefix[1]

=

1
```

One bad pair exists.

Answer

```
False
```

---

# Query 2

```
[2,3]
```

Need pair

```
2
```

Count

```
prefix[2]-prefix[1]

=

1-1

=

0
```

No bad pair.

Answer

```
True
```

---

# Python Code

```python
class Solution:
    def isArraySpecial(self, nums, queries):

        n = len(nums)

        bad = [0] * (n - 1)

        for i in range(n - 1):

            if nums[i] % 2 == nums[i + 1] % 2:
                bad[i] = 1

        prefix = [0] * (n - 1)

        if n > 1:
            prefix[0] = bad[0]

            for i in range(1, n - 1):
                prefix[i] = prefix[i - 1] + bad[i]

        ans = []

        for l, r in queries:

            # Single element is always special
            if l == r:
                ans.append(True)
                continue

            left = l
            right = r - 1

            badPairs = prefix[right]

            if left > 0:
                badPairs -= prefix[left - 1]

            ans.append(badPairs == 0)

        return ans
```

---

# Simpler Prefix Construction (Recommended)

Instead of creating a separate `bad` array, we can directly build a prefix array of size `n`.

```python
class Solution:
    def isArraySpecial(self, nums, queries):

        n = len(nums)

        prefix = [0] * n

        for i in range(1, n):

            prefix[i] = prefix[i - 1]

            if nums[i] % 2 == nums[i - 1] % 2:
                prefix[i] += 1

        ans = []

        for l, r in queries:

            if prefix[r] == prefix[l]:
                ans.append(True)
            else:
                ans.append(False)

        return ans
```

This version is cleaner and is the one most people use in interviews.

---

# Why Does

```python
prefix[r] == prefix[l]
```

Work?

Suppose

```
nums

4 3 1 6
```

Prefix

```
Index

0 1 2 3

Value

0 0 1 1
```

Query

```
[2,3]
```

Compare

```
prefix[3]

=

1

prefix[2]

=

1
```

Difference

```
0
```

Meaning

No new bad pair appeared.

Hence

```
Special
```

---

# Complexity

Building Prefix

```
O(n)
```

Answering Query

```
O(1)
```

Total

```
O(n+q)
```

Space

```
O(n)
```

---

# Pattern Recognition

Whenever you see

```
Many Queries

+

Check whether something exists

inside a range
```

Think

```
Prefix Sum
```

Ask yourself

> Can I preprocess the array so that each query becomes O(1)?

Here,

instead of checking every adjacent pair for every query,

we preprocess

```
Bad Adjacent Pairs
```

and answer each query using prefix sums.

---

# Mathematical Logic

A subarray is special **if and only if** it contains **zero bad adjacent pairs**.

Let

```
Bad Pair

=

Adjacent numbers with same parity.
```

Then

```
Special

⇔

Bad Pair Count = 0
```

Using prefix sums,

```
Bad Pair Count

=

prefix[right]

-

prefix[left]
```

If

```
0
```

Then

```
Special
```

Otherwise

```
Not Special
```

---

# Similar Problems

-  303. Range Sum Query
-  304. Range Sum Query 2D
-  724. Find Pivot Index
-  560. Subarray Sum Equals K
-  525. Contiguous Array
- Range Query Problems using Prefix Sum

---

# Key Takeaways

✅ Convert the original condition into a simpler property (**bad adjacent pairs**).

✅ Precompute the locations of bad pairs.

✅ Use prefix sums to answer each query in O(1).

✅ For every query, check whether the number of bad pairs in the range is zero.

This is a classic **preprocessing + range query** pattern: spend O(n) time once so that each of up to 100,000 queries can be answered instantly.

<br/><br/><br/><br/><br/>

---

# 2055. Plates Between Candles

> **Difficulty:** Medium  
> **Technique:** Prefix Sum + Preprocessing (Nearest Left & Right Arrays)

---

# 📚 Problem Statement

There is a long table containing:

- Plates (`*`)
- Candles (`|`)

For every query `[left, right]`, find the number of **plates that are between two candles** inside that substring.

A plate is counted **only if**

- there is a candle somewhere to its left
- there is a candle somewhere to its right

inside the queried substring.

---

## Example

Input

```text
s = "**|**|***|"

queries = [[2,5],[5,9]]
```

Output

```text
[2,3]
```

---

# 🤔 Understanding the Question

Suppose

```text
s

**|**|***|
```

Index

```text
0 1 2 3 4 5 6 7 8 9
```

Characters

```text
* * | * * | * * * |
```

---

## Query

```text
[2,9]
```

Substring

```text
|**|***|
```

Visualization

```text
| ** | *** |
```

Plates between candles

```text
2 + 3

=

5
```

Answer

```text
5
```

---

## Another Query

```text
[0,4]
```

Substring

```text
**|**
```

Visualization

```text
* * | * *
```

There is only **one candle**.

A plate must have

- candle on left
- candle on right

Impossible.

Answer

```text
0
```

---

# ❌ Brute Force Thinking

For every query

1. Extract substring
2. Find first candle
3. Find last candle
4. Count plates

Example

```python
for query:

    scan substring

    count plates
```

---

## Complexity

If

```
N = 100000

Queries = 100000
```

Worst case

```
100000 × 100000

=

10^10
```

Too slow.

---

# 💡 Observation

Every query asks exactly three things.

```
Find

↓

First Candle

↓

Last Candle

↓

Count Plates Between Them
```

Instead of finding these every time,

can we prepare them once?

Yes.

---

# ⭐ Key Idea

Preprocess the string once.

Create three arrays.

1. Prefix Sum of Plates
2. Nearest Candle on Left
3. Nearest Candle on Right

Then every query becomes O(1).

---

# 1️⃣ Prefix Sum of Plates

Store

```
Number of plates seen till index i
```

Example

```
* * | * * | * * * |
```

Index

```
0 1 2 3 4 5 6 7 8 9
```

Prefix

```
1
2
2
3
4
4
5
6
7
7
```

Or as an array

```text
prefix = [0,1,2,2,3,4,4,5,6,7,7]
```

Notice

```
prefix[i]

=

plates before index i
```

Now

plates between

```
L

and

R
```

becomes

```
prefix[R]

-

prefix[L]
```

No scanning needed.

---

# 2️⃣ Nearest Candle on Left

For every index,

store

```
Nearest candle on LEFT
```

Example

```
* * | * * | * * * |
```

Nearest Left

```text
-1
-1
2
2
2
5
5
5
5
9
```

Meaning

For index

```
7
```

nearest candle on left

```
5
```

---

# 3️⃣ Nearest Candle on Right

Store

```
Nearest candle on RIGHT
```

Example

```text
2
2
2
5
5
5
9
9
9
9
```

Meaning

For index

```
3
```

nearest candle on right

```
5
```

---

# 🧠 Answering a Query

Suppose

```
Query

[0,8]
```

First valid candle

```
right[0]

↓

2
```

Last valid candle

```
left[8]

↓

5
```

Now

plates

```
between

2

and

5
```

Answer

```
prefix[5]

-

prefix[2]

=

4

-

2

=

2
```

Done.

Every query takes

```
O(1)
```

---

# Dry Run

Example

```
s

**|**|***|
```

Query

```
[2,9]
```

Nearest candle from left boundary

```
right[2]

↓

2
```

Nearest candle from right boundary

```
left[9]

↓

9
```

Now

```
plates

=

prefix[9]

-

prefix[2]

=

7

-

2

=

5
```

Answer

```
5
```

---

# Another Dry Run

Query

```
[0,4]
```

Nearest right candle

```
2
```

Nearest left candle

```
2
```

Notice

```
L

=

R
```

Need two different candles.

Answer

```
0
```

---

# Algorithm

Step 1

Compute prefix sum of plates.

---

Step 2

Compute nearest candle on left.

---

Step 3

Compute nearest candle on right.

---

Step 4

For every query

```
L = nearest candle on right of left boundary

R = nearest candle on left of right boundary
```

If

```
L >= R
```

Answer

```
0
```

Otherwise

```
prefix[R]

-

prefix[L]
```

---

# Python Solution

```python
class Solution:
    def platesBetweenCandles(self, s: str, queries: List[List[int]]) -> List[int]:

        n = len(s)

        # Prefix Sum of Plates
        prefix = [0] * (n + 1)

        for i in range(n):
            prefix[i + 1] = prefix[i] + (1 if s[i] == '*' else 0)

        # Nearest Candle on Left
        left = [-1] * n
        last = -1

        for i in range(n):
            if s[i] == '|':
                last = i
            left[i] = last

        # Nearest Candle on Right
        right = [-1] * n
        last = -1

        for i in range(n - 1, -1, -1):
            if s[i] == '|':
                last = i
            right[i] = last

        ans = []

        for l, r in queries:

            L = right[l]
            R = left[r]

            if L == -1 or R == -1 or L >= R:
                ans.append(0)
            else:
                ans.append(prefix[R] - prefix[L])

        return ans
```

---

# Complexity Analysis

### Time Complexity

Preprocessing

```
Prefix

O(N)
```

Left array

```
O(N)
```

Right array

```
O(N)
```

Each Query

```
O(1)
```

Overall

```
O(N + Q)
```

---

### Space Complexity

```
Prefix

O(N)

Left

O(N)

Right

O(N)
```

Overall

```
O(N)
```

---

# 🎯 Pattern Recognition

Whenever you see

- One fixed string/array
- Multiple range queries
- Query count up to `10^5`
- Need information inside a range

Think

```
Preprocessing
```

Possible preprocessing techniques include

- Prefix Sum
- Suffix Sum
- Nearest Left Array
- Nearest Right Array
- Prefix Maximum
- Prefix Minimum

---

# 📝 Key Takeaways

```
Brute Force

↓

Scan every query

↓

O(N × Q)

❌

────────────────────────────

Preprocess Once

↓

Prefix Sum

+

Nearest Left Candle

+

Nearest Right Candle

↓

Every Query

O(1)

↓

Overall

O(N + Q)
```

---

# 🧠 Interview Learning

### ✅ Good Observation

You correctly realized that the answer depends on the **first candle** and the **last candle** inside the query.

---

### ❌ What Needed Improvement

You solved every query independently.

Whenever constraints look like

```
N = 100000

Q = 100000
```

Immediately ask yourself

```
Can I preprocess once

and

answer each query quickly?
```

This is one of the most common interview patterns for range query problems.

<br/><br/><br/><br/><br/>

---

# 2121. Intervals Between Identical Elements

> **Difficulty:** Medium  
> **Technique:** HashMap + Prefix Sum / Running Prefix & Suffix Contribution

---

# 📚 Problem Statement

You are given an array.

```text
arr = [2,1,3,1,2,3,3]
```

For every index `i`, calculate

```
Sum of distances
```

between

```
arr[i]
```

and every other occurrence of the same value.

Distance means

```
|i-j|
```

where

```
j
```

is another index having the same value.

---

# Example

Input

```text
arr = [2,1,3,1,2,3,3]
```

Output

```text
[4,2,7,2,4,4,5]
```

---

# 🤔 Understanding the Problem

Take

```
Index 2

↓

Value = 3
```

Where are other 3's?

```
Index

2
5
6
```

Distances

```
|2-5| = 3

|2-6| = 4
```

Answer

```
3+4=7
```

---

Take

```
Index 5

↓

Value = 3
```

Other 3's

```
2
6
```

Distance

```
|5-2|=3

|5-6|=1
```

Answer

```
4
```

---

# ❌ First Brute Force Thinking

For every index

scan entire array.

```
for i

    for j

        if arr[i]==arr[j]

            answer+=abs(i-j)
```

Complexity

```
O(N²)
```

Impossible for

```
N=100000
```

---

# 💡 Observation 1

We never compare

```
2

with

3
```

Only

same values matter.

Example

```
2

only compares

2
```

```
3

only compares

3
```

So first group equal numbers.

---

# Observation 2

Example

```
2 1 3 1 2 3 3
```

Group indices.

```
2

↓

0

4
```

```
1

↓

1

3
```

```
3

↓

2

5

6
```

Now each group becomes an independent problem.

---

# New Problem

Suppose

```
Indices

2

5

6
```

Need answer for every position.

---

For

```
2
```

```
3+4=7
```

For

```
5
```

```
3+1=4
```

For

```
6
```

```
4+1=5
```

---

# ❌ Brute Force Again

Inside each group

```
for every index

compare all other indices
```

Still

```
O(K²)
```

Need better.

---

# 💡 Biggest Observation

Take

```
Indices

2

5

6
```

Imagine standing at

```
5
```

Everything

```
Left

↓

2
```

Distance

```
5-2
```

Everything

```
Right

↓

6
```

Distance

```
6-5
```

Instead of computing absolute values one by one,

can we calculate

```
Left Contribution

+

Right Contribution
```

Yes.

---

# Understanding Left Contribution

Suppose

```
Indices

2

5

6
```

Standing at

```
5
```

Left

```
2
```

Distance

```
5-2
```

Formula

```
CurrentIndex

×

NumberOfLeftElements

-

SumOfLeftIndices
```

Why?

Because

```
(5-2)

=

5

-

2
```

If there were

```
1

2

4
```

Standing at

```
7
```

Distance

```
7-1

+

7-2

+

7-4
```

Expand

```
7+7+7

-

(1+2+4)
```

Which becomes

```
7×3

-

7
```

General Formula

```
CurrentIndex × LeftCount

-

LeftSum
```

---

# Understanding Right Contribution

Suppose

```
Standing at

5
```

Right

```
6
```

Distance

```
6-5
```

Formula

```
RightSum

-

CurrentIndex × RightCount
```

Because

```
(6-5)

=

6

-

5
```

If many elements

```
9

10

13
```

Standing at

```
5
```

Distance

```
9-5

+

10-5

+

13-5
```

Expand

```
(9+10+13)

-

5×3
```

Formula

```
RightSum

-

Current×RightCount
```

---

# Final Formula

For every occurrence

```
Answer

=

Left Contribution

+

Right Contribution
```

Which becomes

```
CurrentIndex×LeftCount

-

LeftSum

+

RightSum

-

CurrentIndex×RightCount
```

---

# How Do We Compute Efficiently?

We process each group twice.

---

## Pass 1

Left → Right

Maintain

```
Left Count

Left Sum
```

Compute

```
Left Contribution
```

---

## Pass 2

Right → Left

Maintain

```
Right Count

Right Sum
```

Compute

```
Right Contribution
```

---

# Example Dry Run

Group

```
2

5

6
```

---

## Left Pass

Initially

```
LeftCount=0

LeftSum=0
```

---

Current

```
2
```

Contribution

```
0
```

Update

```
Count=1

Sum=2
```

---

Current

```
5
```

Contribution

```
5×1

-

2

=

3
```

Update

```
Count=2

Sum=7
```

---

Current

```
6
```

Contribution

```
6×2

-

7

=

5
```

Update

```
Count=3

Sum=13
```

Left contributions

```
0

3

5
```

---

## Right Pass

Start

```
Count=0

Sum=0
```

---

Current

```
6
```

Contribution

```
0
```

Update

```
Count=1

Sum=6
```

---

Current

```
5
```

Contribution

```
6

-

5×1

=

1
```

Update

```
Count=2

Sum=11
```

---

Current

```
2
```

Contribution

```
11

-

2×2

=

7
```

Update

```
Count=3

Sum=13
```

Right contribution

```
7

1

0
```

---

Final Answer

```
Left

+

Right
```

```
0+7=7

3+1=4

5+0=5
```

Exactly matches

```
7

4

5
```

---

# Algorithm

Step 1

Group indices by value.

Example

```
2

↓

0

4
```

---

Step 2

For every group

Perform Left Pass.

---

Step 3

Perform Right Pass.

---

Step 4

Store answer.

---

# Python Solution

```python
from collections import defaultdict

class Solution:
    def getDistances(self, arr):
        groups = defaultdict(list)

        # Store indices for each value
        for i, val in enumerate(arr):
            groups[val].append(i)

        ans = [0] * len(arr)

        # Process each group
        for indices in groups.values():

            # Left contribution
            left_sum = 0
            left_count = 0

            for idx in indices:
                ans[idx] += idx * left_count - left_sum
                left_sum += idx
                left_count += 1

            # Right contribution
            right_sum = 0
            right_count = 0

            for idx in reversed(indices):
                ans[idx] += right_sum - idx * right_count
                right_sum += idx
                right_count += 1

        return ans
```

---

# Complexity Analysis

Building HashMap

```
O(N)
```

Processing Groups

```
O(N)
```

Total

```
O(N)
```

Space

```
O(N)
```

---

# 🧠 Pattern Recognition

Whenever you see

```
Sum of distances

Same values only

Many repeated elements

Absolute difference
```

Think

```
Group equal values

↓

Process indices only

↓

Left Contribution

+

Right Contribution

↓

Running Prefix / Running Suffix
```

---

# 🎯 How to Think in Interviews

When you first saw this problem, it's completely normal to feel stuck because your brain naturally goes to:

```
For every index

↓

Compare with every other index
```

The mental shift interviewers expect is:

1. **Ignore unrelated elements.** Only equal values matter, so group them.
2. **Don't compute each distance separately.** Distances on the left and right follow a pattern.
3. **Turn many absolute differences into a formula** using counts and sums.
4. **Maintain running information** (count and sum) instead of recomputing.

This "group → derive a formula → maintain running state" approach is a common pattern in many hard array and prefix-sum problems.

<br/><br/><br/><br/><br/>

---

# 3381. Maximum Subarray Sum With Length Divisible by K

> **Difficulty:** Medium
>
> **Technique:** Prefix Sum + Modulo Observation + HashMap

---

# Step 1: Read the Question Carefully

We are asked to find

```
Maximum Subarray Sum
```

BUT

There is a condition

```
Length of Subarray

must be divisible by K
```

Notice carefully.

The condition is **NOT**

```
Sum divisible by K
```

It is

```
Length divisible by K
```

Many beginners read it incorrectly.

---

# Step 2: Brute Force Thinking

Suppose

```
nums = [1,2,3,4]

k = 2
```

Generate every subarray.

```
[1]

length =1 ❌

----------------

[1,2]

length=2 ✅

sum=3

----------------

[1,2,3]

length=3 ❌

----------------

[1,2,3,4]

length=4 ✅

sum=10

...
```

Time complexity

```
O(N²)
```

Too slow.

---

# Step 3: Whenever You See

```
Maximum Sum

Subarray
```

Immediately think

```
Prefix Sum
```

This is your first instinct.

---

# Step 4: Prefix Sum Formula

Suppose

```
prefix[i]

=

sum of first i elements
```

Then

```
Subarray Sum

(L,R)

=

prefix[R+1]

-

prefix[L]
```

Nothing new.

---

# Step 5: What About the Length?

Length

```
R-L+1
```

Condition

```
(R-L+1)%k==0
```

Let's rewrite.

```
R-L+1

=

multiple of k
```

Move terms.

```
(R+1)-L

=

multiple of k
```

Now think carefully.

This looks exactly like

```
prefix index

-

starting prefix index
```

Interesting.

---

# Step 6: Important Observation

Suppose

```
k=3
```

Prefix indices

```
0

1

2

3

4

5

6

7

8
```

Take

```
Prefix index

8
```

Need another prefix index

whose difference

is divisible by 3.

Example

```
8-5=3

✓
```

```
8-2=6

✓
```

```
8-7=1

✗
```

Notice something?

Look at remainder.

```
8%3

=

2
```

```
5%3

=

2
```

```
2%3

=

2
```

All have SAME remainder.

---

# Huge Observation

If

```
(a-b)%k==0
```

Then

```
a%k

=

b%k
```

This is one of the most useful mathematical identities in DSA.

Remember forever.

---

# Step 7: What Does This Mean?

Suppose

Current Prefix Index

```
i
```

Need another prefix index

```
j
```

such that

```
(i-j)%k==0
```

Meaning

```
i%k

=

j%k
```

So instead of searching

by length,

we only care about

```
Prefix Index % K
```

Amazing.

---

# Step 8: Now the Maximum Sum

Suppose

Current Prefix Sum

```
currentPrefix
```

Need

```
currentPrefix

-

smallestPrefixSeen
```

Because

```
Current

-

Smallest

=

Maximum
```

Exactly like Kadane thinking.

---

# Step 9: What Do We Store?

For every remainder

```
0

1

2

...

k-1
```

Store

```
Minimum Prefix Sum
```

Why minimum?

Because

```
Current Prefix

-

Minimum Prefix

=

Maximum Sum
```

---

# Dry Run

nums

```
[-5,1,2,-3,4]
```

k

```
2
```

Prefix

```
0

-5

-4

-2

-5

-1
```

Prefix indices

```
0

1

2

3

4

5
```

Now compute

```
Index %2
```

```
0→0

1→1

2→0

3→1

4→0

5→1
```

Now process.

Initially

```
remainder

0

↓

minimum prefix

0
```

---

Index

```
1
```

Prefix

```
-5
```

Remainder

```
1
```

Store

```
-5
```

---

Index

```
2
```

Prefix

```
-4
```

Remainder

```
0
```

Current answer

```
-4

-

0

=

-4
```

Minimum stays

```
-4?
```

No.

Minimum becomes

```
-4? Wait

0<-4?

Actually

minimum=-4
```

---

Continue similarly.

Eventually

maximum

```
4
```

---

# Algorithm

1. Compute prefix sums.
2. For every prefix index

```
i
```

find

```
remainder=i%k
```

3. Maintain

```
minimum prefix sum

for each remainder
```

4. Update answer

```
current prefix

-

minimum prefix
```

5. Update minimum.

Done.

---

# Pattern Recognition

Whenever you see

```
Subarray

+

Length

divisible

by

K
```

Think

```
Prefix Index

NOT

Prefix Sum
```

Because

length depends on

```
Indices
```

not values.

---

# Mental Checklist

Question contains

```
Subarray
```

↓

Think Prefix Sum

↓

Condition on Length?

↓

Think Prefix Indices

↓

Difference divisible by K?

↓

Same remainder

↓

HashMap

↓

Store Best Prefix

↓

Answer

# Step 10: Complete Dry Run

Let's completely dry run the algorithm.

Example

```text
nums = [-5,1,2,-3,4]

k = 2
```

---

## Prefix Sums

Remember

```
prefix[0] = 0
```

Compute prefix sums.

| Prefix Index | Prefix Sum |
| ------------ | ---------: |
| 0            |          0 |
| 1            |         -5 |
| 2            |         -4 |
| 3            |         -2 |
| 4            |         -5 |
| 5            |         -1 |

---

## Prefix Index Mod K

Since

```
k = 2
```

Compute

```
prefix_index % 2
```

| Prefix Index | Prefix Sum | Index % 2 |
| ------------ | ---------: | --------: |
| 0            |          0 |         0 |
| 1            |         -5 |         1 |
| 2            |         -4 |         0 |
| 3            |         -2 |         1 |
| 4            |         -5 |         0 |
| 5            |         -1 |         1 |

---

## What Will We Store?

Dictionary

```
minimumPrefix[remainder]
```

Initially

```
minimumPrefix

{

0 : 0

}
```

Because

```
Prefix Index = 0

Prefix Sum = 0
```

Answer

```
-∞
```

---

## Process Prefix Index 1

Current

```
Prefix Sum

=

-5
```

Remainder

```
1
```

No previous prefix having remainder 1.

Store

```
minimumPrefix

{

0:0,

1:-5

}
```

Answer unchanged.

---

## Process Prefix Index 2

Current Prefix

```
-4
```

Remainder

```
0
```

Already have

```
minimumPrefix[0]

=

0
```

Possible answer

```
-4

-

0

=

-4
```

Update

```
answer

=

-4
```

Now update minimum

Current minimum

```
0
```

Current prefix

```
-4
```

Smaller

↓

Update

```
minimumPrefix[0]

=

-4
```

Dictionary

```
{

0:-4,

1:-5

}
```

---

## Process Prefix Index 3

Current Prefix

```
-2
```

Remainder

```
1
```

minimum

```
-5
```

Candidate

```
-2

-

(-5)

=

3
```

Update answer

```
max(-4,3)

=

3
```

Update minimum?

Current minimum

```
-5
```

Current prefix

```
-2
```

No.

Dictionary remains

```
{

0:-4,

1:-5

}
```

---

## Process Prefix Index 4

Current Prefix

```
-5
```

Remainder

```
0
```

Candidate

```
-5

-

(-4)

=

-1
```

Answer stays

```
3
```

Update minimum

```
Current minimum

-4

Current Prefix

-5
```

Update

```
minimumPrefix[0]

=

-5
```

---

## Process Prefix Index 5

Current Prefix

```
-1
```

Remainder

```
1
```

Candidate

```
-1

-

(-5)

=

4
```

Update

```
answer

=

4
```

Finished.

---

Final Answer

```
4
```

Exactly matches the expected output.

---

# Why Do We Store Minimum Prefix Sum?

Suppose

Current Prefix

```
20
```

Possible previous prefixes

```
15

7

2

10
```

Subarray sums

```
20-15 = 5

20-7 = 13

20-2 = 18

20-10 = 10
```

Which gives the maximum?

```
20

-

Smallest Prefix
```

Always.

So for every remainder

we only need

```
Smallest Prefix Sum
```

No other prefix is useful.

---

# Why Does This Work?

Suppose

```
Current Prefix Index

=

10
```

Need

```
Subarray Length

divisible by 3
```

Possible starting prefix indices

```
7

4

1
```

Notice

```
10 % 3

=

1
```

```
7 % 3

=

1
```

```
4 % 3

=

1
```

```
1 % 3

=

1
```

Same remainder.

Therefore,

all valid starting points belong to the same remainder group.

Among them,

choose the one having

```
Minimum Prefix Sum
```

to maximize the answer.

---

# Python Solution

```python
from math import inf

class Solution:
    def maxSubarraySum(self, nums, k):

        # minimum prefix sum for every remainder
        min_prefix = {0: 0}

        prefix = 0

        ans = -inf

        for i, num in enumerate(nums):

            prefix += num

            # prefix index
            idx = i + 1

            rem = idx % k

            if rem in min_prefix:
                ans = max(ans, prefix - min_prefix[rem])

            if rem not in min_prefix:
                min_prefix[rem] = prefix
            else:
                min_prefix[rem] = min(min_prefix[rem], prefix)

        return ans
```

---

# Complexity Analysis

### Time Complexity

Each element is processed exactly once.

```
O(N)
```

---

### Space Complexity

We store at most

```
k
```

remainders.

```
O(k)
```

---

# Visualization

```
Array
 │
 ▼
Compute Prefix Sum
 │
 ▼
Current Prefix Index
 │
 ▼
Compute

Index % K
 │
 ▼
Same remainder?
 │
 ├── No
 │      │
 │      ▼
 │   Store Prefix
 │
 └── Yes
        │
        ▼
Candidate Sum

Current Prefix

-

Minimum Prefix

        │
        ▼
Update Answer
        │
        ▼
Update Minimum Prefix
```

---

# Pattern Recognition

Whenever you see

```
Maximum

Subarray

+

Length Divisible by K
```

Think immediately

```
Subarray

↓

Prefix Sum

↓

Length

↓

Prefix Indices

↓

(Index Difference)

↓

Same Remainder

↓

HashMap

↓

Store Minimum Prefix
```

---

# Interview Trick ⭐

There are two very similar but different patterns:

## Pattern 1

### Sum Divisible by K

Store

```
Prefix Sum % K
```

Examples:

-  974. Subarray Sums Divisible by K
-  523. Continuous Subarray Sum

---

## Pattern 2

### Length Divisible by K

Store

```
Prefix Index % K
```

Example:

-  3381. Maximum Subarray Sum With Length Divisible by K

---

# 📝 Final Cheat Sheet

```
Subarray Problem
        │
        ▼
Think Prefix Sum
        │
        ▼
Constraint on SUM?
        │
        ├── Yes
        │      │
        │      ▼
        │  Prefix Sum % K
        │
        ▼
Constraint on LENGTH?
        │
        ▼
Prefix Index % K
        │
        ▼
Same Remainder
        │
        ▼
Need Maximum?
        │
        ▼
Store Minimum Prefix Sum
        │
        ▼
Current Prefix - Minimum Prefix
        │
        ▼
Answer
```

<br/><br/><br/><br/><br>

---

# 1124. Longest Well-Performing Interval

> **Difficulty:** Medium  
> **Technique:** Prefix Sum + HashMap

---

# 📚 Problem Statement

You are given an array `hours`, where each element represents the number of hours worked on that day.

A day is called:

- **Tiring Day** → hours > 8
- **Non-Tiring Day** → hours ≤ 8

A **Well-Performing Interval** is a continuous subarray where

```
Number of Tiring Days
        >
Number of Non-Tiring Days
```

Return the **length of the longest well-performing interval**.

---

# Example

```
Input

hours = [9,9,6,0,6,6,9]
```

Output

```
3
```

Explanation

```
[9,9,6]

Hard Days = 2

Easy Days = 1

2 > 1

Length = 3
```

---

# Step 1 : Understand the Problem

Instead of thinking

```
Hours
```

think

```
Hard Days

Easy Days
```

Example

```
hours

9 9 6 0 6 6 9
```

Convert into

```
H H E E E E H
```

Our condition becomes

```
Hard Days

>

Easy Days
```

---

# Step 2 : Convert English into Mathematics

Instead of keeping two counts

```
Hard

Easy
```

Move everything to one side.

```
Hard > Easy

↓

Hard - Easy > 0
```

This is the biggest trick.

Whenever you see

```
More A than B
```

convert it into

```
A - B > 0
```

---

# Step 3 : Convert Categories into Numbers

Now represent each day by one number.

```
Hard Day

↓

+1
```

```
Easy Day

↓

-1
```

Example

```
9 9 6 0 6 6 9
```

becomes

```
+1 +1 -1 -1 -1 -1 +1
```

Now the problem changes into

> Find the **longest subarray whose sum is greater than 0.**

This is much easier.

---

# Step 4 : Prefix Sum Appears

Whenever you hear

```
Subarray Sum
```

think

```
Prefix Sum
```

Let's compute it.

---

## Prefix Sum

Array

```
+1 +1 -1 -1 -1 -1 +1
```

Start

```
prefix = 0
```

| Index | Value | Prefix |
| ----- | ----- | ------ |
| -1    | —     | 0      |
| 0     | +1    | 1      |
| 1     | +1    | 2      |
| 2     | -1    | 1      |
| 3     | -1    | 0      |
| 4     | -1    | -1     |
| 5     | -1    | -2     |
| 6     | +1    | -1     |

So

```
Prefix

0

1

2

1

0

-1

-2

-1
```

---

# Step 5 : Why Prefix Sum Works

Remember

```
Subarray Sum

=

Current Prefix

-

Previous Prefix
```

We need

```
Subarray Sum > 0
```

Therefore

```
Current Prefix

-

Previous Prefix

>

0
```

Move

```
Previous Prefix
```

to the other side.

```
Current Prefix

>

Previous Prefix
```

So for every position

we need an **earlier prefix sum that is smaller than the current prefix**.

---

# Step 6 : Important Observation

Suppose

```
Current Prefix = 5
```

Previous Prefix can be

```
4

3

2

1

0

...
```

Searching all smaller values every time would be expensive.

---

# Why Only Check `prefix - 1`?

This is the trick that most people memorize.

Let's understand why.

Our prefix changes only by

```
+1

or

-1
```

because each day contributes only

```
+1

or

-1
```

Suppose

```
Current Prefix = -1
```

Need

```
Previous Prefix

<

-1
```

Possible values

```
-2

-3

-4
```

If any smaller prefix exists,

then

```
prefix - 1
```

must also have appeared before.

So checking

```
prefix - 1
```

is sufficient.

---

# Why Store the First Occurrence?

Suppose

```
Prefix = 2
```

appears at

```
Index 3

and

Index 8
```

Current Index

```
12
```

Using

```
3

↓

Length = 9
```

Using

```
8

↓

Length = 4
```

The earlier occurrence always gives a longer interval.

Therefore

store only the first occurrence.

---

# Dry Run

Input

```
hours

9 9 6 0 6 6 9
```

Converted

```
+1 +1 -1 -1 -1 -1 +1
```

---

Initial

```
prefix = 0

ans = 0

first = {}
```

---

## Index 0

Value

```
+1
```

Prefix

```
1
```

Since

```
prefix > 0
```

Entire array

```
[9]
```

is valid.

Answer

```
1
```

Dictionary

```
{}
```

---

## Index 1

Value

```
+1
```

Prefix

```
2
```

Still positive.

Entire array

```
9 9
```

Answer

```
2
```

Dictionary

```
{}
```

---

## Index 2

Value

```
-1
```

Prefix

```
1
```

Still positive.

Entire array

```
9 9 6
```

Answer

```
3
```

Dictionary

```
{}
```

---

## Index 3

Value

```
-1
```

Prefix

```
0
```

Not positive.

Store first occurrence.

```
first[0]=3
```

Dictionary

```
{

0 : 3

}
```

Check

```
prefix-1

=

-1
```

Not present.

Answer

```
3
```

---

## Index 4

Value

```
-1
```

Prefix

```
-1
```

Store

```
first[-1]=4
```

Dictionary

```
{

0 : 3

-1 : 4

}
```

Check

```
-2
```

Not present.

---

## Index 5

Value

```
-1
```

Prefix

```
-2
```

Store

```
first[-2]=5
```

Dictionary

```
{

0 : 3

-1 : 4

-2 : 5

}
```

Check

```
-3
```

Not found.

---

## Index 6

Value

```
+1
```

Prefix

```
-1
```

Already exists.

Do NOT overwrite.

Check

```
prefix-1

=

-2
```

Found

```
first[-2]=5
```

Length

```
6-5

=

1
```

Answer remains

```
3
```

---

# Final Answer

```
3
```

Subarray

```
9 9 6
```

---

# Algorithm

1. Convert

```
Hard Day → +1

Easy Day → -1
```

2. Compute Prefix Sum.

3. If

```
prefix > 0
```

then

```
Entire array

0...i
```

is valid.

4. Otherwise

- Store first occurrence of the prefix.
- Check whether

```
prefix - 1
```

exists.

5. Update the maximum interval length.

---

# Python Code

```python
class Solution:
    def longestWPI(self, hours):
        prefix = 0
        ans = 0

        # Stores first occurrence of every prefix sum
        first = {}

        for i, h in enumerate(hours):

            # Convert hours to +1 / -1
            if h > 8:
                prefix += 1
            else:
                prefix -= 1

            # Entire prefix is valid
            if prefix > 0:
                ans = i + 1

            else:
                # Store first occurrence only
                if prefix not in first:
                    first[prefix] = i

                # Check for a smaller prefix
                if (prefix - 1) in first:
                    ans = max(ans, i - first[prefix - 1])

        return ans
```

---

# Complexity Analysis

### Time Complexity

```
O(N)
```

Each element is processed once.

---

### Space Complexity

```
O(N)
```

HashMap stores at most one entry for every prefix sum.

---

# Pattern Recognition

Whenever you see

```
More A than B
```

follow this thinking.

```
English

↓

Hard > Easy

↓

Hard - Easy > 0

↓

Hard = +1

Easy = -1

↓

Subarray Sum > 0

↓

Prefix Sum

↓

Current Prefix > Previous Prefix

↓

HashMap
```

This thinking is used in many interview problems such as

-  1124. Longest Well-Performing Interval
-  525. Contiguous Array
-  560. Subarray Sum Equals K
-  974. Subarray Sums Divisible by K
-  1590. Make Sum Divisible by P

---

# Interview Tips

## Whenever you see

```
More A than B
```

Think

```
A = +1

B = -1
```

---

## Whenever you see

```
Subarray Sum
```

Think

```
Prefix Sum
```

---

## Whenever you see

```
Longest Subarray
```

Think

```
HashMap

or

Monotonic Stack
```

depending on the condition.

---

# Key Takeaways

- Convert categories into numbers whenever possible.
- Transform comparisons like `A > B` into mathematical expressions.
- Prefix Sum converts subarray problems into prefix comparisons.
- Store the **first occurrence** of each prefix sum to maximize interval length.
- The transformation is more important than memorizing the code.

<br/><br/><br/><br/><br>

---

# 238. Product of Array Except Self

> **Difficulty:** Medium  
> **Pattern:** Prefix Product + Suffix Product  
> **Time Complexity:** `O(n)`  
> **Extra Space:** `O(1)` excluding the output array

---

# Problem

Given an integer array `nums`, create an array `answer` where:

```text
answer[i]
```

contains the product of **all elements except `nums[i]`**.

You are **not allowed to use division**, and the solution must run in:

```text
O(n)
```

---

## Example

Input:

```text
nums = [1, 2, 3, 4]
```

For index `0`:

```text
Ignore 1

2 × 3 × 4 = 24
```

For index `1`:

```text
Ignore 2

1 × 3 × 4 = 12
```

For index `2`:

```text
Ignore 3

1 × 2 × 4 = 8
```

For index `3`:

```text
Ignore 4

1 × 2 × 3 = 6
```

Output:

```text
[24, 12, 8, 6]
```

---

# Key Observation

The most important phrase in the problem is:

```text
Product of all elements EXCEPT nums[i]
```

Suppose we are at index `i`.

The array can be imagined as:

```text
[ Everything Before i ] [ nums[i] ] [ Everything After i ]

          LEFT             SELF              RIGHT
```

If we don't want to include `nums[i]`, then:

```text
answer[i]

=

Product of everything LEFT of i

×

Product of everything RIGHT of i
```

This is the entire intuition behind the solution.

---

# Step 1 — How Would a Common Person Think?

Suppose:

```text
nums = [1, 2, 3, 4]
```

You want the answer for every number.

The most natural approach is:

```text
For 1 → multiply 2 × 3 × 4

For 2 → multiply 1 × 3 × 4

For 3 → multiply 1 × 2 × 4

For 4 → multiply 1 × 2 × 3
```

This works.

But notice how much work we repeat.

For example:

```text
1 × 2
```

is calculated multiple times.

```text
3 × 4
```

is also useful multiple times.

So we should ask:

> Can we calculate these repeated products once and reuse them?

This is the first step toward the optimized solution.

---

# Step 2 — Understand the Brute Force Approach

For every index `i`:

1. Traverse the entire array.
2. Multiply every number except `nums[i]`.

Pseudo logic:

```text
for every i:

    product = 1

    for every j:

        if i != j:

            product *= nums[j]
```

Example:

```text
nums = [1, 2, 3, 4]
```

For index `0`:

```text
2 × 3 × 4
```

For index `1`:

```text
1 × 3 × 4
```

And so on.

---

## Time Complexity

For every element:

```text
O(n)
```

we traverse the entire array:

```text
O(n)
```

Therefore:

```text
O(n²)
```

But the problem requires:

```text
O(n)
```

So brute force is not enough.

---

# Step 3 — The Division Idea

The next natural thought might be:

> Why don't we calculate the total product once?

Example:

```text
nums = [1, 2, 3, 4]
```

Total:

```text
1 × 2 × 3 × 4 = 24
```

Then:

```text
answer[0] = 24 / 1 = 24

answer[1] = 24 / 2 = 12

answer[2] = 24 / 3 = 8

answer[3] = 24 / 4 = 6
```

This would be:

```text
O(n)
```

But the problem explicitly says:

```text
Without using division
```

Also, zero causes complications.

Example:

```text
[1, 2, 0, 4]
```

Total product:

```text
0
```

Now division is problematic.

So we need another way.

---

# Step 4 — Break the Problem Around the Current Index

This is the most important thinking step.

Suppose:

```text
nums = [1, 2, 3, 4]
```

We want:

```text
answer[2]
```

Current element:

```text
3
```

Visualize:

```text
[1, 2]    [3]    [4]

  LEFT    SELF   RIGHT
```

We want everything except:

```text
3
```

So simply combine:

```text
LEFT PRODUCT

×

RIGHT PRODUCT
```

That gives:

```text
(1 × 2) × (4)

=

8
```

Therefore:

```text
answer[2] = 8
```

This gives us the main formula:

```text
answer[i]

=

left_product[i]

×

right_product[i]
```

---

# Step 5 — What Is Left Product?

`left_product[i]` means:

> Product of every element strictly before index `i`.

Example:

```text
nums = [1, 2, 3, 4]
```

For every index:

```text
Index 0:

Nothing before 1

Left = 1
```

```text
Index 1:

1

Left = 1
```

```text
Index 2:

1 × 2

Left = 2
```

```text
Index 3:

1 × 2 × 3

Left = 6
```

So:

```text
LEFT

[1, 1, 2, 6]
```

---

# Step 6 — Why Is the First Left Product 1?

For index `0`:

```text
[1, 2, 3, 4]
 ↑
```

There is nothing before it.

So why do we use:

```text
1
```

instead of:

```text
0
```

Because `1` is the identity value of multiplication.

```text
x × 1 = x
```

If we used:

```text
0
```

then:

```text
x × 0 = 0
```

which would destroy the result.

So whenever there are no elements to multiply, we use:

```text
1
```

---

# Step 7 — What Is Right Product?

`right_product[i]` means:

> Product of every element strictly after index `i`.

For:

```text
nums = [1, 2, 3, 4]
```

We get:

```text
Index 0:

2 × 3 × 4

= 24
```

```text
Index 1:

3 × 4

= 12
```

```text
Index 2:

4

= 4
```

```text
Index 3:

Nothing after it

= 1
```

Therefore:

```text
RIGHT

[24, 12, 4, 1]
```

---

# Step 8 — Combine Left and Right

Now we have:

```text
nums

[1, 2, 3, 4]
```

Left products:

```text
[1, 1, 2, 6]
```

Right products:

```text
[24, 12, 4, 1]
```

For every index:

```text
answer[i]

=

left[i] × right[i]
```

So:

```text
Index 0:

1 × 24 = 24
```

```text
Index 1:

1 × 12 = 12
```

```text
Index 2:

2 × 4 = 8
```

```text
Index 3:

6 × 1 = 6
```

Final:

```text
[24, 12, 8, 6]
```

---

# Mathematical Logic

Suppose:

```text
nums = [a, b, c, d, e]
```

We want the answer for:

```text
c
```

That means:

```text
answer[2]
```

Everything except `c` is:

```text
a × b × d × e
```

We can split this as:

```text
(a × b)

×

(d × e)
```

Where:

```text
a × b
```

is the prefix product before `c`.

And:

```text
d × e
```

is the suffix product after `c`.

Therefore:

```text
answer[i]

=

(nums[0] × ... × nums[i-1])

×

(nums[i+1] × ... × nums[n-1])
```

Notice:

```text
nums[i]
```

never appears in the formula.

So the current element is automatically excluded.

---

# Step 9 — First Solution Using Separate Left and Right Arrays

The easiest optimized version to understand is:

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:

        n = len(nums)

        left = [1] * n
        right = [1] * n

        for i in range(1, n):
            left[i] = left[i - 1] * nums[i - 1]

        for i in range(n - 2, -1, -1):
            right[i] = right[i + 1] * nums[i + 1]

        answer = [1] * n

        for i in range(n):
            answer[i] = left[i] * right[i]

        return answer
```

This runs in:

```text
O(n)
```

but uses:

```text
O(n)
```

extra space for `left` and `right`.

We can improve it further.

---

# Step 10 — How Do We Reach O(1) Extra Space?

We don't actually need separate:

```text
left[]
```

and:

```text
right[]
```

arrays.

The problem says:

> The output array does not count as extra space.

So we can use:

```text
answer[]
```

to temporarily store the left products.

First:

```text
answer = LEFT products
```

Then traverse backwards and multiply the right products directly into it.

---

# Step 11 — Build Left Products Directly in Answer

Start:

```text
nums = [1, 2, 3, 4]

answer = [1, 1, 1, 1]

left = 1
```

We move:

```text
LEFT → RIGHT
```

At every index:

```text
answer[i] = left
```

Then:

```text
left *= nums[i]
```

The important idea is:

```text
Use the current left product first.

Then include nums[i] for the next index.
```

---

# Why Do We Store Before Updating?

Look at:

```python
answer[i] = left
left *= nums[i]
```

Why not:

```python
left *= nums[i]
answer[i] = left
```

Because we need:

```text
Product BEFORE current index
```

not:

```text
Product including current index
```

Example:

```text
nums = [1, 2, 3, 4]
```

At index `2`:

```text
nums[2] = 3
```

We want:

```text
1 × 2 = 2
```

We do NOT want:

```text
1 × 2 × 3 = 6
```

So the order must be:

```text
STORE

↓

THEN UPDATE
```

Mental rule:

> **Give the current index the information from before it, then include the current element for the next index.**

---

# Step 12 — Add the Right Product

Now traverse:

```text
RIGHT → LEFT
```

Maintain:

```text
right = 1
```

At every index:

```python
answer[i] *= right
```

Then:

```python
right *= nums[i]
```

Again:

```text
USE first

↓

UPDATE second
```

Because `right` must represent everything strictly after the current index.

---

# Algorithm

1. Create `answer` filled with `1`.
2. Initialize `left = 1`.
3. Traverse from left to right.
4. Store the current `left` product in `answer[i]`.
5. Multiply `left` by `nums[i]`.
6. Initialize `right = 1`.
7. Traverse from right to left.
8. Multiply `answer[i]` by `right`.
9. Multiply `right` by `nums[i]`.
10.   Return `answer`.

---

# Dry Run — Example 1

Input:

```text
nums = [1, 2, 3, 4]
```

Initial:

```text
answer = [1, 1, 1, 1]

left = 1
```

---

## First Pass — Left Products

### Index 0

```text
nums[0] = 1
```

Current:

```text
left = 1
```

Store:

```text
answer[0] = 1
```

Answer:

```text
[1, 1, 1, 1]
```

Update:

```text
left = 1 × 1

= 1
```

---

### Index 1

Current:

```text
left = 1
```

Store:

```text
answer[1] = 1
```

Answer:

```text
[1, 1, 1, 1]
```

Update:

```text
left = 1 × 2

= 2
```

---

### Index 2

Current:

```text
left = 2
```

This represents:

```text
1 × 2
```

Store:

```text
answer[2] = 2
```

Answer:

```text
[1, 1, 2, 1]
```

Update:

```text
left = 2 × 3

= 6
```

---

### Index 3

Current:

```text
left = 6
```

This represents:

```text
1 × 2 × 3
```

Store:

```text
answer[3] = 6
```

Answer:

```text
[1, 1, 2, 6]
```

Update:

```text
left = 6 × 4

= 24
```

After the first pass:

```text
answer = [1, 1, 2, 6]
```

Each position now contains:

```text
Product of everything LEFT of that position.
```

---

# Second Pass — Right Products

Initialize:

```text
right = 1
```

Move:

```text
RIGHT → LEFT
```

---

### Index 3

```text
nums[3] = 4
```

Current:

```text
answer[3] = 6

right = 1
```

Calculate:

```text
answer[3]

=

6 × 1

=

6
```

Answer:

```text
[1, 1, 2, 6]
```

Update right:

```text
right = 1 × 4

= 4
```

---

### Index 2

Current:

```text
answer[2] = 2
```

This is the left product:

```text
1 × 2
```

Current right:

```text
4
```

Multiply:

```text
2 × 4 = 8
```

Answer:

```text
[1, 1, 8, 6]
```

Update:

```text
right = 4 × 3

= 12
```

---

### Index 1

Current left product:

```text
1
```

Current right product:

```text
12
```

Multiply:

```text
1 × 12 = 12
```

Answer:

```text
[1, 12, 8, 6]
```

Update:

```text
right = 12 × 2

= 24
```

---

### Index 0

Current left:

```text
1
```

Current right:

```text
24
```

Multiply:

```text
1 × 24 = 24
```

Answer:

```text
[24, 12, 8, 6]
```

Final answer:

```text
[24, 12, 8, 6]
```

---

# Dry Run Summary

```text
nums

[1, 2, 3, 4]
```

After left pass:

```text
[1, 1, 2, 6]
```

Meaning:

```text
Product before each index
```

Right products conceptually are:

```text
[24, 12, 4, 1]
```

Combine:

```text
[1, 1, 2, 6]

×

[24, 12, 4, 1]

=

[24, 12, 8, 6]
```

---

# Dry Run — Example 2

Input:

```text
nums = [-1, 1, 0, -3, 3]
```

Expected:

```text
[0, 0, 9, 0, 0]
```

---

## Understand It Manually First

For index `0`:

```text
Ignore -1

1 × 0 × -3 × 3

= 0
```

For index `1`:

```text
Ignore 1

-1 × 0 × -3 × 3

= 0
```

For index `2`:

```text
Ignore 0

-1 × 1 × -3 × 3
```

Calculate:

```text
-1 × 1 = -1

-1 × -3 = 3

3 × 3 = 9
```

So:

```text
answer[2] = 9
```

For index `3`:

```text
Remaining elements contain 0

↓

answer = 0
```

For index `4`:

```text
Remaining elements contain 0

↓

answer = 0
```

Final:

```text
[0, 0, 9, 0, 0]
```

The Prefix + Suffix solution handles zero automatically.

No special zero handling is required.

---

# Python Solution

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:

        n = len(nums)

        answer = [1] * n

        left = 1

        # Store product of everything before each index
        for i in range(n):

            answer[i] = left

            left *= nums[i]

        right = 1

        # Multiply product of everything after each index
        for i in range(n - 1, -1, -1):

            answer[i] *= right

            right *= nums[i]

        return answer
```

---

# Code Explanation

### Step 1

```python
n = len(nums)
```

Get the size of the array.

---

### Step 2

```python
answer = [1] * n
```

Create the output array.

Initially:

```text
[1, 1, 1, ...]
```

We use `1` because it is the identity value for multiplication.

---

### Step 3

```python
left = 1
```

`left` stores:

```text
Product of everything before the current index.
```

Initially there is nothing before index `0`, so:

```text
left = 1
```

---

### Step 4

```python
for i in range(n):
```

Traverse:

```text
LEFT → RIGHT
```

---

### Step 5

```python
answer[i] = left
```

Store the product of everything before the current index.

---

### Step 6

```python
left *= nums[i]
```

Now include the current number.

This updated value will be used by the next index.

Mental model:

```text
Give current index what came BEFORE it

↓

Then include current element

↓

Move forward
```

---

### Step 7

```python
right = 1
```

Now prepare to calculate suffix products.

Nothing exists after the last index, so:

```text
right = 1
```

---

### Step 8

```python
for i in range(n - 1, -1, -1):
```

Traverse:

```text
RIGHT → LEFT
```

---

### Step 9

```python
answer[i] *= right
```

`answer[i]` already contains:

```text
LEFT PRODUCT
```

`right` contains:

```text
RIGHT PRODUCT
```

Therefore:

```text
answer[i]

=

LEFT × RIGHT
```

which is exactly:

```text
Product of everything except nums[i]
```

---

### Step 10

```python
right *= nums[i]
```

Include the current number so it becomes part of the right product for the next index to the left.

---

# Complexity Analysis

## Time Complexity

First traversal:

```text
O(n)
```

Second traversal:

```text
O(n)
```

Therefore:

```text
O(n) + O(n)

=

O(n)
```

---

## Space Complexity

We use:

```text
left

right
```

which are constant variables.

The `answer` array does not count as extra space according to the problem.

Therefore:

```text
O(1)
```

extra space.

---

# Interview Thought Process

Do not try to magically recognize:

```text
Prefix Product + Suffix Product
```

Start from the problem.

```text
What does answer[i] need?

↓

Everything except nums[i]

↓

Brute force scans everything for every index

↓

O(n²) — too much repeated work

↓

Can I use total product?

↓

Would require division

↓

Division is forbidden

↓

Can I break "everything except me"
into smaller pieces?

↓

YES

↓

Everything LEFT of me
+
Everything RIGHT of me

↓

Since operation is multiplication:

LEFT PRODUCT × RIGHT PRODUCT

↓

Can these be calculated incrementally?

↓

YES

↓

Prefix Product + Suffix Product
```

That is how you should arrive at the solution.

---

# Recognition Pattern

Whenever a problem asks:

```text
For every index,
calculate something using
all elements except the current element
```

think about splitting the array:

```text
LEFT | CURRENT | RIGHT
```

Then ask:

```text
Can I combine information from LEFT and RIGHT?
```

Possible patterns include:

- Prefix Sum + Suffix Sum
- Prefix Product + Suffix Product
- Prefix Maximum + Suffix Maximum
- Prefix Minimum + Suffix Minimum

The broader pattern is:

```text
Precompute information from both directions.
```

---

# Key Takeaways

- Start with the obvious brute-force solution first.
- Identify what work is being repeated.
- `"Except Self"` should make you visualize:

```text
LEFT | SELF | RIGHT
```

- The current element can be excluded naturally by combining only the left and right sides.
- For this problem:

```text
answer[i]

=

LEFT PRODUCT × RIGHT PRODUCT
```

- Prefix does not always mean Prefix **Sum**.
- Prefix can store any cumulative information:

```text
Prefix Sum
Prefix Product
Prefix XOR
Prefix Maximum
Prefix Minimum
Prefix State
```

- Use:

```text
STORE first → UPDATE second
```

to ensure the current element is not included.

---

# Mental Formula

```text
Need Product Except Self
          │
          ▼
Try Brute Force
          │
          ▼
O(n²) — Repeated Work
          │
          ▼
Can Total Product Help?
          │
          ▼
Requires Division ❌
          │
          ▼
Break Around Current Index
          │
     ┌────┴────┐
     ▼         ▼
   LEFT       RIGHT
     │         │
     ▼         ▼
 Prefix      Suffix
 Product     Product
     │         │
     └────┬────┘
          ▼
   LEFT × RIGHT
          │
          ▼
      answer[i]
          │
          ▼
Optimize Storage
          │
          ▼
Use answer[] for LEFT
+
One variable for RIGHT
          │
          ▼
O(n) Time
O(1) Extra Space
```

<br/><br/><br/><br/><br>

---

# 304. Range Sum Query 2D — Immutable

> **Difficulty:** Medium  
> **Pattern:** 2D Prefix Sum  
> **Core Idea:** Precompute once → Answer every rectangle query in `O(1)`

---

# Problem

We are given a **2D matrix**.

Example:

```text
3  0  1  4  2
5  6  3  2  1
1  2  0  1  5
4  1  0  1  7
1  0  3  0  5
```

We will receive many queries.

Each query gives:

```text
row1, col1, row2, col2
```

These represent two corners of a rectangle:

```text
(row1, col1) → Top-Left

(row2, col2) → Bottom-Right
```

Our job is to return the **sum of every number inside that rectangle**.

The important requirement is:

```text
sumRegion() must work in O(1)
```

So we cannot loop through the rectangle every time.

---

# First — Understand What a Query Means

Suppose:

```text
sumRegion(1, 1, 2, 2)
```

That means:

```text
Top-left     = (1,1)

Bottom-right = (2,2)
```

Original matrix:

```text
      c0 c1 c2 c3 c4

r0     3  0  1  4  2

r1     5 [6  3] 2  1
          ↑     ↑
r2     1 [2  0] 1  5
          ↑     ↑

r3     4  1  0  1  7

r4     1  0  3  0  5
```

The selected rectangle is:

```text
6  3
2  0
```

Sum:

```text
6 + 3 + 2 + 0

=

11
```

Therefore:

```text
sumRegion(1,1,2,2)

=

11
```

That's all the problem is asking.

The difficulty is making every query:

```text
O(1)
```

---

# Key Observation

The problem says:

```text
At most 10⁴ calls will be made to sumRegion()
```

Imagine calculating every rectangle manually.

For every query:

```text
Visit every row

↓

Visit every column

↓

Add every element
```

We would repeatedly calculate sums over overlapping regions.

That should make us ask:

> Can we calculate cumulative information once and reuse it for every query?

This is exactly the idea behind:

```text
2D Prefix Sum
```

---

# Step 1 — Start With the Brute Force Thinking

Suppose the query asks for:

```text
row1 = 1
col1 = 1

row2 = 2
col2 = 2
```

We could simply do:

```python
total = 0

for r in range(row1, row2 + 1):

    for c in range(col1, col2 + 1):

        total += matrix[r][c]
```

This works.

But if the rectangle is large, one query may take:

```text
O(m × n)
```

And we may have:

```text
10⁴ queries
```

Too much repeated work.

---

# Step 2 — Remember Normal 1D Prefix Sum

Before understanding 2D Prefix Sum, remember the 1D idea.

Suppose:

```text
nums = [2, 4, 3, 5]
```

We can create:

```text
prefix = [0, 2, 6, 9, 14]
```

Notice the extra leading `0`.

```text
prefix[0] = 0

prefix[1] = 2

prefix[2] = 2 + 4 = 6

prefix[3] = 2 + 4 + 3 = 9

prefix[4] = 2 + 4 + 3 + 5 = 14
```

To find the sum from index `1` to `3`:

```text
4 + 3 + 5
```

we calculate:

```text
prefix[4] - prefix[1]

=

14 - 2

=

12
```

Mental idea:

```text
Everything until the end

-

Everything before the start
```

2D Prefix Sum uses the same idea.

But now we have:

```text
ROWS + COLUMNS
```

So instead of removing one unwanted prefix, we must remove unwanted **rectangular regions**.

---

# Step 3 — What Does 2D Prefix Sum Store?

We create:

```text
prefix
```

where:

```text
prefix[r][c]
```

stores the sum of the rectangle from the **top-left of the matrix** up to that position.

Conceptually:

```text
(0,0)
  ┌───────────────┐
  │               │
  │   EVERYTHING  │
  │               │
  │           (r,c)
  └───────────────┘
```

So every prefix cell remembers:

> The sum of everything above me and to my left, including the corresponding matrix region.

---

# Step 4 — Why Create an Extra Row and Column?

Instead of creating:

```text
m × n
```

prefix matrix,

we create:

```text
(m + 1) × (n + 1)
```

Example:

Original:

```text
1 2
3 4
```

Prefix structure:

```text
0 0 0
0 ? ?
0 ? ?
```

The first row and first column contain zeros.

This makes formulas much easier.

We don't need special cases such as:

```text
if row == 0

if col == 0
```

The extra zero boundary handles them automatically.

---

# Step 5 — Understand How to Build One Prefix Cell

Suppose we have this small matrix:

```text
1  2
3  4
```

We want the prefix sum containing the whole matrix:

```text
1 + 2 + 3 + 4 = 10
```

Think about the current cell:

```text
4
```

To calculate everything up to `4`, we need:

```text
TOP region

+

LEFT region

+

CURRENT cell
```

But there is a problem.

The top and left regions overlap.

So we must subtract that overlap once.

This gives:

```text
Prefix

=

TOP

+

LEFT

-

OVERLAP

+

CURRENT
```

This is the fundamental 2D Prefix Sum formula.

---

# Mathematical Formula for Building Prefix

Using the extra row and column:

```python
prefix[r][c] = (
    matrix[r - 1][c - 1]
    + prefix[r - 1][c]
    + prefix[r][c - 1]
    - prefix[r - 1][c - 1]
)
```

Or visually:

```text
Current Prefix

=

Top Prefix

+

Left Prefix

-

Top-Left Overlap

+

Current Matrix Value
```

---

# Step 6 — Why Do We Subtract the Overlap?

This is one of the most important concepts.

Imagine:

```text
TOP
```

already contains some area.

And:

```text
LEFT
```

also contains some area.

When we do:

```text
TOP + LEFT
```

their common region gets counted:

```text
TWICE
```

Example:

```text
Common region

↓

counted in TOP

+

counted in LEFT
```

So:

```text
2 times
```

But we only want it:

```text
1 time
```

Therefore:

```text
TOP + LEFT - OVERLAP
```

This is called:

```text
Inclusion-Exclusion
```

Mental rule:

> Add what you need, then subtract anything counted twice.

---

# Step 7 — Build a Small 2D Prefix Sum From Scratch

Take:

```text
matrix =

1  2
3  4
```

Create:

```text
prefix =

0  0  0
0  0  0
0  0  0
```

---

## Process matrix[0][0] = 1

Corresponding prefix position:

```text
prefix[1][1]
```

Formula:

```text
current
+
top
+
left
-
overlap
```

```text
1 + 0 + 0 - 0

=

1
```

Now:

```text
0  0  0
0  1  0
0  0  0
```

---

## Process matrix[0][1] = 2

```text
prefix[1][2]

=

2
+
prefix[0][2]
+
prefix[1][1]
-
prefix[0][1]
```

```text
=

2 + 0 + 1 - 0

=

3
```

Now:

```text
0  0  0
0  1  3
0  0  0
```

Meaning:

```text
prefix[1][2]

=

1 + 2

=

3
```

---

## Process matrix[1][0] = 3

```text
prefix[2][1]

=

3 + 1

=

4
```

Now:

```text
0  0  0
0  1  3
0  4  0
```

Meaning:

```text
1 + 3

=

4
```

---

## Process matrix[1][1] = 4

We calculate:

```text
prefix[2][2]

=

4
+
prefix[1][2]
+
prefix[2][1]
-
prefix[1][1]
```

Substitute:

```text
=

4 + 3 + 4 - 1

=

10
```

Why subtract `1`?

Because:

```text
3
```

represented:

```text
1 + 2
```

And:

```text
4
```

represented:

```text
1 + 3
```

So `1` was counted twice.

Subtract it once.

Final:

```text
prefix =

0  0   0
0  1   3
0  4  10
```

---

# Step 8 — Now the Real Challenge: Getting Any Rectangle

Suppose we want some rectangle inside a larger matrix.

Imagine:

```text
┌──────────────────────────┐
│                          │
│      EVERYTHING          │
│                          │
│       ┌──────────┐       │
│       │  TARGET  │       │
│       │          │       │
│       └──────────┘       │
└──────────────────────────┘
```

Our prefix gives us:

```text
Everything from the top-left
```

But we only want:

```text
TARGET
```

So we need to remove unwanted areas.

---

# Step 9 — The Rectangle Sum Formula

For query:

```text
(row1, col1)

to

(row2, col2)
```

Using our extra-row/column prefix matrix:

```python
total = (
    prefix[row2 + 1][col2 + 1]
    - prefix[row1][col2 + 1]
    - prefix[row2 + 1][col1]
    + prefix[row1][col1]
)
```

At first this formula looks scary.

Don't memorize it.

Understand where it comes from.

---

# Step 10 — Think Like a Common Person: Take Too Much First

Suppose the target is:

```text
        col1           col2
          ↓              ↓

       ┌─────────────────┐
row1 → │      TARGET     │
       │                 │
row2 → │                 │
       └─────────────────┘
```

First take:

```text
Everything from (0,0) to (row2,col2)
```

This contains our target.

But it also contains unwanted cells:

```text
ABOVE the target

LEFT of the target
```

So:

```text
TARGET

=

BIG PREFIX

-

TOP

-

LEFT
```

But there's one problem.

The top-left corner region belongs to:

```text
TOP

and

LEFT
```

So we subtracted it twice.

We must add it back once.

Therefore:

```text
TARGET

=

BIG

-

TOP

-

LEFT

+

OVERLAP
```

This is the most important formula in 2D Prefix Sum.

---

# Rectangle Sum Mental Formula

```text
ANSWER

=

WHOLE PREFIX

-

TOP

-

LEFT

+

TOP-LEFT OVERLAP
```

Or simply:

```text
BIG - TOP - LEFT + OVERLAP
```

---

# Why Do We Add the Overlap Back?

Suppose some region has value:

```text
10
```

It exists inside both:

```text
TOP

and

LEFT
```

When we calculate:

```text
BIG - TOP - LEFT
```

that region gets removed:

```text
TWICE
```

But it was originally inside `BIG` only once.

So its final contribution becomes:

```text
1 - 1 - 1

=

-1
```

We removed too much.

Add it once:

```text
1 - 1 - 1 + 1

=

0
```

Now it is correctly removed.

This is the mathematical reason for:

```text
+ overlap
```

---

# Mathematical Logic

Let:

```text
A = Big prefix rectangle

B = Area above target

C = Area left of target

D = Overlap between B and C
```

We want:

```text
Target
```

Start with:

```text
A
```

Remove top:

```text
A - B
```

Remove left:

```text
A - B - C
```

But:

```text
D
```

was inside both `B` and `C`.

Therefore it was removed twice.

Add it once:

```text
A - B - C + D
```

Therefore:

```text
Rectangle Sum

=

Big

-

Top

-

Left

+

Overlap
```

---

# Algorithm

## Constructor — `NumMatrix(matrix)`

1. Get `m` and `n`.
2. Create a `(m+1) × (n+1)` prefix matrix filled with zeros.
3. Traverse every matrix cell.
4. Build its 2D prefix using:

```text
current value

+

top

+

left

-

overlap
```

---

## Query — `sumRegion()`

For every query:

```text
(row1,col1)

to

(row2,col2)
```

Return:

```text
BIG

-

TOP

-

LEFT

+

OVERLAP
```

Since all these values are already stored:

```text
O(1)
```

---

# Dry Run — Given Test Case

Matrix:

```text
3  0  1  4  2
5  6  3  2  1
1  2  0  1  5
4  1  0  1  7
1  0  3  0  5
```

The built prefix matrix with an extra zero row and column is:

```text
0   0   0   0   0   0
0   3   3   4   8  10
0   8  14  18  24  27
0   9  17  21  28  36
0  13  22  26  34  49
0  14  23  30  38  58
```

---

# Query 1

```text
sumRegion(2, 1, 4, 3)
```

Target rectangle:

```text
2  0  1
1  0  1
0  3  0
```

Manual sum:

```text
2 + 0 + 1

+

1 + 0 + 1

+

0 + 3 + 0

=

8
```

Now use prefix.

Formula:

```text
prefix[row2+1][col2+1]

-

prefix[row1][col2+1]

-

prefix[row2+1][col1]

+

prefix[row1][col1]
```

Substitute:

```text
prefix[5][4]

-

prefix[2][4]

-

prefix[5][1]

+

prefix[2][1]
```

Values:

```text
38 - 24 - 14 + 8
```

Calculate:

```text
38 - 24 = 14

14 - 14 = 0

0 + 8 = 8
```

Answer:

```text
8
```

---

# Query 2

```text
sumRegion(1, 1, 2, 2)
```

Target:

```text
6  3
2  0
```

Manual:

```text
6 + 3 + 2 + 0

=

11
```

Prefix formula:

```text
prefix[3][3]

-

prefix[1][3]

-

prefix[3][1]

+

prefix[1][1]
```

Values:

```text
21 - 4 - 9 + 3
```

Calculate:

```text
21 - 4 = 17

17 - 9 = 8

8 + 3 = 11
```

Answer:

```text
11
```

---

# Query 3

```text
sumRegion(1, 2, 2, 4)
```

Target:

```text
3  2  1
0  1  5
```

Manual:

```text
3 + 2 + 1 + 0 + 1 + 5

=

12
```

Prefix formula:

```text
prefix[3][5]

-

prefix[1][5]

-

prefix[3][2]

+

prefix[1][2]
```

Values:

```text
36 - 10 - 17 + 3
```

Calculate:

```text
36 - 10 = 26

26 - 17 = 9

9 + 3 = 12
```

Answer:

```text
12
```

---

# Python Solution

```python
class NumMatrix:

    def __init__(self, matrix: List[List[int]]):

        m = len(matrix)
        n = len(matrix[0])

        self.prefix = [
            [0] * (n + 1)
            for _ in range(m + 1)
        ]

        for r in range(1, m + 1):

            for c in range(1, n + 1):

                self.prefix[r][c] = (
                    matrix[r - 1][c - 1]
                    + self.prefix[r - 1][c]
                    + self.prefix[r][c - 1]
                    - self.prefix[r - 1][c - 1]
                )


    def sumRegion(
        self,
        row1: int,
        col1: int,
        row2: int,
        col2: int
    ) -> int:

        return (
            self.prefix[row2 + 1][col2 + 1]
            - self.prefix[row1][col2 + 1]
            - self.prefix[row2 + 1][col1]
            + self.prefix[row1][col1]
        )
```

---

# Code Explanation

### Step 1

```python
m = len(matrix)
n = len(matrix[0])
```

Get:

```text
Number of rows

Number of columns
```

---

### Step 2

```python
self.prefix = [
    [0] * (n + 1)
    for _ in range(m + 1)
]
```

Create an extra:

```text
zero row

+

zero column
```

So if matrix is:

```text
m × n
```

prefix becomes:

```text
(m+1) × (n+1)
```

This avoids boundary-condition code.

---

### Step 3

```python
for r in range(1, m + 1):

    for c in range(1, n + 1):
```

Traverse every prefix position corresponding to an original matrix cell.

Because of the extra row and column:

```text
matrix[r-1][c-1]
```

corresponds to:

```text
prefix[r][c]
```

---

### Step 4

```python
self.prefix[r][c] = (
    matrix[r - 1][c - 1]
    + self.prefix[r - 1][c]
    + self.prefix[r][c - 1]
    - self.prefix[r - 1][c - 1]
)
```

Mental formula:

```text
CURRENT

+

TOP

+

LEFT

-

OVERLAP
```

Why subtract overlap?

Because:

```text
TOP + LEFT
```

counts it twice.

---

### Step 5

For the query:

```python
self.prefix[row2 + 1][col2 + 1]
```

Take the entire big prefix ending at the bottom-right of our target.

---

### Step 6

```python
- self.prefix[row1][col2 + 1]
```

Remove everything above the target.

---

### Step 7

```python
- self.prefix[row2 + 1][col1]
```

Remove everything to the left of the target.

---

### Step 8

```python
+ self.prefix[row1][col1]
```

Add back the top-left overlap because we removed it twice.

Final mental formula:

```text
BIG

-

TOP

-

LEFT

+

OVERLAP
```

---

# Complexity Analysis

## Building the Prefix Matrix

We visit every matrix cell once.

```text
O(m × n)
```

---

## sumRegion()

Only four prefix values are accessed.

```text
BIG

TOP

LEFT

OVERLAP
```

Therefore:

```text
O(1)
```

This is exactly what the problem requires.

---

## Space Complexity

Prefix matrix:

```text
(m+1) × (n+1)
```

Therefore:

```text
O(m × n)
```

---

# Interview Thought Process

Don't start by trying to remember the 2D Prefix Sum formula.

Start from the problem.

```text
What is being asked?

↓

Rectangle Sum

↓

How often?

↓

Many queries

↓

Brute force scans every rectangle repeatedly

↓

Repeated work

↓

Can I precompute cumulative information?

↓

Yes

↓

2D Prefix Sum
```

Then derive the query:

```text
Start with a big prefix containing target

↓

Remove area ABOVE

↓

Remove area LEFT

↓

Oops — top-left overlap removed twice

↓

Add overlap back once

↓

BIG - TOP - LEFT + OVERLAP
```

This is much better than memorizing four random array accesses.

---

# Recognition Pattern

Whenever you see:

- Multiple rectangle sum queries
- Submatrix sum
- Sum of a rectangular region
- Matrix is not changing
- Many queries on the same matrix
- Query must be very fast

Think:

```text
2D Prefix Sum
```

Especially when the problem says:

```text
Matrix is Immutable
```

That means:

```text
Matrix does not change
```

So it is perfect for:

```text
Precompute once

↓

Reuse many times
```

---

# Key Takeaways

- 2D Prefix Sum is the matrix version of normal Prefix Sum.
- Preprocessing takes:

```text
O(m × n)
```

- Every rectangle query becomes:

```text
O(1)
```

- Use an extra zero row and column to simplify boundary handling.

To build prefix:

```text
CURRENT + TOP + LEFT - OVERLAP
```

To query a rectangle:

```text
BIG - TOP - LEFT + OVERLAP
```

The `+ overlap` is not a trick to memorize.

It comes from **inclusion-exclusion**:

```text
We subtract TOP

We subtract LEFT

Overlap belonged to both

So overlap was subtracted twice

Therefore add it back once
```

---

# Mental Formula

```text
Many Rectangle Sum Queries
             │
             ▼
Brute Force Repeats Work
             │
             ▼
Can We Precompute?
             │
             ▼
       2D Prefix Sum
             │
      ┌──────┴───────┐
      ▼              ▼
BUILD PREFIX      QUERY RECTANGLE
      │              │
      ▼              ▼
TOP + LEFT        Start with BIG
+ CURRENT             │
- OVERLAP              ▼
                   Remove TOP
                       │
                       ▼
                   Remove LEFT
                       │
                       ▼
               Overlap Removed Twice
                       │
                       ▼
                 Add It Back
                       │
                       ▼
             BIG - TOP - LEFT
                 + OVERLAP
                       │
                       ▼
                   O(1) Query
```

---

# One-Line Memory Trick

```text
Building 2D Prefix:

CURRENT + TOP + LEFT - OVERLAP


Getting Rectangle:

BIG - TOP - LEFT + OVERLAP
```

But remember:

> **Understand the overlap logic instead of blindly memorizing the formulas.**

If you forget the formula during an interview, draw the rectangles and ask:

```text
What did I include?

What should I remove?

What did I accidentally remove twice?
```

You can derive the formula again from scratch.
