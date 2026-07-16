# Prefix Sum Problems

Welcome to the prefix sum problems section! Here you will find various data structure and algorithm problems related to Prefix Sum, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [1371. Find the Longest Substring Containing Vowels in Even Counts](#1371-find-the-longest-substring-containing-vowels-in-even-counts)
- [1314. Matrix Block Sum](#1314-matrix-block-sum)
- [3152. Special Array II](#3152-special-array-ii)

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
|--------|--------|
| 0 | Even |
| 1 | Odd |
| 2 | Even |
| 3 | Odd |
| 4 | Even |

Represent parity using one bit.

```
0 → Even
1 → Odd
```

---

# Step 2 — Represent Every Vowel Using One Bit

Assign one bit to each vowel.

| Vowel | Bit Position |
|--------|--------------|
| a | 0 |
| e | 1 |
| i | 2 |
| o | 3 |
| u | 4 |

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

| Character | Mask |
|------------|------|
| Start | 00000 |
| a | 00001 |
| e | 00011 |
| a | 00010 |
| i | 00110 |

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

- 303. Range Sum Query
- 304. Range Sum Query 2D
- 724. Find Pivot Index
- 560. Subarray Sum Equals K
- 525. Contiguous Array
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