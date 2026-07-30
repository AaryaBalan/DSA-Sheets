# Dynamic Programming (DP) - Basics

# What is Dynamic Programming?

Dynamic Programming (DP) is an optimization technique used when a recursive solution solves the **same subproblems multiple times**.

Instead of solving them repeatedly, DP stores the answer the first time and reuses it later.

---

# When should you think about DP?

DP is useful when a problem has:

1. **Overlapping Subproblems**
2. **Optimal Substructure** (covered in later lectures)

This lecture mainly focuses on **Overlapping Subproblems**.

---

# Two Ways to Solve DP Problems

## 1. Memoization (Top-Down)

- Starts from the original problem.
- Uses recursion.
- Stores already computed answers.
- Avoids recomputation.

```
Problem
   ↓
Recursive Calls
   ↓
Store Answer
   ↓
Reuse Stored Answer
```

---

## 2. Tabulation (Bottom-Up)

- Starts from the base cases.
- Builds the answer iteratively.
- No recursion.

```
Base Cases
    ↓
Build Smaller Answers
    ↓
Reach Final Answer
```

---

# General DP Workflow

For every DP problem:

```
Recursion
      ↓
Memoization
      ↓
Tabulation
      ↓
Space Optimization
```

This is the complete DP pipeline followed throughout the series.

---

# Example Problem

Find the nth Fibonacci Number.

Sequence

```
0
1
1
2
3
5
8
13
21
...
```

Recurrence Relation

```
F(n) = F(n-1) + F(n-2)
```

Base Cases

```
F(0) = 0
F(1) = 1
```

---

# Recursive Solution

```python
def fib(n):
    if n <= 1:
        return n

    return fib(n - 1) + fib(n - 2)
```

---

# Why is recursion slow?

Consider

```
F(5)
```

Recursion Tree

```
                F(5)
             /        \
          F(4)        F(3)
         /    \      /    \
      F(3)   F(2)  F(2)  F(1)
      /  \
   F(2) F(1)
```

Notice

```
F(3) computed multiple times

F(2) computed multiple times

F(1) computed multiple times
```

This is unnecessary work.

---

# Overlapping Subproblems

A subproblem that gets solved repeatedly.

Example

```
F(2)

appears here

F(4)
  |
 F(2)

and again

F(3)
  |
 F(2)
```

Since

```
F(2)

always equals

1
```

there is no reason to compute it again.

---

# Memoization

Idea:

Store every computed answer.

Whenever the same problem appears,

return the stored value immediately.

Example DP Array

```
Index

0 1 2 3 4 5

DP

0 1 1 2 3 5
```

Initially

```
-1 -1 -1 -1 -1 -1
```

Whenever a value is computed

store it.

Example

```
DP[2]=1

DP[3]=2

DP[4]=3

DP[5]=5
```

---

# Converting Recursion into Memoization

## Step 0

Create a DP array.

```python
dp = [-1] * (n + 1)
```

---

## Step 1

Before solving,

check whether the answer already exists.

```python
if dp[n] != -1:
    return dp[n]
```

---

## Step 2

Compute the answer.

```python
dp[n] = fib(n - 1, dp) + fib(n - 2, dp)
```

---

## Step 3

Return it.

```python
return dp[n]
```

---

# Memoization Code

```python
def fib(n, dp):

    if n <= 1:
        return n

    if dp[n] != -1:
        return dp[n]

    dp[n] = fib(n - 1, dp) + fib(n - 2, dp)

    return dp[n]


n = 5
dp = [-1] * (n + 1)

print(fib(n, dp))
```

---

# Time Complexity

Every state is computed only once.

```
Time Complexity

O(N)
```

---

# Space Complexity

DP Array

```
O(N)
```

Recursion Stack

```
O(N)
```

Total

```
O(N) + O(N)
```

---

# Tabulation

Instead of solving recursively,

start from the base cases.

Base Cases

```python
dp[0] = 0
dp[1] = 1
```

Then compute

```
dp[2]

dp[3]

dp[4]

...

dp[n]
```

using the recurrence.

---

# Tabulation Code

```python
def fib(n):

    if n <= 1:
        return n

    dp = [0] * (n + 1)

    dp[0] = 0
    dp[1] = 1

    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]

    return dp[n]


print(fib(5))
```

---

# Time Complexity

```
O(N)
```

---

# Space Complexity

Only the DP array.

```
O(N)
```

No recursion stack.

---

# Can we optimize further?

Observe

```
dp[i]
```

depends only on

```
dp[i-1]

dp[i-2]
```

Nothing else is required.

So storing the whole array is unnecessary.

---

# Space Optimization

Instead of an array,

store only

```
previous2

previous
```

Initially

```
previous2 = 0

previous = 1
```

For every iteration

```
current = previous + previous2

previous2 = previous

previous = current
```

Finally

```
previous

contains the answer.
```

---

# Space Optimized Code

```python
def fib(n):

    if n <= 1:
        return n

    previous2 = 0
    previous = 1

    for i in range(2, n + 1):

        current = previous + previous2

        previous2 = previous

        previous = current

    return previous


print(fib(5))
```

---

# Complexity

Time

```
O(N)
```

Space

```
O(1)
```

---

# DP Progression

```
Recursive Solution

↓

Memoization

↓

Tabulation

↓

Space Optimization
```

This is the standard process followed for most DP problems.

---

# Key Observations

✔ Learn recursion first.

✔ Identify overlapping subproblems.

✔ Store computed answers.

✔ Convert recursion to memoization.

✔ Convert memoization to tabulation.

✔ Observe dependencies.

✔ Reduce unnecessary space.

---

# Quick Revision

## Memoization

- Top-Down
- Uses recursion
- Stores answers
- O(N) Time
- O(N) Space + recursion stack

---

## Tabulation

- Bottom-Up
- Iterative
- No recursion
- O(N) Time
- O(N) Space

---

## Space Optimized DP

- Uses only required previous states
- O(N) Time
- O(1) Space

---

# Python DP Templates

## Recursive Template

```python
def solve(state):

    if base_case:
        return answer

    return solve(next_state1) + solve(next_state2)
```

---

## Memoization Template

```python
def solve(state, dp):

    if base_case:
        return answer

    if dp[state] != -1:
        return dp[state]

    dp[state] = ...

    return dp[state]
```

---

## Tabulation Template

```python
dp = [0] * (n + 1)

# Fill base cases

for i in range(...):
    dp[i] = ...

print(dp[n])
```

---

## Space Optimization Template

```python
prev2 = ...
prev1 = ...

for i in range(...):

    current = ...

    prev2 = prev1
    prev1 = current

print(prev1)
```

---

# Golden Rule

Whenever a recursive solution computes the same subproblem multiple times,

**think Dynamic Programming.**