# Math Logic Problems

Welcome to the Math Logic Problems section! Here you will find various data structure and algorithm problems that require mathematical observations, pattern recognition, logical reasoning, and optimization techniques.

These problems focus on understanding the hidden mathematical structure behind a problem instead of relying only on brute-force approaches. You will learn how to derive formulas, identify patterns, use number properties, and convert mathematical insights into efficient algorithms.
## Questions

- [264. Ugly Number II](#264-ugly-number-ii)
- [769. Max Chunks To Make Sorted](#769-max-chunks-to-make-sorted)
- [640. Solve the Equation](#640-Solve-the-Equation)

<br><br><br><br><br>

---

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

<br/><br/><br/><br/><br/><br/>

---


# 769. Max Chunks To Make Sorted

> **Difficulty:** Medium  
> **Technique:** Greedy + Prefix Maximum

---

# 📚 Problem Statement

You are given a permutation of numbers from

```
0 to n-1
```

For example,

```
[1,0,2,3,4]
```

contains every number exactly once.

You may split the array into several **continuous chunks**.

Example

```
[1,0] | [2] | [3] | [4]
```

Then

1. Sort each chunk individually.
2. Join all chunks together.

The final array **must become completely sorted**.

Your goal is to create the **maximum number of chunks**.

---

# Example 1

```
Input

[4,3,2,1,0]
```

Output

```
1
```

Why?

No matter where you cut,

sorting pieces separately will never produce

```
0 1 2 3 4
```

---

# Example 2

```
Input

[1,0,2,3,4]
```

Output

```
4
```

Possible partition

```
[1,0] | [2] | [3] | [4]
```

Sort individually

```
[0,1] | [2] | [3] | [4]
```

Join

```
0 1 2 3 4
```

Perfect.

---

# 🤔 First Think Like a Common Person

Forget algorithms.

Imagine numbers are students standing in the wrong order.

```
1 0 2 3 4
```

You want to divide them into classrooms.

Each classroom can rearrange students **only inside itself**.

Students **cannot move between classrooms**.

Question:

> When is it safe to close a classroom?

---

# First Observation

Suppose

```
1 0
```

Can we close this classroom?

Inside

```
1 0
```

Sorting gives

```
0 1
```

Exactly what we need.

Good.

---

Now look at

```
1 0 2
```

Sorting

```
0 1 2
```

Still good.

But

why was

```
1 0
```

already enough?

---

# Let's Think About Positions

Index

```
0 1
```

Values

```
1 0
```

Notice

Largest value

```
1
```

Largest index

```
1
```

They are equal.

Interesting.

---

# Another Example

```
2 0 1
```

Indices

```
0 1 2
```

Largest value

```
2
```

Largest index

```
2
```

Again equal.

Sorting gives

```
0 1 2
```

Works.

---

# Another Example

```
2 1
```

Indices

```
0 1
```

Largest value

```
2
```

Largest index

```
1
```

Largest value is outside.

If we cut here,

where will

```
2
```

go?

It belongs later.

Impossible.

So we cannot cut.

---

# The Biggest Observation

Whenever we reach

index

```
i
```

ask

```
Have I already seen every number
that belongs in positions 0...i ?
```

If yes,

then we can cut.

---

# How Do We Check That?

Instead of checking every number,

look only at

```
Maximum Value
```

Why?

Suppose

Current index

```
5
```

Maximum seen

```
5
```

Since the array is a permutation,

numbers are unique.

If maximum is

```
5
```

then all numbers

```
0 1 2 3 4 5
```

must already exist somewhere before index 5.

Nothing larger belongs here.

So we can safely cut.

---

# Mathematical Proof

Suppose

```
Index = i
```

Maximum seen

```
maxi
```

If

```
maxi == i
```

Then

There are exactly

```
i+1
```

positions

```
0...i
```

and exactly

```
i+1
```

numbers

```
0...i
```

Since every number is unique,

all required numbers are already inside this chunk.

Sorting will automatically place them correctly.

Therefore

```
Cut Here
```

---

# Greedy Idea

Traverse the array.

Maintain

```
Maximum value seen so far.
```

Whenever

```
Maximum == Current Index
```

create one chunk.

---

# Dry Run

Example

```
arr

1 0 2 3 4
```

---

Start

```
maxi = 0

chunks = 0
```

---

## Index 0

Value

```
1
```

Maximum

```
1
```

Compare

```
1 == 0 ?

No
```

Cannot cut.

---

## Index 1

Value

```
0
```

Maximum

```
1
```

Compare

```
1 == 1

Yes
```

Everything

```
0

1
```

already exists.

Cut.

Chunks

```
1
```

---

## Index 2

Value

```
2
```

Maximum

```
2
```

Compare

```
2 == 2

Yes
```

Cut.

Chunks

```
2
```

---

## Index 3

Value

```
3
```

Maximum

```
3
```

Cut.

Chunks

```
3
```

---

## Index 4

Value

```
4
```

Maximum

```
4
```

Cut.

Chunks

```
4
```

Answer

```
4
```

---

# Another Dry Run

```
arr

4 3 2 1 0
```

---

Start

```
maxi = 0
```

---

Index 0

```
maxi = 4
```

```
4==0 ?

No
```

---

Index 1

```
maxi = 4
```

```
4==1 ?

No
```

---

Index 2

```
4==2 ?

No
```

---

Index 3

```
4==3 ?

No
```

---

Index 4

```
4==4

Yes
```

Only now

can we cut.

Answer

```
1
```

---

# Why Does This Work?

Imagine boxes.

```
Index

0 1 2 3
```

Need numbers

```
0 1 2 3
```

If maximum seen is

```
3
```

then

all numbers

```
0

1

2

3
```

must already be inside.

No number outside can belong here.

Sorting fixes everything.

---

# Algorithm

```
maxi = 0

chunks = 0

for every index

    maxi = max(maxi, arr[i])

    if maxi == i

         chunks +=1

return chunks
```

---

# Python Solution

```python
class Solution:
    def maxChunksToSorted(self, arr):
        maxi = 0
        chunks = 0

        for i in range(len(arr)):
            maxi = max(maxi, arr[i])

            if maxi == i:
                chunks += 1

        return chunks
```

---

# Complexity Analysis

### Time

```
O(N)
```

Single traversal.

---

### Space

```
O(1)
```

Only two variables.

---

# How to Think Like an Interviewer

Most beginners think

```
Where should I cut?
```

Experienced candidates ask

```
When is it SAFE to cut?
```

This small change in thinking makes the solution much easier.

---

# The Thinking Process

Whenever you see a partition problem,

don't immediately think about cutting.

Ask yourself

```
What condition must be true
before I can safely make a cut?
```

Here,

the condition is

```
Every number that belongs
to this part
must already be inside it.
```

Then ask

```
How can I check this efficiently?
```

Instead of checking every number,

observe that the array is a permutation.

That means checking only

```
Maximum Seen
```

is enough.

---

# Why Only Maximum?

Suppose

```
Current Index = 5
```

Maximum seen

```
5
```

Since every number is unique,

the first six numbers

```
0 1 2 3 4 5
```

must already be present.

There is nothing missing.

So sorting this chunk will automatically place them correctly.

---

# Pattern Recognition

Whenever you see

```
Split Array

Partition Array

Maximum Chunks

Independent Segments
```

ask yourself

```
When is it safe to split?
```

Many greedy problems become easy after finding the **safe boundary condition**.

---

# Similar Problems

- **768. Max Chunks To Make Sorted II** (harder version)
- **763. Partition Labels**
- **915. Partition Array into Disjoint Intervals**
- **56. Merge Intervals** (boundary thinking)
- **1024. Video Stitching** (greedy partition)

---

# Interview Tips

Whenever you see

```
Partition
```

follow this checklist:

```
Can elements move across partitions?

↓

No

↓

What must already be true
before making a cut?

↓

Can I maintain that property
while scanning once?

↓

Greedy
```

---

# Key Takeaways

- Don't think **"Where do I cut?"**
- Think **"When is it safe to cut?"**
- Convert the safety condition into a mathematical property.
- For permutations, **`max_seen == current_index`** means every required value for that chunk has already appeared.
- Once you recognize the safety condition, the greedy solution becomes a simple one-pass algorithm.

<br/><br/><br/><br/><br/>

---

# 640. Solve the Equation

---

# 1. Understanding the Problem Like a Normal Person

Suppose someone gives you an equation

```
x + 5 - 3 + x = 6 + x - 2
```

and asks

> "What is the value of x?"

Normally in mathematics, what do we do?

We move every **x** to one side.

We move every **number** to the other side.

Then solve.

This problem is asking us to make a computer do exactly that.

---

## Example

```
x + 5 - 3 + x = 6 + x - 2
```

Simplify each side.

Left

```
x + x + 5 - 3
= 2x + 2
```

Right

```
6 + x - 2
= x + 4
```

Now

```
2x + 2 = x + 4
```

Move x to left.

```
x + 2 = 4
```

Move numbers to right.

```
x = 2
```

Answer

```
x=2
```

---

# 2. What Makes This Problem Difficult?

The equation is given as a string.

Instead of

```
2x + 5 = 9
```

we get

```
"2x+5=9"
```

The computer only sees characters.

```
'2'
'x'
'+'
'5'
'='
'9'
```

So we first need to understand the string.

---

# 3. How Should a Human Think?

Forget coding.

Imagine you're reading the equation.

```
2x+5-x+7=9+x
```

What do you naturally do?

You mentally separate things into two groups.

### Group 1

Terms containing x

```
2x
-x
+x
```

### Group 2

Normal numbers

```
5
7
9
```

That's literally all we need.

---

# 4. The Biggest Observation

Every equation can be written as

```
(ax + b) = (cx + d)
```

For example

```
3x + 7 = 5x - 2
```

Here

```
a = 3
b = 7

c = 5
d = -2
```

We don't actually care about the original expression.

We only care about

* total x coefficient
* total constant

---

# 5. What Information Do We Need?

Only two numbers.

```
Total coefficient of x

Total constant number
```

For example

```
2x+5-3+x
```

becomes

```
3x + 2
```

Store

```
xCoefficient = 3

constant = 2
```

That's it.

---

# 6. The Main Trick

Instead of simplifying each side separately,

we move everything to the left while reading.

Suppose

```
2x+5=x+9
```

Move everything to left.

```
2x+5-x-9=0

x-4=0
```

Now we only need

```
x coefficient = 1

constant = -4
```

Then

```
x = 4
```

---

# 7. How Do We Read the String?

Look at

```
2x+5-x+7
```

Read character by character.

```
2

2x

+

5

-

x

+

7
```

Whenever a term finishes,

store it.

Exactly like reading words in a sentence.

---

# 8. Understanding Every Possible Term

There are only four possibilities.

### Case 1

```
2x
```

Coefficient

```
2
```

---

### Case 2

```
x
```

Means

```
1x
```

Coefficient

```
1
```

---

### Case 3

```
-x
```

Means

```
-1x
```

Coefficient

```
-1
```

---

### Case 4

```
15
```

Just a number.

Add to constant.

---

# 9. Why Do We Flip Signs After '='?

Suppose

```
2x+5=x+9
```

Normally

```
2x+5=x+9
```

Move right side left.

```
2x+5-x-9=0
```

Notice

```
+x
```

became

```
-x
```

and

```
+9
```

became

```
-9
```

So after crossing '='

every sign changes.

That's why your code does

```python
lhsAns = -lhsAns
xCoff = -xCoff
```

Very clever.

It makes every future value automatically act like it's moving to the left.

---

# 10. Dry Run

Equation

```
x+5-3+x=6+x-2
```

Initially

```
xCoeff = 0

constant = 0

sign = +
```

---

Read

```
x
```

Means

```
1x
```

```
xCoeff = 1
```

---

Read

```
+
```

Nothing happens.

---

Read

```
5
```

Store number

```
constant = 5
```

---

Read

```
-
```

Next sign becomes negative.

---

Read

```
3
```

Store

```
constant = 2
```

because

```
5-3
```

---

Read

```
+
```

---

Read

```
x
```

```
xCoeff = 2
```

Current equation

```
2x+2
```

---

Read

```
=
```

Everything after '=' should move left.

So

```
constant = -2

xCoeff = -2
```

---

Now read

```
6
```

```
constant = 4
```

because

```
-2+6
```

---

Read

```
+x
```

```
xCoeff = -1
```

because

```
-2+1
```

---

Read

```
-2
```

```
constant = 2
```

Final

```
-1x + 2 = 0
```

Solve

```
-x + 2 = 0

-x = -2

x = 2
```

Correct.

---

# 11. The Mathematics

Every equation eventually becomes

```
ax+b=0
```

Move b

```
ax=-b
```

Divide by a

```
x=-b/a
```

So if

```
xCoeff = a

constant = b
```

Answer

```python
x = -constant // xCoeff
```

Exactly what your code does.

---

# 12. Special Cases

## Infinite Solutions

Example

```
x=x
```

Move left

```
0x+0=0
```

Means

```
0=0
```

Always true.

Answer

```
Infinite solutions
```

---

## No Solution

Example

```
x=x+5
```

Move left

```
0x-5=0
```

Means

```
-5=0
```

Impossible.

Answer

```
No solution
```

---

## Normal Solution

Example

```
2x=x
```

Move left

```
x=0
```

Answer

```
x=0
```

---

# 13. Building the Algorithm From Scratch

```
Start

↓

Read equation from left to right

↓

Keep current sign

↓

Whenever a number or x finishes

↓

If it has x
    add coefficient

Else
    add constant

↓

When '=' appears

Flip both totals

↓

Continue

↓

End

↓

Equation becomes

ax+b=0

↓

Solve

x=-b/a
```

---

# My Solution

```python
class Solution:
    def solveEquation(self, equation: str) -> str:
        num = ""
        sign = 1
        xCoff = 0
        lhsAns = 0
        for i in equation:
            if i == "=":
                if  num != "":
                    lhsAns += int(num) * sign
                    num = ''
                lhsAns = -lhsAns
                xCoff = -xCoff
                sign = 1
            if not i.isdigit():
                if num != "" and i != 'x':
                    lhsAns += int(num) * sign
                if i == '-':
                    sign = -1
                if i == '+':
                    sign = 1
                if num != "" and i == 'x':
                    xCoff += sign * int(num)
                if num == "" and i == 'x':
                    xCoff += sign
                num = ""
            if i.isdigit():
                num += i
        if num != "":
            lhsAns += int(num) * sign
        

        if xCoff == 0 and lhsAns == 0:
            return "Infinite solutions"
        if xCoff == 0:
            return "No solution"
        ans = lhsAns//xCoff * -1
        return "x=" + str(ans)
        

```

# 14. Cleaner Python Solution

A common interview approach is to write a helper that parses one side of the equation and returns `(x_coefficient, constant)`.

```python
class Solution:
    def solveEquation(self, equation: str) -> str:

        def parse(expr):
            i = 0
            xCoeff = 0
            constant = 0
            sign = 1

            while i < len(expr):

                if expr[i] == '+':
                    sign = 1
                    i += 1

                elif expr[i] == '-':
                    sign = -1
                    i += 1

                else:
                    num = 0
                    hasNum = False

                    while i < len(expr) and expr[i].isdigit():
                        num = num * 10 + int(expr[i])
                        hasNum = True
                        i += 1

                    if i < len(expr) and expr[i] == 'x':
                        if not hasNum:
                            num = 1
                        xCoeff += sign * num
                        i += 1
                    else:
                        constant += sign * num

            return xCoeff, constant

        left, right = equation.split('=')

        leftX, leftConst = parse(left)
        rightX, rightConst = parse(right)

        xCoeff = leftX - rightX
        constant = leftConst - rightConst

        if xCoeff == 0 and constant == 0:
            return "Infinite solutions"

        if xCoeff == 0:
            return "No solution"

        return f"x={-constant // xCoeff}"
```

---

# 15. How to Develop This Intuition in Future Problems

Many beginners immediately think:

> "How do I parse this complicated string?"

Experienced problem solvers first ask:

> "What information do I actually need?"

In this problem, you don't need to preserve the entire equation. You only need two accumulated values:

* the total coefficient of `x`
* the total constant

Once you recognize that, the parsing becomes much simpler because every term contributes to exactly one of those two totals.

A useful way to build this intuition for similar problems is to follow this sequence:

1. **Forget the string.** Solve the problem on paper as you learned in school.
2. **Identify what changes.** Here, terms with `x` and plain numbers are treated differently.
3. **Ask what information must be remembered.** Only two running totals are needed.
4. **Design the parser** so that every token updates one of those totals.
5. **Apply the mathematical formula** (`ax + b = 0` ⇒ `x = -b / a`) at the end.

This approach—reducing a seemingly complex input into a few key state variables—is a common pattern in parsing, simulation, and many interview problems.
