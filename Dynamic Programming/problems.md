# Dynamic Programming Problems

Welcome to the Dynamic Programming (DP) problems section! Here you will find various Data Structures and Algorithms problems related to **Dynamic Programming**, along with detailed explanations, intuition, recursion, memoization, tabulation, space optimization, dry runs, and optimal solutions.

The goal is not just to memorize solutions, but to **develop the intuition** to identify DP patterns and solve unseen interview problems confidently.

---

# Questions

- [#70. Climbing Stairs](#70-climbing-stairs)

<br/><br/><br/><br/><br/>

---

# 70. Climbing Stairs

# Difficulty

Easy

# Pattern

- Dynamic Programming
- Fibonacci Pattern

---

# Problem Statement

You are climbing a staircase with **n steps**.

At every move, you can either:

- Climb **1 step**
- Climb **2 steps**

Find the **total number of different ways** to reach the top.

---

# Examples

## Example 1

```
Input

n = 2
```

Possible ways

```
1 + 1

2
```

Answer

```
2
```

---

## Example 2

```
Input

n = 3
```

Possible ways

```
1 + 1 + 1

1 + 2

2 + 1
```

Answer

```
3
```

---

# Understanding the Problem Like a Common Man

Imagine there is a staircase in front of you.

```
        TOP
         ↑
        Step 5
        Step 4
        Step 3
        Step 2
        Step 1
       Ground
```

Suppose your friend tells you

> "You can only climb either **1 step** or **2 steps** at a time."

Now the question is

> **How many different ways can I reach the top?**

Notice something.

You don't need to find the shortest path.

You don't need to minimize anything.

You simply need to count every possible way.

---

# Let's Think Like a Human

Suppose

```
n = 4
```

You're standing at the ground.

What choices do you have?

```
Choice 1

Take 1 step
```

or

```
Choice 2

Take 2 steps
```

That's it.

Nothing else.

Every time you're standing somewhere,

you only have these two choices.

This immediately tells us

```
The problem naturally breaks into smaller problems.
```

---

# Thinking from the End (Most Important Intuition)

Instead of thinking

> "How do I climb from Step 0?"

Think

> "How could I have reached Step n?"

Suppose

```
Top = Step 5
```

How can you reach Step 5?

There are only TWO possibilities.

---

## Possibility 1

You came from Step 4

```
4 → 5
```

because you climbed

```
1 step
```

---

## Possibility 2

You came from Step 3

```
3 → 5
```

because you climbed

```
2 steps
```

There is NO third possibility.

You cannot come from

```
Step 2

Step 1

Ground
```

because you can only climb

```
1 or 2
```

---

Therefore

```
Ways(5)

=

Ways(4)

+

Ways(3)
```

This is the entire intuition.

---

# Generalizing

For every stair

```
Ways(n)

=

Ways(n-1)

+

Ways(n-2)
```

Why?

Because

Every path reaching

```
n-1
```

can take one more step.

Every path reaching

```
n-2
```

can take two more steps.

Those are all possible paths.

---

# This Looks Familiar...

Look carefully.

```
Ways(n)

=

Ways(n-1)

+

Ways(n-2)
```

Compare with Fibonacci

```
Fib(n)

=

Fib(n-1)

+

Fib(n-2)
```

Exactly the same recurrence.

That's why this problem is called

```
Fibonacci DP
```

---

# Finding Base Cases

Every DP problem starts with

```
"What are the smallest problems whose answers I already know?"
```

---

## n = 1

```
Only one way

1
```

Answer

```
1
```

---

## n = 2

Possible

```
1+1

2
```

Answer

```
2
```

So

```
Ways(1)=1

Ways(2)=2
```

---

# Recurrence Relation

```
Ways(n)

=

Ways(n-1)

+

Ways(n-2)
```

---

# Recursive Thinking

Imagine

```
Ways(5)
```

Immediately your brain should think

```
Need

Ways(4)

and

Ways(3)
```

Then

```
Ways(4)

needs

Ways(3)

Ways(2)
```

Again

```
Ways(3)

needs

Ways(2)

Ways(1)
```

---

# Recursion Tree

```
                    5
                 /     \
               4         3
             /   \     /   \
            3     2   2     1
          /  \
         2    1
```

Notice

```
Ways(3)

computed twice

Ways(2)

computed three times
```

This is

```
Overlapping Subproblems
```

Perfect place for DP.

---

# Recursive Solution

```python
class Solution:
    def climbStairs(self, n: int) -> int:

        if n == 1:
            return 1

        if n == 2:
            return 2

        return self.climbStairs(n-1) + self.climbStairs(n-2)
```

---

# Why Recursion is Slow

Suppose

```
n=40
```

The recursion repeatedly computes

```
Ways(35)

Ways(30)

Ways(20)

...
```

again and again.

Huge waste.

Time Complexity

```
O(2^n)
```

---

# Memoization

Idea

Store every answer after computing it.

Next time

Don't compute.

Just return it.

---

# DP Array

Initially

```
-1 -1 -1 -1 -1
```

Suppose

```
Ways(2)=2
```

Store

```
dp[2]=2
```

Suppose

```
Ways(3)=3
```

Store

```
dp[3]=3
```

Next time

Need

```
Ways(3)
```

Already stored.

Return immediately.

---

# Memoization Code

```python
class Solution:

    def solve(self,n,dp):

        if n==1:
            return 1

        if n==2:
            return 2

        if dp[n]!=-1:
            return dp[n]

        dp[n]=self.solve(n-1,dp)+self.solve(n-2,dp)

        return dp[n]

    def climbStairs(self,n):

        dp=[-1]*(n+1)

        return self.solve(n,dp)
```

---

# Time Complexity

Every state computed once.

```
O(n)
```

Space

```
DP Array

+

Recursion Stack

=

O(n)
```

---

# Tabulation

Instead of

Starting from

```
n
```

Start from

```
1

2
```

and build upwards.

---

# DP Table

For

```
n=5
```

Initially

```
Step

0 1 2 3 4 5

DP

0 1 2 0 0 0
```

Now compute

```
dp[3]

=

dp[2]+dp[1]

=

2+1

=

3
```

Table

```
0 1 2 3 0 0
```

Next

```
dp[4]

=

3+2

=

5
```

```
0 1 2 3 5 0
```

Next

```
dp[5]

=

5+3

=

8
```

Final

```
0 1 2 3 5 8
```

Answer

```
8
```

---

# Tabulation Code

```python
class Solution:

    def climbStairs(self,n):

        if n<=2:
            return n

        dp=[0]*(n+1)

        dp[1]=1
        dp[2]=2

        for i in range(3,n+1):

            dp[i]=dp[i-1]+dp[i-2]

        return dp[n]
```

---

# Time Complexity

```
O(n)
```

Space

```
O(n)
```

---

# Space Optimization

Observe

```
dp[i]

depends only on

dp[i-1]

dp[i-2]
```

Nothing else.

So why store the whole array?

No need.

Just remember

```
previous

previous2
```

---

Initially

```
previous2=1

previous=2
```

For every step

```
current

=

previous

+

previous2
```

Then move forward.

```
previous2=previous

previous=current
```

---

# Dry Run

Suppose

```
n=5
```

Initially

```
previous2=1

previous=2
```

---

i=3

```
current

=

2+1

=

3

previous2=2

previous=3
```

---

i=4

```
current

=

3+2

=

5

previous2=3

previous=5
```

---

i=5

```
current

=

5+3

=

8

previous2=5

previous=8
```

Return

```
8
```

---

# Space Optimized Code

```python
class Solution:

    def climbStairs(self,n):

        if n<=2:
            return n

        previous2=1
        previous=2

        for i in range(3,n+1):

            current=previous+previous2

            previous2=previous

            previous=current

        return previous
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

# How to Build the Intuition for DP Problems

Whenever you see a problem, ask yourself these questions:

### Question 1

```
Can I reach the answer from smaller answers?
```

If yes,

DP may work.

---

### Question 2

```
What is the last move?
```

Here

Last move is

```
1 step

or

2 steps
```

That immediately gives

```
Ways(n)

=

Ways(n-1)

+

Ways(n-2)
```

---

### Question 3

```
Am I solving the same smaller problem repeatedly?
```

Yes.

```
Ways(3)

Ways(2)

Ways(3)

Ways(2)
```

Hence

```
Memoization
```

or

```
Tabulation
```

---

# Pattern Recognition

Whenever you hear

- Number of Ways
- Count Paths
- Reach Destination
- 1 step / 2 step
- Jump Problems

Immediately think

```
Dynamic Programming
```

---

# Common Mistakes

❌ Using recursion directly for large `n`.

❌ Forgetting the base cases.

❌ Using `dp[i]` before initializing `dp[1]` and `dp[2]`.

❌ Returning `current` instead of `previous` after the loop.

---

# Similar Problems

- Fibonacci Number
- Min Cost Climbing Stairs
- House Robber
- Decode Ways
- Jump Game
- Coin Change
- Unique Paths

---

# Quick Revision

### Recurrence

```
Ways(n)

=

Ways(n-1)

+

Ways(n-2)
```

---

### Base Cases

```
Ways(1)=1

Ways(2)=2
```

---

### DP Progression

```
Recursion

↓

Memoization

↓

Tabulation

↓

Space Optimization
```

---

# Golden Rule

Whenever a problem asks:

> **"How many different ways are there?"**

and every answer depends on a few **previous smaller answers**,

**think Dynamic Programming.**

For this problem, the key realization is:

> **To reach stair `n`, you must have come from either stair `n-1` or stair `n-2`. Once you discover this, the recurrence `ways(n) = ways(n-1) + ways(n-2)` naturally follows.**