# Dynamic Programming Problems

Welcome to the Dynamic Programming (DP) problems section! Here you will find various Data Structures and Algorithms problems related to **Dynamic Programming**, along with detailed explanations, intuition, recursion, memoization, tabulation, space optimization, dry runs, and optimal solutions.

The goal is not just to memorize solutions, but to **develop the intuition** to identify DP patterns and solve unseen interview problems confidently.

---

# Questions

- [#70. Climbing Stairs](#70-climbing-stairs)
- [#198. House Robber](#198-house-robber)
- [#213. House Robber II](#213-house-robber-ii)
- [740. Delete and Earn](#740-delete-and-earn)
- [3186. Maximum Total Damage With Spell Casting](#3186-maximum-total-damage-with-spell-casting)
- [55. Jump Game](#55-jump-game)

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

<br/><br/><br/><br/><br/>

---

# 740. Delete and Earn

## Difficulty

Medium

## Pattern

- Dynamic Programming
- 1D DP
- House Robber Transformation
- Pick / Not Pick Pattern

---

# Problem Statement

You are given an integer array `nums`.

You can perform the following operation any number of times.

Choose any number `x`.

When you choose it,

- You earn `x` points.
- Every occurrence of `x - 1` is deleted.
- Every occurrence of `x + 1` is deleted.

Your goal is to maximize the total points earned.

---

# Examples

## Example 1

### Input

```text
nums = [3,4,2]
```

### Explanation

Choose `4`

```
Earn = 4
```

Now `3` disappears.

Remaining array

```
[2]
```

Choose `2`

```
Earn = 2
```

Total

```
4 + 2 = 6
```

Answer

```
6
```

---

## Example 2

### Input

```text
nums = [2,2,3,3,3,4]
```

Instead of taking `2`s,

take every `3`.

```
3 + 3 + 3 = 9
```

All `2`s and `4` disappear.

Answer

```
9
```

---

# Understanding the Problem Like a Common Man

At first, the problem looks confusing because it talks about deleting numbers.

But ask yourself,

**What actually matters?**

Suppose

```
nums = [2,2,3,3,3,4]
```

You have

```
2 appears 2 times

3 appears 3 times

4 appears 1 time
```

Now think.

If you decide to choose `3`,

will choosing one `3` delete the other `3`s?

**No.**

It only deletes

```
2

and

4
```

That means

if you decide to take value `3`,

you should take **every** `3`.

This is the first important observation.

---

# First Intuition

Instead of thinking about every element,

group equal numbers together.

Example

```
nums

=

[2,2,3,3,3,4]
```

Frequency

```
2 → 2 times

3 → 3 times

4 → 1 time
```

Now calculate the total points each value can contribute.

```
2 → 2 × 2 = 4

3 → 3 × 3 = 9

4 → 4 × 1 = 4
```

Now forget the original array.

Instead think

```
Value     Points

2         4

3         9

4         4
```

---

# Why Can We Group Equal Numbers?

Suppose

```
nums=[5,5,5]
```

If you choose one `5`,

does another `5` disappear?

No.

Only

```
4

and

6
```

disappear.

So if you decide to choose `5`,

there is absolutely no reason to leave another `5`.

You should collect all of them.

Therefore

```
points[value]

=

value × frequency
```

This is the mathematical reason behind grouping.

---

# Converting the Problem

Create a new array called

```
points
```

where

```
points[x]

=

x × frequency(x)
```

Example

```
nums=[2,2,3,3,3,4]
```

becomes

```
Index

0 1 2 3 4

Value

0 0 4 9 4
```

Now ask yourself.

Can I take both

```
2

and

3
```

No.

Can I take both

```
3

and

4
```

No.

Suddenly this becomes

# House Robber

because adjacent values cannot both be chosen.

---

# Visual Transformation

Original array

```
[2,2,3,3,3,4]
```

↓

Count frequency

```
2 → 2

3 → 3

4 → 1
```

↓

Convert into points

```
points

0 1 2 3 4

0 0 4 9 4
```

↓

Now solve

```
Maximum sum of non-adjacent values
```

Exactly House Robber.

---

# Thinking Like a Common Person

Imagine standing at value

```
3
```

You have only two choices.

---

## Choice 1

Take it.

Earn

```
9
```

But now you cannot take

```
2

or

4
```

---

## Choice 2

Skip it.

Maybe

```
2

or

4
```

gives a better answer.

These are exactly the same two choices as House Robber.

---

# The Most Important Intuition

Whenever you stand at value `i`

there are only two possibilities.

## Option 1

Take it.

Earn

```
points[i]
```

Then skip

```
i-1
```

Continue from

```
i-2
```

Total

```
points[i] + solve(i-2)
```

---

## Option 2

Don't take it.

Move to

```
i-1
```

Total

```
solve(i-1)
```

Take whichever is larger.

---

# Recurrence Relation

```
dp[i]

=

max(

points[i] + dp[i-2],

dp[i-1]

)
```

Notice.

This is **exactly** the House Robber recurrence.

Only

```
nums

↓

points
```

changed.

---

# Base Cases

If

```
i == 0
```

Return

```
points[0]
```

If

```
i < 0
```

Return

```
0
```

---

# Recursive Thinking

Suppose

```
points

=

[0,0,4,9,4]
```

Need answer for

```
4
```

Two choices

Take

```
4 + solve(2)
```

Skip

```
solve(3)
```

Again

```
solve(3)
```

creates two more choices.

Eventually recursion reaches

```
i<0
```

---

# Recursion Tree

```text
                 solve(4)
               /          \
          Take            Skip
            |               |
      4 + solve(2)       solve(3)
         /      \         /     \
      Take    Skip     Take    Skip
```

Notice

```
solve(2)

solve(1)
```

are solved multiple times.

Hence

Dynamic Programming.

---

# Recursive Solution

```python
class Solution:

    def solve(self, i, points):

        if i == 0:
            return points[0]

        if i < 0:
            return 0

        take = points[i] + self.solve(i - 2, points)

        skip = self.solve(i - 1, points)

        return max(take, skip)

    def deleteAndEarn(self, nums):

        max_num = max(nums)

        points = [0] * (max_num + 1)

        for num in nums:
            points[num] += num

        return self.solve(max_num, points)
```

---

# Why Recursion is Slow

The same states are solved repeatedly.

Example

```
solve(5)

solve(4)

solve(3)
```

again and again.

Time Complexity

```
O(2^n)
```

---

# Memoization

Whenever a state is solved,

store it.

If the same state appears again,

return it immediately.

---

# Memoization Code

```python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        n = len(nums)
        k = max(nums)
        points = [0] * (k + 1)

        for i in nums:
            points[i] += i
        
        dp = [-1] * (k + 1)

        def getMax(index):
            if index == 0:
                return points[index]
            
            if index < 0:
                return 0
            if dp[index] != -1:
                return dp[index]
                
            pick = points[index] + getMax(index - 2)
            skip = getMax(index - 1)
            ans = max(pick, skip)

            dp[index] = ans

            return ans

        return getMax(k)
```

---

# Dry Run (Memoization)

Input

```
nums=[2,2,3,3,3,4]
```

## Step 1

Build points array

```
points

Index

0 1 2 3 4

Value

0 0 4 9 4
```

Need

```
solve(4)
```

Take

```
4 + solve(2)

=

4 + 4

=

8
```

Skip

```
solve(3)

=

9
```

Choose

```
max(8,9)

=

9
```

Answer

```
9
```

---

# Time Complexity

```
O(MaxValue)
```

Space

```
O(MaxValue)
```

---

# Tabulation

Instead of recursion,

build answers from left to right.

---

## Transition

```
take

=

points[i]

+

dp[i-2]
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
    def deleteAndEarn(self, nums: List[int]) -> int:

        n = len(nums)
        k = max(nums)
        points = [0] * (k + 1)

        for i in nums:
            points[i] += i
        
        dp = [0] * (k + 1)
        dp[0] = points[0]

        for i in range(1, k+1):
            pick = points[i]
            if i > 1:
                pick += dp[i - 2]
            skip = dp[i-1]
            dp[i] = max(pick, skip)

        return dp[k]
```

---

# Dry Run (Tabulation)

Input

```
nums=[2,2,3,3,3,4]
```

Points

```
[0,0,4,9,4]
```

Initially

```
dp

[0,0,0,0,0]
```

### i = 2

```
take = 4

skip = 0

dp[2]=4
```

```
[0,0,4,0,0]
```

---

### i = 3

```
take = 9

skip = 4

dp[3]=9
```

```
[0,0,4,9,0]
```

---

### i = 4

```
take = 4+4=8

skip = 9

dp[4]=9
```

Final

```
[0,0,4,9,9]
```

Answer

```
9
```

---

# Space Optimization

Observe carefully.

```
dp[i]
```

depends only on

```
dp[i-1]

and

dp[i-2]
```

So we don't need the entire DP array.

Only two variables are enough.

---

# Space Optimized Code

```python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        n = len(nums)
        k = max(nums)
        points = [0] * (k + 1)

        for i in nums:
            points[i] += i

        prev1 = points[0]
        prev2 = 0

        for i in range(1, k+1):
            pick = points[i]
            if i > 1:
                pick += prev2
            prev2 = prev1
            prev1 = max(pick, prev2)

        return prev1
```

---

# Complete Dry Run

Input

```
nums=[2,2,3,3,3,4]
```

### Step 1

Count frequency

```
2 → 2

3 → 3

4 → 1
```

---

### Step 2

Calculate total points

```
2 → 4

3 → 9

4 → 4
```

---

### Step 3

Build points array

```
Index

0 1 2 3 4

Value

0 0 4 9 4
```

---

### Step 4

Run House Robber

At value

```
2
```

```
Take = 4

Skip = 0

Choose = 4
```

---

At value

```
3
```

```
Take = 9

Skip = 4

Choose = 9
```

---

At value

```
4
```

```
Take = 4 + 4 = 8

Skip = 9

Choose = 9
```

Final Answer

```
9
```

---

# Mathematical Logic

Suppose

```
frequency[x] = f
```

Each occurrence contributes

```
x
```

points.

Total contribution becomes

```
points[x]

=

x × f
```

Choosing one occurrence of `x` never deletes another occurrence of `x`.

It only deletes

```
x-1

and

x+1
```

Therefore,

it is always optimal to take **all** occurrences of `x`.

After grouping,

every unique value behaves like a house.

The recurrence becomes

```
dp[i]

=

max(

points[i] + dp[i-2],

dp[i-1]

)
```

---

# How to Build the Intuition

Whenever you solve a DP problem, ask yourself these questions.

### Question 1

Can equal values be grouped?

Here

```
Yes
```

---

### Question 2

If I choose one occurrence,

should I take all occurrences?

```
Yes
```

---

### Question 3

What becomes impossible after choosing?

Choosing

```
x
```

blocks

```
x-1

and

x+1
```

---

### Question 4

Does this remind me of another problem?

Yes.

```
House Robber
```

---

### Question 5

How many choices do I have?

Only two.

```
Take

or

Skip
```

That is the classic Pick / Not Pick DP pattern.

---

# Pattern Recognition

Whenever you see

- Maximize score
- Adjacent values cannot both be chosen
- Equal values can be merged
- Choosing one value blocks neighboring values

Immediately think

```
Original Array

↓

Frequency Map

↓

Points Array

↓

House Robber

↓

Dynamic Programming
```

---

# Common Mistakes

❌ Solving directly on the original array.

❌ Forgetting to group equal values.

❌ Applying House Robber on `nums` instead of `points`.

❌ Forgetting to size the points array as

```
max(nums)+1
```

---

# Quick Revision

### Step 1

```
Count frequency
```

↓

### Step 2

```
points[value]

=

value × frequency
```

↓

### Step 3

```
Treat points as House Robber array
```

↓

### Step 4

Use recurrence

```
dp[i]

=

max(

points[i]+dp[i-2],

dp[i-1]

)
```

↓

### Step 5

Space optimize to **O(1)**.

---

# Golden Rule

Whenever a problem says

- Choosing one value prevents choosing neighboring values.
- Duplicate values can be merged into one score.
- Find the maximum possible score.

Immediately think

```
Group Equal Values

↓

Build Points Array

↓

House Robber

↓

Pick / Skip DP
```

**The hardest part of this problem is not the Dynamic Programming.**

The hardest part is recognizing that after grouping equal values into total points, the problem becomes **exactly the House Robber problem**, and the same recurrence can be used directly.

<br/><br/><br/><br/><br/>

---

# 3186. Maximum Total Damage With Spell Casting

## Difficulty

**Medium**

---

# Pattern

- Dynamic Programming
- Coordinate Compression
- Pick / Not Pick DP
- Hash Map + Sorting

---

# Problem Statement

A magician has many spells.

Each spell has a damage value.

```
power = [1,1,3,4]
```

means there are four spells.

- Spell 1 → Damage = 1
- Spell 2 → Damage = 1
- Spell 3 → Damage = 3
- Spell 4 → Damage = 4

A spell can only be cast once.

However, there is one important rule.

If you cast a spell with damage **x**, then you **cannot cast any spell having damage**

```
x-2
x-1
x+1
x+2
```

Notice something important.

The restriction is based on the **damage value**, **NOT** on the spell index.

Your goal is to obtain the **maximum total damage**.

---

# Example 1

## Input

```
power = [1,1,3,4]
```

---

### Possible Choices

Take all 1's

```
1 + 1 = 2
```

Cannot take 3.

Can take 4.

Total

```
2 + 4 = 6
```

---

Take

```
3
```

Cannot take

```
1
4
5
2
```

Total

```
3
```

---

Take

```
4
```

Only

```
4
```

Total

```
4
```

---

Maximum

```
6
```

---

# Example 2

## Input

```
power = [7,1,6,6]
```

Choose

```
1 + 6 + 6
```

Total

```
13
```

If we choose

```
7
```

we cannot choose

```
6
```

Total

```
8
```

Maximum

```
13
```

---

# Understanding the Problem Like a Common Man

Imagine there are different magical stones.

```
1
1
3
4
```

Whenever you activate a stone,

all stones within distance **2** become useless.

For example

Choose

```
4
```

Then

```
2
3
5
6
```

cannot be chosen.

But

```
1
```

is still allowed.

Notice something interesting.

If there are multiple stones with the same value,

they **never block each other.**

So if we decide to use damage **4**,

there is no reason to take only one.

Take **all** of them.

This is the biggest observation.

---

# The Biggest Observation

Suppose

```
power =

[5,5,5,5]
```

If we decide to use damage

```
5
```

Why leave any 5 behind?

There is no penalty.

So

```
5+5+5+5
```

is always better.

Therefore,

instead of thinking about every spell,

we should think about **each unique damage value.**

---

# First Step

Group equal numbers.

Example

```
power =

[1,1,3,4,4,4]
```

becomes

| Damage | Total Damage |
|---------|--------------|
|1|2|
|3|3|
|4|12|

Instead of six spells,

we now have only

```
1
3
4
```

with profits

```
2
3
12
```

This greatly simplifies the problem.

---

# Why Sorting is Necessary

The restriction depends on

```
difference <=2
```

To know which damages conflict,

we must process them in order.

Sort unique damages.

Example

```
Damage

1
3
4
7
10
```

Now conflicts become easy to identify.

---

# How Should We Think?

Whenever we stand at one damage value,

we have only **two choices.**

---

## Choice 1

Take this damage.

If we take it,

we earn

```
totalDamage[current]
```

But then

every damage within

```
current-2
current-1
```

cannot exist in our answer.

So we jump to the nearest previous damage that differs by at least 3.

---

## Choice 2

Skip this damage.

Then we simply move to the previous damage.

---

Every DP problem starts with these two questions.

```
Take

or

Skip
```

---

# Building the Intuition

Suppose

```
Damages

1
3
4
7
```

Totals

```
2
3
4
7
```

Look at

```
4
```

Ask yourself

```
If I take 4,

what becomes impossible?
```

Answer

```
2
3
5
6
```

Among previous damages,

only

```
3
```

is affected.

```
1
```

is perfectly safe.

Therefore

```
Take 4

=

4

+

Best answer ending before damage 2
```

This is exactly what Dynamic Programming stores.

---

# Converting Into a DP Problem

Create

```
values = sorted(unique damages)
```

Example

```
values

[1,3,4,7]
```

Create

```
gain

[2,3,4,7]
```

Now define

```
dp[i]
```

Meaning

```
Maximum damage obtainable
using first i unique damages.
```

---

# Finding the Previous Compatible Damage

Suppose

```
values

[1,3,4,7]
```

At

```
7
```

We need previous value

```
<=4
```

because

```
7-4=3
```

Difference 3 is allowed.

So

```
previous = 4
```

At

```
4
```

Need previous value

```
<=1
```

Difference

```
4-1=3
```

Allowed.

This previous compatible index can be found efficiently using binary search.

---

# Mathematical Logic

For every unique damage,

there are only two possibilities.

Take it.

or

Don't take it.

If we take,

our answer becomes

```
Current Gain

+

Best Compatible Previous Answer
```

If we don't,

our answer remains

```
Previous DP
```

Hence

```
dp[i]

=

max(

Take,

Skip

)
```

---

# Recurrence Relation

Suppose

```
prev
```

is the last compatible damage.

Then

```
Take

=

gain[i]

+

dp[prev]
```

If no compatible damage exists,

```
Take = gain[i]
```

Skip

```
dp[i-1]
```

Finally

```
dp[i]

=

max(

gain[i]+dp[prev],

dp[i-1]

)
```

This is the heart of the solution.

---

# Why Binary Search?

Suppose there are

```
100000
```

unique damages.

Searching backwards every time would cost

```
O(n²)
```

Instead,

because damages are sorted,

we binary search.

Need first damage

```
< current-2
```

Binary search finds it in

```
O(log n)
```

---

# Algorithm

### Step 1

Count frequency.

```
Counter(power)
```

---

### Step 2

Convert into total gains.

```
gain[x]

=

x * frequency[x]
```

---

### Step 3

Sort unique damages.

---

### Step 4

DP over sorted damages.

For every damage

Binary search previous compatible value.

Compute

```
Take

Skip
```

Store maximum.

---

# Dry Run

Example

```
power

[1,1,3,4]
```

---

Frequency

```
1 → 2

3 → 1

4 →1
```

Gain

```
1 →2

3 →3

4 →4
```

Sorted values

```
[1,3,4]
```

---

## i = 0

Damage

```
1
```

Take

```
2
```

Skip

```
0
```

DP

```
[2]
```

---

## i = 1

Damage

```
3
```

Need previous damage

```
<=0
```

None.

Take

```
3
```

Skip

```
2
```

DP

```
[2,3]
```

---

## i = 2

Damage

```
4
```

Need previous damage

```
<=1
```

Compatible damage

```
1
```

Take

```
4+2

=

6
```

Skip

```
3
```

Choose

```
6
```

DP

```
[2,3,6]
```

Final Answer

```
6
```

---

# Another Dry Run

```
power

[7,1,6,6]
```

Frequency

```
1→1

6→2

7→1
```

Gain

```
1→1

6→12

7→7
```

Values

```
1

6

7
```

---

### Damage 1

```
dp=1
```

---

### Damage 6

Need previous

```
<=3
```

Compatible

```
1
```

Take

```
12+1

=13
```

Skip

```
1
```

DP

```
13
```

---

### Damage 7

Need previous

```
<=4
```

Compatible

```
1
```

Take

```
7+1=8
```

Skip

```
13
```

Choose

```
13
```

Answer

```
13
```

---

# Python Solution

## Memoization with Binary Search

```python
class Solution:
    def maximumTotalDamage(self, power: List[int]) -> int:
        freq = Counter(power)
        values = sorted(freq.keys())
        points = [v * freq[v] for v in values]

        n = len(points)
        
        def take(index):
            target = values[index] - 3
            start = 0
            end = index - 1
            ans = -1
            while start <= end:
                mid = (start + end) // 2
                if values[mid] <= target:
                    ans = mid
                    start = mid + 1
                else:
                    end = mid - 1
            return ans

        dp = [-1] * n

        def maxDamage(index):
            if index == 0:
                return points[0]
            
            if index < 0:
                return 0

            if dp[index] != -1:
                return dp[index]
            
            pick = points[index] + maxDamage(take(index))
            skip = maxDamage(index-1)
            ans = max(pick, skip)
            dp[index] = ans

            return ans

        return maxDamage(n-1)
```

## Tabulation with Binary Search

```python
from collections import Counter
from bisect import bisect_left

class Solution:
    def maximumTotalDamage(self, power):
        freq = Counter(power)

        values = sorted(freq.keys())
        gain = [x * freq[x] for x in values]

        n = len(values)
        dp = [0] * n

        for i in range(n):
            # Last value <= values[i] - 3
            j = bisect_left(values, values[i] - 2) - 1

            take = gain[i]
            if j >= 0:
                take += dp[j]

            skip = dp[i - 1] if i > 0 else 0

            dp[i] = max(take, skip)

        return dp[-1]
```

---

# Time Complexity

Sorting

```
O(n log n)
```

DP

```
O(m log m)
```

where

```
m = number of unique damages
```

Overall

```
O(n log n)
```

---

# Space Complexity

Counter

```
O(m)
```

DP

```
O(m)
```

Overall

```
O(m)
```

---

# How to Build the Intuition

Whenever you see a problem, ask these questions.

### Question 1

Can I group identical values?

If taking one copy never prevents taking another copy of the same value, then grouping them into one total gain is almost always beneficial.

---

### Question 2

What actually causes conflicts?

Here, conflicts are based on **damage values**, not on array positions.

This means the original order of the array is irrelevant. We should focus on the sorted unique damage values.

---

### Question 3

At every value, what choices do I have?

There are always two:

- **Take** this damage (and combine it with the best compatible previous answer).
- **Skip** this damage (keep the previous best answer).

Whenever you naturally think "take or skip", you should suspect Dynamic Programming.

---

### Question 4

What information from the past do I need?

If I take the current damage, I only need the best answer from the last damage that is at least 3 smaller.

This is why we use **binary search** on the sorted unique values.

---

### Question 5

Can I define a smaller subproblem?

Yes.

```
dp[i]
```

stores the maximum total damage considering only the first `i` unique damage values.

Once you define this state, the recurrence becomes straightforward.

---

# Common Mistakes

❌ Treating every spell individually instead of grouping identical damage values.

❌ Forgetting that the restriction is based on **damage values**, not indices.

❌ Scanning backwards linearly for every state, leading to `O(n²)`.

❌ Using `current - 2` as a compatible value. The previous compatible damage must satisfy:

```
previous_damage <= current_damage - 3
```

---

# Similar Problems

- House Robber
- Delete and Earn
- Maximum Sum of Non-Adjacent Elements
- House Robber II
- House Robber III

---

# Quick Revision

### State

```
dp[i]
=
Maximum total damage using the first i unique damage values.
```

### Transition

```
dp[i]
=
max(
    gain[i] + dp[last_compatible],
    dp[i-1]
)
```

### Optimization

- Group equal values.
- Sort unique damages.
- Use binary search to find the last compatible damage.
- Apply Pick / Skip Dynamic Programming.

---

# Golden Rule

Whenever a problem asks you to **maximize a total**, where **choosing one value makes a range of nearby values unavailable**, think:

1. Can I **merge identical values** into one total gain?
2. Can I **sort the unique values**?
3. At each value, is the decision simply **Take or Skip**?
4. If yes, it is very likely a **Pick / Not Pick Dynamic Programming** problem, often combined with **binary search** to find the last compatible choice.

<br/><br/><br/><br/><br/>

---

# 55. Jump Game

## Difficulty

Medium

## Pattern

- Greedy
- Dynamic Programming (1D DP)
- Reachability Problem

---

# Problem Statement

You are given an integer array `nums`.

Initially, you stand at the first index.

Each element represents the **maximum jump length** you can make from that position.

Return **true** if you can reach the last index.

Otherwise return **false**.

---

# Examples

## Example 1

Input

```
nums = [2,3,1,1,4]
```

Explanation

```
Start at index 0

Jump 1 step to index 1

Jump 3 steps to index 4

Reached the last index.
```

Output

```
True
```

---

## Example 2

Input

```
nums = [3,2,1,0,4]
```

Explanation

```
You always get stuck at index 3.

nums[3] = 0

No further jumps are possible.
```

Output

```
False
```

---

# Understanding the Problem Like a Common Man

Imagine there are stones placed in a straight line.

```
0   1   2   3   4

2   3   1   1   4
```

You are standing on the first stone.

The number written on a stone tells you

```
How far you can jump.
```

If a stone has

```
3
```

You can jump

```
1 step

or

2 steps

or

3 steps
```

Your goal is simply

```
Can I reach the last stone?
```

Not

```
Minimum jumps

Maximum jumps

Total jumps
```

Just

```
Can I reach?
```

---

# Thinking Like a Beginner

Suppose

```
nums = [2,3,1,1,4]
```

Initially

```
Index = 0

Maximum Jump = 2
```

You have only two choices.

Jump

```
1 step
```

or

```
2 steps
```

Again,

wherever you land,

you get new choices.

Notice

At every position,

you are making decisions.

This naturally suggests recursion.

---

# Step 1 : Define the State

Ask yourself

```
What information completely describes the remaining problem?
```

Suppose you are currently standing at

```
Index i
```

Question

Can you reach the last index from here?

That is enough.

So define

```
solve(i)

=

Can I reach the last index starting from index i?
```

This is a

```
1D DP
```

because only one variable changes.

---

# Step 2 : Choices

At index

```
i
```

Maximum jump

```
nums[i]
```

You can jump

```
i+1

i+2

...

i+nums[i]
```

If **any** of these reaches the end,

then

```
solve(i)=True
```

Otherwise

```
False
```

---

# Recurrence Relation

```
dp[i]

=

dp[i+1]

OR

dp[i+2]

OR

...

OR

dp[i+nums[i]]
```

In words

```
If any reachable position can reach the end,

then the current position can also reach the end.
```

---

# Base Case

If

```
index >= last index
```

You have already reached the destination.

Return

```
True
```

---

# Recursive Solution

```python
from functools import cache

class Solution:

    def canJump(self, nums):

        n = len(nums)

        @cache
        def solve(index):

            if index >= n - 1:
                return True

            farthest = min(n - 1, index + nums[index])

            for nextIndex in range(index + 1, farthest + 1):

                if solve(nextIndex):
                    return True

            return False

        return solve(0)
```

---

# Why Recursion is Slow

Suppose

```
nums = [5,5,5,5,5,5...]
```

From every index,

you explore many future indices.

Many states are computed repeatedly.

Example

```
solve(8)

solve(9)

solve(10)
```

appear again and again.

Hence

Dynamic Programming.

---

# Memoization

Store every solved state.

Initially

```
dp

-1

-1

-1

-1
```

Whenever

```
solve(i)
```

is computed,

store it.

Next time,

return immediately.

---

# Memoization Code

```python
from functools import cache

class Solution:

    def canJump(self, nums):

        n = len(nums)

        @cache
        def solve(index):

            if index >= n - 1:
                return True

            farthest = min(n - 1, index + nums[index])

            for nextIndex in range(index + 1, farthest + 1):

                if solve(nextIndex):
                    return True

            return False

        return solve(0)
```

---

# Dry Run

Example

```
nums = [2,3,1,1,4]
```

```
solve(0)
```

Possible jumps

```
1

2
```

Try

```
solve(1)
```

Again

Possible jumps

```
2

3

4
```

```
solve(4)

↓

Reached last index

↓

True
```

Therefore

```
solve(1)=True

↓

solve(0)=True
```

---

# Time Complexity

Worst Case

```
O(n²)
```

Space

```
DP Array / Cache

+

Recursion Stack

=

O(n)
```

---

# Tabulation

Instead of solving recursively,

start from the last index.

We know

```
Last index

↓

Always True
```

Now move backwards.

If any reachable index is True,

mark the current index as True.

---

# Transition

```
dp[i]

=

True

if

any

dp[j]

is True

where

i<j<=i+nums[i]
```

---

# Tabulation Code

```python
class Solution:

    def canJump(self, nums):

        n = len(nums)

        dp = [False] * n

        dp[n - 1] = True

        for i in range(n - 2, -1, -1):

            farthest = min(n - 1, i + nums[i])

            for j in range(i + 1, farthest + 1):

                if dp[j]:

                    dp[i] = True
                    break

        return dp[0]
```

---

# Dry Run

```
nums = [2,3,1,1,4]
```

Initially

```
dp

F F F F T
```

Index

```
3
```

Can reach

```
4

↓

True
```

```
F F F T T
```

Index

```
2
```

Can reach

```
3

↓

True
```

```
F F T T T
```

Index

```
1
```

Can reach

```
2

↓

True
```

```
F T T T T
```

Index

```
0
```

Can reach

```
1

↓

True
```

Final

```
T T T T T
```

Answer

```
True
```

---

# Can DP Be Optimized?

Yes.

Observe carefully.

DP is repeatedly checking

```
Can I reach a True position?
```

Instead,

can we simply remember

```
The farthest index reachable so far?
```

Yes.

This leads to a Greedy solution.

---

# Greedy Intuition

Suppose

```
nums = [2,3,1,1,4]
```

Initially

```
Maximum Reach

=

0
```

At index

```
0
```

Maximum reachable

```
0+2

=

2
```

Now

```
Maximum Reach

=

2
```

Move to

```
Index 1
```

Can you stand here?

Yes.

Because

```
1<=2
```

Update

```
1+3=4
```

Now

```
Maximum Reach

=

4
```

Since

```
4

>=

Last Index
```

We can reach the end.

---

# Greedy Algorithm

Maintain

```
maxReach
```

For every index

If

```
Current Index

>

maxReach
```

You cannot even stand there.

Return

```
False
```

Otherwise

update

```
maxReach

=

max(

maxReach,

i+nums[i]

)
```

If

```
maxReach

>=

Last Index
```

Return

```
True
```

---

# Greedy Code

```python
class Solution:

    def canJump(self, nums):

        maxReach = 0

        for i in range(len(nums)):

            if i > maxReach:
                return False

            maxReach = max(maxReach, i + nums[i])

        return True
```

---

# Dry Run (Greedy)

Example

```
nums = [2,3,1,1,4]
```

Initially

```
maxReach = 0
```

Index

```
0
```

```
Reach

=

0+2

=

2
```

```
maxReach=2
```

---

Index

```
1
```

```
Reach

=

1+3

=

4
```

```
maxReach=4
```

Already

```
Last Index

=

4
```

Reached.

Answer

```
True
```

---

# Why Greedy Works

DP asks

```
Can I reach the end from every index?
```

Greedy asks

```
What is the farthest place I can currently reach?
```

If you can always move inside the reachable region,

there is no need to remember every DP state.

Only remember

```
Maximum Reach
```

---

# Complexity Comparison

## Dynamic Programming

Time

```
O(n²)
```

Space

```
O(n)
```

---

## Greedy

Time

```
O(n)
```

Space

```
O(1)
```

---

# Mathematical Logic

DP checks

```
Every possible jump.
```

Greedy keeps

```
The farthest reachable index.
```

If you can reach index

```
i
```

then every jump from

```
i
```

extends your reachable region.

Eventually,

either

```
Reach

>=

Last Index
```

or

you get stuck.

---

# Pattern Recognition

If a problem asks

- Can I reach somewhere?
- Maximum reachable position
- Reachability
- Continuous movement

Think

```
Greedy
```

If it asks

- Count ways
- Maximum score
- Minimum cost

Think

```
Dynamic Programming
```

---

# Common Mistakes

❌ Treating `nums[i]` as the destination index instead of the jump length.

❌ Writing

```python
range(i + 1, nums[i] + 1)
```

instead of

```python
range(i + 1, i + nums[i] + 1)
```

❌ Forgetting to limit the jump using

```python
min(n - 1, i + nums[i])
```

❌ Assuming DP is the optimal solution.

---

# Similar Problems

- Jump Game II
- Frog Jump
- Minimum Jumps to Reach End
- Can Reach End
- Reachability Problems

---

# Quick Revision

## DP State

```
dp[i]

=

Can I reach the last index starting from i?
```

---

## DP Recurrence

```
dp[i]

=

OR

of

dp[i+1]

...

dp[i+nums[i]]
```

---

## Greedy Variable

```
maxReach
```

---

## DP Complexity

```
Time

O(n²)

Space

O(n)
```

---

## Greedy Complexity

```
Time

O(n)

Space

O(1)
```

---

# Golden Rule

Whenever a problem asks **whether a destination is reachable** and every move only expands the region you can already access, first think about **Greedy**. If you find yourself repeatedly checking the same future states from different positions, a DP solution exists—but always ask whether maintaining just the **farthest reachable position** is enough. In Jump Game, that single greedy observation reduces the solution from **O(n²)** DP to **O(n)** time.