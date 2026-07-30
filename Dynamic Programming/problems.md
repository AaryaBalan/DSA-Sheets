# Dynamic Programming Problems

Welcome to the Dynamic Programming (DP) problems section! Here you will find various Data Structures and Algorithms problems related to **Dynamic Programming**, along with detailed explanations, intuition, recursion, memoization, tabulation, space optimization, dry runs, and optimal solutions.

The goal is not just to memorize solutions, but to **develop the intuition** to identify DP patterns and solve unseen interview problems confidently.

---

# Questions

- [#70. Climbing Stairs](#70-climbing-stairs)
- [#198. House Robber](#198-house-robber)
- [#213. House Robber II](#213-house-robber-ii)

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

<br/><br/><br/><br/><br/>

---

# 198. House Robber

## Difficulty

Medium

## Pattern

- Dynamic Programming
- 1D DP
- Pick / Not Pick Pattern

---

# Problem Statement

You are a professional robber planning to rob houses along a street.

Each house contains some amount of money.

The only rule is:

> If you rob two adjacent houses, the police will be alerted.

Your task is to find the **maximum amount of money** you can rob without robbing two adjacent houses.

---

# Examples

## Example 1

```
Input

nums = [1,2,3,1]
```

Possible Choices

```
Rob House 1 + House 3

1 + 3 = 4
```

Answer

```
4
```

---

## Example 2

```
Input

nums = [2,7,9,3,1]
```

Possible Choices

```
2 + 9 + 1 = 12
```

Answer

```
12
```

---

# Understanding the Problem Like a Common Man

Imagine there are houses in a straight line.

```
🏠   🏠   🏠   🏠   🏠

 2    7    9    3    1
```

You are a robber.

If you rob one house,

its neighboring houses immediately become unsafe.

For example,

if you rob

```
7
```

you cannot rob

```
2

or

9
```

So every time you stand at a house, you only have **two choices**.

---

# Two Choices

## Choice 1

Rob this house.

If you rob it,

you must skip the next house.

---

## Choice 2

Skip this house.

Move to the next one.

That's all.

Every house only gives these two choices.

This is why this problem belongs to the **Pick / Not Pick** DP pattern.

---

# Thinking Like a Common Person

Suppose

```
nums = [2,7,9,3,1]
```

Start from the last house.

```
1
```

Can I rob it?

Yes.

But if I rob it,

I cannot rob

```
3
```

If I don't rob it,

I can consider

```
3
```

So every decision depends on two future possibilities.

This is exactly how recursion thinks.

---

# The Most Important Intuition

Whenever you are standing at index `i`, ask yourself:

## Option 1

Rob this house.

Money earned

```
nums[i]
```

Since the next house cannot be robbed,

go to

```
i-2
```

Total

```
nums[i] + solve(i-2)
```

---

## Option 2

Skip this house.

Move to

```
i-1
```

Total

```
solve(i-1)
```

Now choose whichever gives more money.

---

# Recurrence Relation

```
dp[i]

=

max(

nums[i] + dp[i-2],

dp[i-1]

)
```

This is the heart of the problem.

---

# Finding the Base Cases

Suppose

```
Only one house
```

Example

```
[5]
```

Obviously

```
Maximum Money = 5
```

So

```
dp[0]=nums[0]
```

---

If

```
index < 0
```

There is no house.

Return

```
0
```

---

# Recursive Thinking

Suppose

```
nums=[2,7,9,3]
```

Need answer for

```
House 3
```

Two options

```
Take House 3

↓

3 + solve(1)
```

or

```
Don't Take House 3

↓

solve(2)
```

Again

```
solve(2)
```

will make two choices.

Again

```
solve(1)
```

will make two choices.

Eventually every path reaches the base case.

---

# Recursion Tree

```
                    f(3)
                 /        \
           Take           Skip
             |              |
        3+f(1)           f(2)
         /  \            /   \
     Take Skip      Take   Skip
```

Notice

```
f(1)

gets calculated again.

f(0)

gets calculated again.
```

These are

```
Overlapping Subproblems
```

Hence

Dynamic Programming.

---

# Recursive Solution

```python
class Solution:

    def solve(self,index,nums):

        if index==0:
            return nums[0]

        if index<0:
            return 0

        take=nums[index]+self.solve(index-2,nums)

        notTake=self.solve(index-1,nums)

        return max(take,notTake)

    def rob(self,nums):

        return self.solve(len(nums)-1,nums)
```

---

# Why Recursion is Slow

Suppose

```
100 houses
```

The same states

```
solve(70)

solve(50)

solve(30)
```

are computed many times.

Huge waste.

Time Complexity

```
O(2^n)
```

---

# Memoization

Idea

Whenever a state is solved,

store its answer.

If the same state appears again,

return the stored answer.

No need to solve again.

---

# DP Array

Initially

```
-1 -1 -1 -1 -1
```

Suppose

```
dp[2]=11
```

Store it.

Next time

Need

```
dp[2]
```

Already computed.

Return immediately.

---

# Memoization Code

```python
class Solution:

    def solve(self,index,nums,dp):

        if index==0:
            return nums[0]

        if index<0:
            return 0

        if dp[index]!=-1:
            return dp[index]

        take=nums[index]+self.solve(index-2,nums,dp)

        notTake=self.solve(index-1,nums,dp)

        dp[index]=max(take,notTake)

        return dp[index]

    def rob(self,nums):

        n=len(nums)

        dp=[-1]*n

        return self.solve(n-1,nums,dp)
```

---

# Dry Run (Memoization)

Example

```
nums=[2,7,9,3,1]
```

```
solve(4)

↓

Take

1+solve(2)

↓

1+11=12
```

Skip

```
solve(3)=11
```

Choose

```
max(12,11)

=

12
```

Answer

```
12
```

---

# Time Complexity

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

Instead of solving backwards,

build the answer from the beginning.

We already know

```
dp[0]=nums[0]
```

Now compute

```
dp[1]

dp[2]

dp[3]

...

dp[n-1]
```

---

# Transition

```
take

=

nums[i]

+

dp[i-2]
```

If

```
i<2
```

take only

```
nums[i]
```

Skip

```
dp[i-1]
```

Store

```
max(take,skip)
```

---

# Tabulation Code

```python
class Solution:

    def rob(self,nums):

        n=len(nums)

        if n==1:
            return nums[0]

        dp=[0]*n

        dp[0]=nums[0]

        for i in range(1,n):

            take=nums[i]

            if i>1:
                take+=dp[i-2]

            skip=dp[i-1]

            dp[i]=max(take,skip)

        return dp[n-1]
```

---

# Dry Run (Tabulation)

Example

```
nums=[2,7,9,3,1]
```

Initially

```
dp

[2,0,0,0,0]
```

i=1

```
take=7

skip=2

dp[1]=7
```

```
[2,7,0,0,0]
```

---

i=2

```
take=9+2=11

skip=7

dp[2]=11
```

```
[2,7,11,0,0]
```

---

i=3

```
take=3+7=10

skip=11

dp[3]=11
```

```
[2,7,11,11,0]
```

---

i=4

```
take=1+11=12

skip=11

dp[4]=12
```

Final

```
[2,7,11,11,12]
```

Answer

```
12
```

---

# Space Optimization

Observe carefully.

```
dp[i]

depends only on

dp[i-1]

and

dp[i-2]
```

Nothing else.

So storing the whole DP array is unnecessary.

Only keep

```
prev2

prev
```

---

Initially

```
prev2=0

prev=nums[0]
```

Every iteration

```
current

=

max(

nums[i]+prev2,

prev

)
```

Move the variables

```
prev2=prev

prev=current
```

Finally

```
prev

contains the answer.
```

---

# Space Optimized Code

```python
class Solution:

    def rob(self,nums):

        n=len(nums)

        if n==1:
            return nums[0]

        prev=nums[0]

        prev2=0

        for i in range(1,n):

            take=nums[i]

            if i>1:
                take+=prev2

            skip=prev

            current=max(take,skip)

            prev2=prev

            prev=current

        return prev
```

---

# Time Complexity

```
O(n)
```

---

# Space Complexity

```
O(1)
```

---

# Mathematical Logic

At every house,

there are only two possibilities.

```
Take

or

Don't Take
```

If you

```
Take
```

you must skip the adjacent house.

If you

```
Don't Take
```

you remain free to consider the next house.

Therefore

```
Answer

=

Maximum of

Take

and

Don't Take
```

which gives

```
dp[i]

=

max(

nums[i]+dp[i-2],

dp[i-1]

)
```

---

# How to Build the Intuition

Whenever you see a problem, ask:

### Question 1

Can I make a choice?

```
Take

or

Skip
```

---

### Question 2

If I take this,

what becomes impossible?

Here

```
Adjacent house
```

---

### Question 3

Can I solve the remaining smaller problem?

Yes.

```
Take

↓

solve(i-2)

Skip

↓

solve(i-1)
```

---

### Question 4

Am I solving the same smaller problems repeatedly?

Yes.

Hence

```
Dynamic Programming
```

---

# Pattern Recognition

Whenever you hear

- Maximum Sum
- Cannot Choose Adjacent
- Pick or Skip
- Independent Choices

Immediately think

```
Pick / Not Pick DP
```

---

# Common Mistakes

❌ Robbing adjacent houses.

❌ Forgetting the `index < 0` base case.

❌ Forgetting to handle `n == 1`.

❌ Using `dp[i-2]` when `i == 1`.

---

# Similar Problems

- House Robber II
- House Robber III
- Maximum Sum of Non-Adjacent Elements
- Delete and Earn
- Paint House

---

# Quick Revision

### Recurrence

```
dp[i]

=

max(

nums[i]+dp[i-2],

dp[i-1]

)
```

---

### Base Cases

```
index==0

↓

nums[0]

index<0

↓

0
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

Whenever a problem asks you to maximize something and every choice affects the next available choices, think in terms of **"Take" or "Skip."** If taking the current element prevents taking a neighboring element, there's a strong chance the problem can be solved using the **Pick / Not Pick Dynamic Programming pattern**.

<br/><br/><br/><br/><br/>

---

# 213. House Robber II

# Difficulty

Medium

# Pattern

- Dynamic Programming
- 1D DP
- Pick / Not Pick

---

# Problem Statement

You are a professional robber.

There are **N houses** arranged in a **circle**.

Each house contains some money.

The rule is:

- You **cannot rob two adjacent houses**.
- Since the houses are arranged in a **circle**, the **first and last houses are also adjacent**.

Find the **maximum money** that can be robbed.

---

# Difference from House Robber I

House Robber I

```
1 ---- 2 ---- 3 ---- 4 ---- 5
```

Only neighbors are adjacent.

---

House Robber II

```
      1
   /     \
  5       2
  |       |
  4 ----- 3
```

Now

```
First house

and

Last house

are also adjacent.
```

This is the only difference.

---

# The Biggest Observation

Can the answer ever contain

```
First House

and

Last House
```

together?

No.

Because they are adjacent.

Therefore

```
At least one of them

must be excluded.
```

This single observation solves the entire problem.

---

# Think Like a Common Man

Suppose

```
2  3  2
```

If you rob

```
First House (2)
```

you cannot rob

```
Last House (2)
```

If you rob

```
Last House (2)
```

you cannot rob

```
First House (2)
```

So both cannot exist together.

---

Suppose

```
1 2 3 1
```

Again,

```
First

and

Last

cannot both be robbed.
```

So instead of solving a circular problem,

convert it into two normal House Robber problems.

---

# The Main Intuition

Since

```
First

and

Last

cannot both be selected,
```

only two cases are possible.

---

## Case 1

Ignore the last house.

Solve

```
0 → n-2
```

Example

```
1 2 3 1

↓

1 2 3
```

Apply House Robber I.

---

## Case 2

Ignore the first house.

Solve

```
1 → n-1
```

Example

```
1 2 3 1

↓

2 3 1
```

Again,

apply House Robber I.

---

Finally

```
Answer

=

max(

Case1,

Case2

)
```

---

# Why Does This Always Work?

Suppose the optimal answer includes

```
First House.
```

Then

```
Last House

cannot be included.
```

So it belongs to

```
0 → n-2
```

---

Suppose the optimal answer includes

```
Last House.
```

Then

```
First House

cannot be included.
```

So it belongs to

```
1 → n-1
```

---

There is no third possibility.

Hence

```
Answer

=

max(

rob(nums[:-1]),

rob(nums[1:])

)
```

---

# Step 1 : Solve House Robber I

We already know

```
dp[i]

=

max(

nums[i]+dp[i-2],

dp[i-1]

)
```

Now simply call it twice.

---

# Memoization

## Idea

Store the answer for every index.

Whenever the same state appears,

reuse it.

---

## State

```
solve(index)
```

means

```
Maximum money

from

0 → index
```

---

## Base Cases

```
index==0

↓

nums[0]
```

```
index<0

↓

0
```

---

## Transition

### Pick

```
nums[index]

+

solve(index-2)
```

---

### Not Pick

```
solve(index-1)
```

---

Take maximum.

```
return max(

pick,

notPick

)
```

---

# Memoization Code

```python
from functools import cache

class Solution:

    def rob(self, nums):

        if len(nums)==1:
            return nums[0]

        def helper(arr):

            @cache
            def solve(index):

                if index==0:
                    return arr[0]

                if index<0:
                    return 0

                pick=arr[index]+solve(index-2)

                notPick=solve(index-1)

                return max(pick,notPick)

            return solve(len(arr)-1)

        return max(

            helper(nums[:-1]),

            helper(nums[1:])

        )
```

---

# Memoization Dry Run

Example

```
nums=[2,3,2]
```

Case 1

```
[2,3]

↓

Answer=3
```

Case 2

```
[3,2]

↓

Answer=3
```

Final

```
max(3,3)

=

3
```

---

# Memoization Complexity

```
Time

O(N)
```

```
Space

O(N)
```

---

# Tabulation

Instead of recursion,

build the answer from left to right.

---

## DP Definition

```
dp[i]

=

Maximum money

up to house i
```

---

## Base

```
dp[0]=nums[0]
```

---

## Formula

```
pick

=

nums[i]

+

dp[i-2]
```

```
notPick

=

dp[i-1]
```

```
dp[i]

=

max(

pick,

notPick

)
```

---

# Tabulation Code

```python
class Solution:

    def rob(self, nums):

        if len(nums)==1:
            return nums[0]

        def helper(arr):

            n=len(arr)

            dp=[0]*n

            dp[0]=arr[0]

            for i in range(1,n):

                pick=arr[i]

                if i>1:
                    pick+=dp[i-2]

                notPick=dp[i-1]

                dp[i]=max(pick,notPick)

            return dp[-1]

        return max(

            helper(nums[:-1]),

            helper(nums[1:])

        )
```

---

# Tabulation Dry Run

Example

```
nums=[1,2,3,1]
```

Case 1

```
[1,2,3]
```

DP

```
1

2

4
```

Answer

```
4
```

---

Case 2

```
[2,3,1]
```

DP

```
2

3

3
```

Answer

```
3
```

Final

```
max(

4,

3

)

=

4
```

---

# Tabulation Complexity

```
Time

O(N)
```

```
Space

O(N)
```

---

# Space Optimization

Observe

```
dp[i]

depends only on

dp[i-1]

and

dp[i-2]
```

Nothing else.

So,

instead of storing the whole DP array,

store only two variables.

---

## Variables

```
prev

↓

dp[i-1]
```

```
prev2

↓

dp[i-2]
```

---

## Transition

```
pick

=

arr[i]

+

prev2
```

```
skip

=

prev
```

```
current

=

max(

pick,

skip

)
```

Move forward

```
prev2=prev

prev=current
```

---

# Space Optimized Code

```python
class Solution:

    def rob(self, nums):

        if len(nums)==1:
            return nums[0]

        def helper(arr):

            prev=arr[0]

            prev2=0

            for i in range(1,len(arr)):

                pick=arr[i]+prev2

                skip=prev

                current=max(pick,skip)

                prev2=prev

                prev=current

            return prev

        return max(

            helper(nums[:-1]),

            helper(nums[1:])

        )
```

---

# Space Optimization Dry Run

Example

```
nums=[2,3,2]
```

Case 1

```
[2,3]
```

Initially

```
prev=2

prev2=0
```

i=1

```
pick=3

skip=2

current=3
```

Update

```
prev2=2

prev=3
```

Answer

```
3
```

---

Case 2

```
[3,2]
```

Initially

```
prev=3

prev2=0
```

i=1

```
pick=2

skip=3

current=3
```

Answer

```
3
```

Final

```
max(

3,

3

)

=

3
```

---

# Time Complexity

```
O(N)
```

because House Robber I runs twice.

```
O(N)+O(N)

=

O(N)
```

---

# Space Complexity

```
O(1)
```

---

# Interview Thinking

Whenever a problem says

```
Linear Array
```

and suddenly changes to

```
Circular Array
```

ask yourself:

> **Can I break the circle into two linear cases?**

Here,

breaking the circle into

```
Exclude First

or

Exclude Last
```

transforms the problem back into the already solved **House Robber I**.

---

# Common Mistakes

❌ Trying to rob both first and last houses.

❌ Forgetting the edge case when there is only one house.

❌ Writing a completely new DP instead of reusing House Robber I.

❌ Forgetting that `nums[:-1]` and `nums[1:]` create two separate linear problems.

---

# Similar Problems

- House Robber I
- House Robber III
- Delete and Earn
- Maximum Sum of Non-Adjacent Elements

---

# Quick Revision

## Observation

```
First

and

Last

cannot both be robbed.
```

---

## Cases

```
Case 1

↓

Ignore Last
```

```
Case 2

↓

Ignore First
```

---

## Formula

```
Answer

=

max(

HouseRobber(nums[:-1]),

HouseRobber(nums[1:])

)
```

---

# Golden Rule

Whenever a circular arrangement introduces a conflict between the first and last elements, try to **break the circle into multiple linear subproblems**. Reusing the solution to the simpler linear version is often the cleanest and most efficient dynamic programming approach.