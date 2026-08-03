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
- [746. Min Cost Climbing Stairs](#746-min-cost-climbing-stairs)
- [#486. Predict the Winner](#486-predict-the-winner)
- [91. Decode Ways](#91-decode-ways)
- [1406. Stone Game III](#1406-stone-game-iii)


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

<br/><br/><br/><br/><br/>

---

# 746. Min Cost Climbing Stairs

# Difficulty

Easy

# Pattern

* Dynamic Programming
* 1D DP
* Decision DP

---

# Problem Statement

You are given an integer array **cost**, where

```
cost[i]
```

represents the cost of stepping on the **i-th stair**.

Once you pay the cost of a stair, you can climb either

* **1 step**
* **2 steps**

You are allowed to start from

* Stair **0**
* Stair **1**

Find the **minimum total cost** required to reach the **top of the staircase**.

The top is just beyond the last stair, so there is **no cost** for reaching the top.

---

# Examples

## Example 1

```
Input

cost = [10,15,20]
```

Possible choices

Start from stair 0

```
10 → 20 → Top

Cost = 30
```

Start from stair 1

```
15 → Top

Cost = 15
```

Answer

```
15
```

---

## Example 2

```
Input

cost = [1,100,1,1,1,100,1,1,100,1]
```

One optimal path

```
1 → 1 → 1 → 1 → 1 → 1
```

Total Cost

```
6
```

Answer

```
6
```

---

# Understanding the Problem Like a Common Man

Imagine there is a staircase.

Every stair has a price written on it.

```
        TOP
         ↑
      [20]
      [15]
      [10]
      Ground
```

Whenever you stand on a stair,

you **must pay** its cost.

After paying,

you can move

```
1 step
```

or

```
2 steps
```

The question is

> **What is the minimum amount of money needed to reach the top?**

Notice something important.

We are **NOT counting the number of ways**.

We are **NOT finding the shortest path**.

We are trying to

```
Minimize the total cost.
```

---

# Let's Think Like a Human

Suppose

```
cost = [10,15,20,5]
```

If you're standing on stair 1,

what choices do you have?

```
Choice 1

Go to stair 2
```

or

```
Choice 2

Go to stair 3
```

Nothing else.

At every stair,

there are only **two possible moves**.

This tells us

```
The problem can be broken into smaller problems.
```

---

# Thinking from the End (Most Important Intuition)

Instead of thinking

> "How do I start from stair 0?"

Think

> "If I'm standing on stair i, what is the minimum cost to reach the top?"

Suppose

```
Current stair = 3
```

To reach the top,

you can

```
Go to stair 4
```

or

```
Go to stair 5
```

You will naturally choose

```
the cheaper path.
```

Therefore,

```
Minimum Cost(i)

=

cost[i]

+

minimum of

Cost(i+1)

and

Cost(i+2)
```

This is the entire intuition.

---

# Generalizing

For every stair

```
Cost(i)

=

cost[i]

+

min(

Cost(i+1),

Cost(i+2)

)
```

Why?

Because after paying the current stair,

you have only two choices

```
Move 1 step

or

Move 2 steps
```

Since we want the minimum cost,

we choose the cheaper option.

---

# This Looks Familiar...

Look carefully.

```
Cost(i)

=

cost[i]

+

min(

Cost(i+1),

Cost(i+2)

)
```

This is a classic Dynamic Programming recurrence.

Unlike **Climbing Stairs**, where we added the number of ways,

here we take the **minimum** because the problem asks us to minimize the total cost.

---

# Finding Base Cases

Every DP problem starts with

```
"What are the smallest problems whose answers I already know?"
```

---

## Base Case 1

If we reach the top

```
i >= n
```

No more cost is required.

Answer

```
0
```

---

## Base Case 2

If we are standing on the last stair

```
n-1
```

We only need to pay its cost.

Answer

```
cost[n-1]
```

---

# Recurrence Relation

```
Cost(i)

=

cost[i]

+

min(

Cost(i+1),

Cost(i+2)

)
```

---

# Recursive Thinking

Suppose we want

```
Cost(0)
```

Immediately,

our brain should think

```
Need

Cost(1)

and

Cost(2)
```

Then

```
Cost(1)

needs

Cost(2)

Cost(3)
```

Again

```
Cost(2)

needs

Cost(3)

Cost(4)
```

---

# Recursion Tree

```
                    Cost(0)
                  /         \
            Cost(1)       Cost(2)
            /     \        /     \
      Cost(2)  Cost(3) Cost(3) Cost(4)
```

Notice

```
Cost(2)

is computed twice.

Cost(3)

is computed multiple times.
```

This is called

```
Overlapping Subproblems.
```

Perfect place for Dynamic Programming.

---

# Recursive Solution

```python
class Solution:

    def solve(self, i, cost):

        n = len(cost)

        if i >= n:
            return 0

        if i == n - 1:
            return cost[n - 1]

        step1 = cost[i] + self.solve(i + 1, cost)
        step2 = cost[i] + self.solve(i + 2, cost)

        return min(step1, step2)

    def minCostClimbingStairs(self, cost):

        return min(self.solve(0, cost),
                   self.solve(1, cost))
```

---

# Why Recursion is Slow

Suppose

```
n = 30
```

The recursion repeatedly computes

```
Cost(20)

Cost(18)

Cost(15)

Cost(10)
```

again and again.

Huge waste of computation.

Time Complexity

```
O(2^n)
```

---

# Memoization

## Idea

Store every answer after computing it.

Next time,

if we need the same state,

don't compute it again.

Simply return the stored answer.

---

# DP Array

Initially

```
-1 -1 -1 -1 -1
```

Suppose

```
Cost(4)=5
```

Store

```
dp[4]=5
```

Next time,

if we again need

```
Cost(4)
```

return

```
dp[4]
```

instead of solving it again.

---

# Memoization Code

```python
class Solution:

    def solve(self, pos, cost, dp):

        n = len(cost)

        if pos >= n:
            return 0

        if pos == n - 1:
            return cost[n - 1]

        if dp[pos] != -1:
            return dp[pos]

        step1 = cost[pos] + self.solve(pos + 1, cost, dp)
        step2 = cost[pos] + self.solve(pos + 2, cost, dp)

        dp[pos] = min(step1, step2)

        return dp[pos]

    def minCostClimbingStairs(self, cost):

        n = len(cost)

        dp = [-1] * n

        return min(self.solve(0, cost, dp),
                   self.solve(1, cost, dp))
```

---

# Time Complexity

Every state is computed only once.

```
Time

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

Instead of solving recursively,

start from the end and build the answer backwards.

---

# DP Meaning

```
dp[i]

=

Minimum cost to reach the top

starting from stair i.
```

---

# DP Initialization

The top requires no cost.

```
dp[n]=0
```

The last stair requires paying only its own cost.

```
dp[n-1]=cost[n-1]
```

---

# DP Transition

Move backwards.

```
dp[i]

=

cost[i]

+

min(

dp[i+1],

dp[i+2]

)
```

---

# DP Table Example

Suppose

```
cost = [10,15,20]
```

Initially

```
Index

0 1 2 3

DP

0 0 20 0
```

Now

```
dp[1]

=

15

+

min(20,0)

=

15
```

Table

```
0 15 20 0
```

Next

```
dp[0]

=

10

+

min(15,20)

=

25
```

Final

```
25 15 20 0
```

Answer

```
min(dp[0],dp[1])

=

15
```

---

# Tabulation Code

```python
class Solution:

    def minCostClimbingStairs(self, cost):

        n = len(cost)

        dp = [0] * (n + 1)

        dp[n] = 0
        dp[n - 1] = cost[n - 1]

        for i in range(n - 2, -1, -1):

            dp[i] = cost[i] + min(dp[i + 1], dp[i + 2])

        return min(dp[0], dp[1])
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

Observe carefully.

```
dp[i]

depends only on

dp[i+1]

and

dp[i+2]
```

Nothing else.

So why store the entire DP array?

No need.

Just remember the next two answers.

---

Initially

```
next1 = cost[n-1]

next2 = 0
```

Here,

```
next1

represents

dp[i+1]
```

and

```
next2

represents

dp[i+2]
```

For every stair,

```
current

=

cost[i]

+

min(next1,next2)
```

Then move the variables backward.

```
next2 = next1

next1 = current
```

---

# Dry Run

Suppose

```
cost = [10,15,20]
```

Initially

```
next1 = 20

next2 = 0
```

---

### i = 1

```
current

=

15

+

min(20,0)

=

15

next2 = 20

next1 = 15
```

---

### i = 0

```
current

=

10

+

min(15,20)

=

25

next2 = 15

next1 = 25
```

Finally

```
next1 = dp[0]

next2 = dp[1]
```

Answer

```
min(25,15)

=

15
```

---

# Space Optimized Code

```python
class Solution:

    def minCostClimbingStairs(self, cost):

        n = len(cost)

        next1 = cost[n - 1]
        next2 = 0

        for i in range(n - 2, -1, -1):

            current = cost[i] + min(next1, next2)

            next2 = next1
            next1 = current

        return min(next1, next2)
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

Whenever you see a problem, ask yourself these questions.

### Question 1

```
Can I express the answer using smaller answers?
```

Here

```
Yes.

Cost(i)

depends on

Cost(i+1)

and

Cost(i+2)
```

---

### Question 2

```
What choices do I have?
```

Here

```
Move

1 step

or

2 steps
```

Choose the cheaper one.

---

### Question 3

```
Am I solving the same smaller problem repeatedly?
```

Yes.

```
Cost(2)

Cost(3)

Cost(2)

Cost(3)
```

Hence,

Dynamic Programming is the correct approach.

---

# Pattern Recognition

Whenever a problem asks

* Minimum Cost
* Minimum Energy
* Minimum Steps
* Reach Destination
* 1 Step / 2 Step Choices
* Optimize Cost

Immediately think

```
Dynamic Programming
```

---

# Common Mistakes

❌ Forgetting that you can start from **both** stair `0` and stair `1`.

❌ Returning `dp[0]` instead of

```
min(dp[0], dp[1])
```

❌ Forgetting the base case

```
i >= n

return 0
```

❌ Using recursion without memoization for large inputs.

---

# Similar Problems

* Climbing Stairs
* Fibonacci Number
* House Robber
* Frog Jump
* Coin Change
* Decode Ways
* Jump Game

---

# Quick Revision

### Recurrence

```
Cost(i)

=

cost[i]

+

min(

Cost(i+1),

Cost(i+2)

)
```

---

### Base Cases

```
Cost(n)=0

Cost(n-1)=cost[n-1]
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

> **"Find the minimum cost to reach a destination,"**

and each answer depends on a few **future or previous smaller states**, think **Dynamic Programming**.

For this problem, the key realization is:

> **From every stair, you have only two choices—climb 1 step or 2 steps. Pay the current stair's cost, then choose the cheaper of the two remaining paths. This naturally leads to the recurrence:**

```
Cost(i)

=

cost[i]

+

min(

Cost(i+1),

Cost(i+2)

)
```

Once this recurrence is identified, the solution can be implemented using **Memoization**, **Tabulation**, or **Space Optimization**.

<br/><br/><br/><br/><br/>

---

# 486. Predict the Winner

# Difficulty

Medium

# Pattern

- Dynamic Programming
- Game Theory DP
- Interval DP (2D DP)
- Min-Max Strategy

---

# Problem Statement

You are given an integer array `nums`.

Two players are playing a game.

- Player 1 starts first.
- Both players play optimally.
- On every turn, a player can choose either:
  - The leftmost number
  - The rightmost number
- The chosen number is added to that player's score.
- The chosen number is removed from the array.

Return

```
True
```

if Player 1 can win.

If both players get the same score,

Player 1 is still considered the winner.

---

# Examples

## Example 1

Input

```
nums = [1,5,2]
```

Player 1 chooses

```
1
```

Remaining

```
5 2
```

Player 2 chooses

```
5
```

Remaining

```
2
```

Scores

```
Player1 = 3

Player2 = 5
```

Answer

```
False
```

---

## Example 2

Input

```
nums = [1,5,233,7]
```

Player 1 chooses

```
1
```

Player 2 chooses

```
5
```

Player 1 chooses

```
233
```

Scores

```
Player1 = 234

Player2 = 12
```

Answer

```
True
```

---

# Understanding the Problem Like a Common Man

Imagine there is a row of chocolates.

```
1   5   233   7
```

Two children are playing.

Rules

- One child picks only from the left end.
- Or from the right end.

Nobody can pick from the middle.

Both children are very smart.

Both want to maximize their own score.

Question

```
Can Player 1 win?
```

---

# The Biggest Mistake Beginners Make

Many beginners think

```
I should calculate Player 1's score.
```

This becomes complicated because

Player 2 is also playing optimally.

Instead,

think about

```
Difference
```

between the scores.

---

# The Most Important Intuition

Suppose

```
Player1 = 15

Player2 = 10
```

Difference

```
+5
```

Player 1 wins.

---

Suppose

```
Player1 = 9

Player2 = 13
```

Difference

```
-4
```

Player 2 wins.

Instead of storing

```
Player1 Score

Player2 Score
```

store

```
Player1 Score

-

Player2 Score
```

This makes the recurrence much simpler.

---

# Thinking Like a Human

Suppose

```
nums = [1,5,2]
```

Current player has only two choices.

Choice 1

Take

```
1
```

Choice 2

Take

```
2
```

Nothing else.

Every state has exactly

```
Two Choices
```

---

# Step 1 : Define the State

Ask yourself

```
What information completely describes the remaining problem?
```

Suppose the remaining array is

```
left

↓

1 5 2 7

      ↑

    right
```

Only

```
Left Index

Right Index
```

matter.

Therefore

```
solve(left,right)
```

is enough.

Since two variables define the state,

this is

```
2D DP
```

---

# What does solve(left,right) mean?

Very important.

```
solve(left,right)

=

Maximum score difference

(Current Player

-

Other Player)

from

nums[left...right]
```

Notice

NOT

maximum score.

Maximum

```
Difference
```

---

# Why Difference?

Suppose

```
Current Player

takes

10
```

Remaining game

belongs to

Opponent.

The recursion will now calculate

```
Opponent

-

Me
```

But I want

```
Me

-

Opponent
```

Therefore

I subtract.

This is the key intuition.

---

# Deriving the Recurrence

Suppose

```
nums = [1,5,2]
```

Current player chooses

Left

```
1
```

Remaining

```
5 2
```

Opponent now plays optimally.

Opponent's advantage

```
solve(left+1,right)
```

My final advantage

```
1

-

solve(left+1,right)
```

---

Similarly

Choose Right

```
2

-

solve(left,right-1)
```

Take whichever gives a better result.

---

# Recurrence Relation

```
dp[left][right]

=

max(

nums[left]

-

dp[left+1][right],

nums[right]

-

dp[left][right-1]

)
```

This is the heart of the problem.

---

# Finding the Base Case

Suppose

Only one element remains.

```
7
```

Current player takes it.

Opponent gets

```
0
```

Difference

```
7
```

Therefore

```
left==right

↓

nums[left]
```

---

# Recursive Thinking

Suppose

```
solve(0,2)
```

Choices

Take Left

```
nums[0]

-

solve(1,2)
```

Take Right

```
nums[2]

-

solve(0,1)
```

Again

```
solve(1,2)
```

creates two more choices.

Eventually,

all recursive paths reach

```
left==right
```

---

# Recursion Tree

```
                  (0,2)
                 /     \
             Left      Right
             /           \
         (1,2)         (0,1)
        /    \         /    \
    (2,2) (1,1)   (1,1) (0,0)
```

Notice

```
solve(1,1)
```

is computed twice.

This is

```
Overlapping Subproblems
```

Hence

Dynamic Programming.

---

# Recursive Solution

```python
class Solution:

    def solve(self,left,right,nums):

        if left==right:
            return nums[left]

        takeLeft=nums[left]-self.solve(left+1,right,nums)

        takeRight=nums[right]-self.solve(left,right-1,nums)

        return max(takeLeft,takeRight)

    def PredictTheWinner(self,nums):

        return self.solve(0,len(nums)-1,nums)>=0
```

---

# Why Recursion is Slow

Suppose

```
20 elements
```

The same intervals

```
solve(5,10)

solve(7,14)

solve(2,8)
```

are solved repeatedly.

Huge waste.

Time Complexity

```
O(2^n)
```

---

# Memoization

Idea

Store every solved interval.

State

```
dp[left][right]
```

Initially

```
-1
```

means

```
Not computed
```

Once computed,

store it.

---

# Memoization Code

```python
class Solution:

    def solve(self,left,right,nums,dp):

        if left==right:
            return nums[left]

        if dp[left][right]!=None:
            return dp[left][right]

        takeLeft=nums[left]-self.solve(left+1,right,nums,dp)

        takeRight=nums[right]-self.solve(left,right-1,nums,dp)

        dp[left][right]=max(takeLeft,takeRight)

        return dp[left][right]

    def PredictTheWinner(self,nums):

        n=len(nums)

        dp=[[None]*n for _ in range(n)]

        return self.solve(0,n-1,nums,dp)>=0
```

---

# Time Complexity

Every interval

```
(left,right)
```

is computed once.

Number of intervals

```
O(n²)
```

Time

```
O(n²)
```

Space

```
DP Table

+

Recursion Stack

=

O(n²)
```

---

# Tabulation

Instead of recursion,

build answers for smaller intervals first.

Base case

```
dp[i][i]

=

nums[i]
```

Now compute

Length

```
2

↓

3

↓

4

↓

...

↓

n
```

---

# Transition

```
dp[left][right]

=

max(

nums[left]-dp[left+1][right],

nums[right]-dp[left][right-1]

)
```

---

# Tabulation Code

```python
class Solution:

    def PredictTheWinner(self, nums):

        n = len(nums)

        dp = [[0] * n for _ in range(n)]

        for i in range(n):
            dp[i][i] = nums[i]

        for length in range(2, n + 1):

            for left in range(n - length + 1):

                right = left + length - 1

                takeLeft = nums[left] - dp[left + 1][right]

                takeRight = nums[right] - dp[left][right - 1]

                dp[left][right] = max(takeLeft, takeRight)

        return dp[0][n - 1] >= 0
```

---

# Dry Run

Suppose

```
nums = [1,5,2]
```

Initially

```
dp

1 0 0
0 5 0
0 0 2
```

Length = 2

```
dp[0][1]

=

max(

1-5,

5-1

)

=

4
```

```
dp[1][2]

=

max(

5-2,

2-5

)

=

3
```

Table

```
1 4 0
0 5 3
0 0 2
```

Length = 3

```
dp[0][2]

=

max(

1-3,

2-4

)

=

-2
```

Final

```
1 4 -2
0 5 3
0 0 2
```

Since

```
-2

<

0
```

Player 1 loses.

Answer

```
False
```

---

# Why >= 0?

Suppose

```
Difference

=

5
```

Player 1 wins.

---

Suppose

```
Difference

=

0
```

Tie.

Problem says

Player 1 still wins.

---

Suppose

```
Difference

=

-3
```

Player 2 wins.

Therefore

```
dp[0][n-1]>=0
```

---

# How to Build the Intuition

Whenever you see

```
Two Players
```

ask

```
Can I store

Difference

instead of

Both Scores?
```

Most game DP problems become much simpler.

---

# Pattern Recognition

Whenever you hear

- Two Players
- Optimal Play
- Pick Left or Right
- Subarray
- Interval

Think

```
Game Theory DP

+

Interval DP
```

---

# Common Mistakes

❌ Trying to calculate both players' scores separately.

❌ Forgetting that after your move, the opponent becomes the current player.

❌ Using `+` instead of `-` in the recurrence.

❌ Defining the wrong DP state.

---

# Similar Problems

- Stone Game
- Stone Game II
- Stone Game III
- Stone Game VII
- Optimal Strategy for a Game
- Burst Balloons (Interval DP)

---

# Quick Revision

### DP State

```
dp[left][right]

=

Maximum Difference

(Current Player

-

Other Player)
```

---

### Recurrence

```
max(

nums[left]-dp[left+1][right],

nums[right]-dp[left][right-1]

)
```

---

### Base Case

```
left==right

↓

nums[left]
```

---

### DP Progression

```
Recursion

↓

Memoization

↓

Tabulation
```

---

# Golden Rule

Whenever a game involves **two optimal players** taking turns, don't try to calculate each player's score separately. Instead, think in terms of the **score difference** between the current player and the opponent. After every move, the roles swap, which naturally leads to subtracting the opponent's best future advantage. This simple idea is the key to solving many Game Theory DP problems.

<br/><br/><br/><br/><br/>

---

# 91. Decode Ways

# Difficulty

Medium

# Pattern

- Dynamic Programming
- 1D DP
- String DP
- Prefix DP

---

# Problem Statement

You are given a string `s` containing only digits.

Each number can be mapped to a letter as follows:

```
1  -> A

2  -> B

3  -> C

...

26 -> Z
```

Your task is to find **how many different ways** the given string can be decoded.

If the string cannot be decoded,

return

```
0
```

---

# Examples

## Example 1

Input

```
s = "12"
```

Possible Decodings

```
1 2

↓

AB
```

and

```
12

↓

L
```

Answer

```
2
```

---

## Example 2

Input

```
s = "226"
```

Possible Decodings

```
2 2 6

↓

BBF
```

```
22 6

↓

VF
```

```
2 26

↓

BZ
```

Answer

```
3
```

---

## Example 3

Input

```
s = "06"
```

Notice

```
06
```

is NOT valid.

Only

```
6
```

is valid.

A number cannot start with

```
0
```

Answer

```
0
```

---

# Understanding the Problem Like a Common Man

Imagine someone sends you a secret code.

```
226
```

You have a dictionary.

```
1 -> A

2 -> B

3 -> C

...

26 -> Z
```

Now you have to read the digits.

Every time you stand at a digit,

you have only two possible ways to read it.

---

## Choice 1

Read

```
One digit
```

Example

```
2

↓

B
```

---

## Choice 2

Read

```
Two digits
```

Example

```
22

↓

V
```

But remember

The two digits must form a number

between

```
10

and

26
```

Otherwise,

it is not a valid letter.

---

# Thinking Like a Human

Suppose

```
226
```

Stand at the first digit.

```
2 2 6
↑
```

What can you do?

---

## Option 1

Take

```
2
```

Remaining string

```
26
↑
```

Now solve

```
26
```

---

## Option 2

Take

```
22
```

Remaining string

```
6
↑
```

Now solve

```
6
```

Notice something.

After making one decision,

the remaining problem is exactly the same type of problem.

This is a strong hint that

```
Dynamic Programming
```

can be used.

---

# The Biggest Intuition

Whenever you are standing at index

```
i
```

ask yourself

> **"How many ways can I decode the remaining string starting from this index?"**

Notice

You are NOT asking

```
How many ways have I already decoded?
```

Instead,

you are asking

```
How many ways are left?
```

This becomes our DP state.

---

# Defining the DP State

Let

```
solve(i)
```

represent

```
Number of ways

to decode

the substring

starting from index i.
```

Example

Suppose

```
s = "226"
```

```
Index

0 1 2

2 2 6
```

Then

```
solve(0)

↓

Ways to decode

"226"
```

```
solve(1)

↓

Ways to decode

"26"
```

```
solve(2)

↓

Ways to decode

"6"
```

```
solve(3)

↓

Ways to decode

""
```

This is our DP state.

Notice

Only one variable

```
i
```

changes.

Therefore,

this is

```
1D DP
```

---

# Why Only One Index?

Many beginners try something like

```
solve(start,end)
```

This is unnecessary.

Ask yourself

```
After decoding one or two digits,

what information is needed?
```

Only

```
Current Position
```

Everything before it has already been decoded.

Therefore

```
solve(i)
```

is enough.

---

# Finding the Choices

Suppose

```
226
↑
```

Current digit

```
2
```

You have exactly two choices.

---

## Choice 1

Decode one digit.

```
2

↓

B
```

Remaining problem

```
26
↑
```

Recursion becomes

```
solve(i+1)
```

---

## Choice 2

Decode two digits.

```
22

↓

V
```

Remaining problem

```
6
↑
```

Recursion becomes

```
solve(i+2)
```

That's all.

Every state has exactly

```
Two Choices
```

---

# When is a Choice Valid?

This is the most important observation.

---

## One Digit

Valid only if

```
1

to

9
```

If

```
0
```

appears,

it cannot be decoded alone.

Example

```
0

❌ Invalid
```

---

## Two Digits

Valid only if

```
10

to

26
```

Examples

```
10

✓
```

```
26

✓
```

```
27

✗
```

```
35

✗
```

```
06

✗
```

Always remember

```
Leading Zero

=

Invalid
```

---

# Deriving the Recurrence Relation

Suppose

```
226
↑
```

Current character

```
2
```

---

## Case 1

Take one digit.

Remaining

```
26
```

Ways

```
solve(i+1)
```

---

## Case 2

Take two digits.

Remaining

```
6
```

Ways

```
solve(i+2)
```

---

Total Ways

```
solve(i)

=

solve(i+1)

+

solve(i+2)
```

But

only if

the two-digit number lies between

```
10

and

26
```

---

If

```
s[i]=='0'
```

Then

```
0
```

ways exist,

because decoding cannot start from

```
0
```

---

# Final Recurrence Relation

If

```
s[i]=='0'
```

```
solve(i)=0
```

Otherwise

```
solve(i)

=

solve(i+1)
```

Plus

```
solve(i+2)
```

only if

```
10 <= int(s[i:i+2]) <= 26
```

This is the heart of the problem.

---

# Finding the Base Case

Suppose

```
i == n
```

This means

You have successfully decoded

the entire string.

For example

```
226
      ↑
```

Nothing is left.

How many ways are there to decode

an empty string?

Exactly

```
1
```

Why?

Because

you have already completed one valid decoding.

Therefore

```
if i==n

↓

return 1
```

---

# Recursive Thinking

Suppose

```
s="226"
```

Initially

```
solve(0)
```

Current digit

```
2
```

Choices

Take one digit

```
↓

solve(1)
```

Take two digits

```
↓

solve(2)
```

Again

```
solve(1)
```

creates two more choices.

Again

```
solve(2)
```

creates another choice.

Eventually,

every recursive path reaches

```
i==n
```

which returns

```
1
```

---

# Recursion Tree

For

```
226
```

```
                 solve(0)
                /        \
         take1           take2
          |                |
      solve(1)         solve(2)
      /      \             |
 take1      take2        take1
   |          |            |
solve(2)   solve(3)    solve(3)
```

Notice

```
solve(2)
```

is computed

multiple times.

This is called

```
Overlapping Subproblems
```

Hence,

Dynamic Programming is needed.

---

# Why Recursion is Slow

Suppose

```
111111111111111111
```

Every index can generate

two recursive calls.

The recursion tree grows

very quickly.

Many states

like

```
solve(10)

solve(15)

solve(20)
```

are computed

again and again.

This creates

a huge amount of

repeated work.

Time Complexity

```
O(2^n)
```

This is too slow.

---

# Recursive Solution

```python
class Solution:

    def solve(self, i, s):

        n = len(s)

        if i == n:
            return 1

        if s[i] == '0':
            return 0

        ways = self.solve(i + 1, s)

        if i + 1 < n and 10 <= int(s[i:i+2]) <= 26:
            ways += self.solve(i + 2, s)

        return ways

    def numDecodings(self, s):

        return self.solve(0, s)
```

---

# Complexity

Time

```
O(2^n)
```

Space

```
O(n)
```

because of the recursion stack.

---

# What's Next?

The recursion above solves the same state

```
solve(i)
```

multiple times.

In the next part,

we will optimize it using

```
Memoization
```

which reduces the time complexity from

```
O(2^n)

↓

O(n)
```
# Memoization (Top-Down DP)

The recursive solution solves the same state many times.

For example,

```
solve(2)
```

can be reached from multiple recursive paths.

Instead of solving it repeatedly,

we store the answer the first time it is computed.

This technique is called

```
Memoization
```

---

# DP State

```
dp[i]

=

Number of ways to decode

starting from index i.
```

Initially

```
dp

-1 -1 -1 -1 ...
```

where

```
-1

means

Not Computed Yet.
```

---

# Memoization Steps

## Step 1

Before solving,

check whether the answer already exists.

```python
if dp[i] != -1:
    return dp[i]
```

---

## Step 2

Handle the base cases.

If we have decoded the whole string,

```
return 1
```

If the current character is

```
'0'
```

then

```
return 0
```

because no decoding can start with zero.

---

## Step 3

Compute the answer.

Take one digit.

```python
ways = solve(i + 1)
```

If two digits form a valid number,

```
10

to

26
```

take two digits also.

```python
ways += solve(i + 2)
```

---

## Step 4

Store the answer.

```python
dp[i] = ways
```

Return it.

---

# Memoization Code

```python
class Solution:

    def numDecodings(self, s: str) -> int:

        n = len(s)

        dp = [-1] * n

        def solve(i):

            if i == n:
                return 1

            if s[i] == '0':
                return 0

            if dp[i] != -1:
                return dp[i]

            ways = solve(i + 1)

            if i + 1 < n and 10 <= int(s[i:i+2]) <= 26:
                ways += solve(i + 2)

            dp[i] = ways

            return dp[i]

        return solve(0)
```

---

# Dry Run (Memoization)

Suppose

```
s = "226"
```

Initially

```
solve(0)
```

↓

```
solve(1)

+

solve(2)
```

Later,

```
solve(1)
```

again calls

```
solve(2)
```

Instead of solving it again,

we directly return

```
dp[2]
```

Thus every state is computed only once.

---

# Time Complexity

```
O(n)
```

---

# Space Complexity

```
DP Array

+

Recursion Stack

=

O(n)
```

---

# Tabulation (Bottom-Up DP)

Instead of starting from the first character,

start from the smallest solved problem.

Remember the base case.

```
solve(n)

=

1
```

Therefore,

```
dp[n] = 1
```

Now build the answer backwards.

---

# DP State

```
dp[i]

=

Number of ways to decode

starting from index i.
```

---

# Transition

If

```
s[i]=='0'
```

then

```
dp[i]=0
```

Otherwise,

take one digit.

```
dp[i]=dp[i+1]
```

If two digits form

```
10

to

26
```

then

```
dp[i]+=dp[i+2]
```

---

# Why Do We Traverse Right to Left?

Look at the recurrence.

```
dp[i]

depends on

dp[i+1]

and

dp[i+2]
```

Before computing

```
dp[i]
```

we already need

```
dp[i+1]

dp[i+2]
```

Therefore,

we must fill the DP table

from

```
Right

↓

Left
```

---

# Dry Run (Tabulation)

Suppose

```
s = "226"
```

Length

```
3
```

Create

```
dp = [0,0,0,0]
```

Base case

```
dp[3]=1
```

Current table

```
Index

0 1 2 3

DP

0 0 0 1
```

---

## i = 2

Character

```
6
```

Single digit is valid.

```
dp[2]=dp[3]

=

1
```

Table

```
0 0 1 1
```

---

## i = 1

Character

```
2
```

Single digit

```
dp[2]=1
```

Two digits

```
26
```

Valid.

```
dp[1]

=

dp[2]

+

dp[3]

=

1+1

=

2
```

Table

```
0 2 1 1
```

---

## i = 0

Character

```
2
```

Single digit

```
dp[1]=2
```

Two digits

```
22
```

Valid.

```
dp[0]

=

dp[1]

+

dp[2]

=

2+1

=

3
```

Final DP

```
3 2 1 1
```

Answer

```
dp[0]

=

3
```

---

# Tabulation Code

```python
class Solution:

    def numDecodings(self, s: str) -> int:

        n = len(s)

        dp = [0] * (n + 1)

        dp[n] = 1

        for i in range(n - 1, -1, -1):

            if s[i] == '0':
                dp[i] = 0
                continue

            dp[i] = dp[i + 1]

            if i + 1 < n and 10 <= int(s[i:i+2]) <= 26:
                dp[i] += dp[i + 2]

        return dp[0]
```

---

# Time Complexity

```
O(n)
```

---

# Space Complexity

```
O(n)
```

---

# Space Optimization

Observe carefully.

```
dp[i]

depends only on

dp[i+1]

and

dp[i+2]
```

Nothing else.

Therefore,

storing the entire DP array is unnecessary.

We only need two future states.

Let

```
next

=

dp[i+1]
```

and

```
next2

=

dp[i+2]
```

For every iteration,

compute the current answer,

then shift the variables.

---

# Variable Meaning

```
next

↓

dp[i+1]
```

```
next2

↓

dp[i+2]
```

```
current

↓

dp[i]
```

---

# Dry Run

Suppose

```
s="226"
```

Initially

```
next = dp[3] = 1

next2 = 0
```

---

### i = 2

```
current

=

next

=

1
```

Update

```
next2 = next = 1

next = current = 1
```

---

### i = 1

```
current

=

next

=

1
```

Two digits

```
26
```

Valid.

```
current

=

1+1

=

2
```

Update

```
next2 = 1

next = 2
```

---

### i = 0

```
current

=

next

=

2
```

Two digits

```
22
```

Valid.

```
current

=

2+1

=

3
```

Update

```
next2 = 2

next = 3
```

Answer

```
3
```

---

# Space Optimized Code

```python
class Solution:

    def numDecodings(self, s: str) -> int:

        n = len(s)

        next = 1      # dp[n]
        next2 = 0     # dp[n+1] (imaginary)

        for i in range(n - 1, -1, -1):

            if s[i] == '0':
                current = 0

            else:

                current = next

                if i + 1 < n and 10 <= int(s[i:i+2]) <= 26:
                    current += next2

            next2 = next
            next = current

        return next
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

# DP Progression

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

# Key Observation

The entire problem is built on one simple idea.

At every index,

you have only two choices.

```
Take One Digit

or

Take Two Digits
```

If a choice is valid,

solve the remaining smaller string.

If you solve the same remaining string repeatedly,

store its answer.

That is exactly

```
Dynamic Programming.
```

<br/><br/><br/><br/><br/>

---

# 1406. Stone Game III

# Difficulty

Hard

# Pattern

- Dynamic Programming
- Game Theory
- Minimax DP
- Score Difference DP

---

# Problem Statement

Alice and Bob are playing another stone game.

There are several stones arranged in a straight line.

Each stone has a value stored in the array

```
stoneValue
```

Some values may even be negative.

Initially,

both players have

```
Score = 0
```

Alice starts first.

On every turn,

the current player can take

- 1 stone
- 2 stones
- 3 stones

but only from the **front** of the remaining stones.

After taking the stones,

their values are added to that player's score.

The game continues until no stones remain.

Both Alice and Bob play **optimally**, meaning they always choose the move that gives them the best possible final outcome.

Return

```
"Alice"
```

if Alice finishes with a higher score.

Return

```
"Bob"
```

if Bob finishes with a higher score.

Return

```
"Tie"
```

if both finish with the same score.

---

# Examples

## Example 1

Input

```
stoneValue = [1,2,3,7]
```

Alice has three choices.

Take

```
1
```

or

```
1+2
```

or

```
1+2+3
```

No matter what Alice does,

Bob can eventually finish with a higher score.

Answer

```
Bob
```

---

## Example 2

Input

```
stoneValue = [1,2,3,-9]
```

If Alice immediately takes

```
1+2+3
```

then Bob is forced to take

```
-9
```

Alice wins.

Answer

```
Alice
```

---

## Example 3

Input

```
stoneValue = [1,2,3,6]
```

Both players can finish with exactly the same score.

Answer

```
Tie
```

---

# Understanding the Problem Like a Common Man

Imagine the stones are lying in a line.

```
+----+----+----+----+
| 1  | 2  | 3  | 7  |
+----+----+----+----+
```

Alice begins.

She may take

```
1 stone
```

or

```
2 stones
```

or

```
3 stones
```

Suppose she takes

```
1
```

Now the remaining stones become

```
+----+----+----+
| 2  | 3  | 7  |
+----+----+----+
```

Now Bob gets exactly the same choices.

Again,

he can take

```
1

or

2

or

3
```

stones.

This continues until all stones disappear.

The winner is simply the person whose total score is larger.

---

# Important Observation

This is **not** a greedy problem.

It is a

```
Thinking Ahead
```

problem.

Whenever Alice makes a move,

she must also think

```
"What will Bob do after this?"
```

Bob also thinks the same way.

Therefore,

every decision depends on

future decisions.

That is exactly where

```
Dynamic Programming
```

comes into the picture.

---

# Thinking Like a Beginner

Suppose

```
stoneValue = [1,2,3,7]
```

Initially

```
1 2 3 7
↑
```

Alice has three choices.

---

## Choice 1

Take one stone.

```
1
```

Remaining

```
2 3 7
↑
```

Now Bob starts.

---

## Choice 2

Take two stones.

```
1+2=3
```

Remaining

```
3 7
↑
```

Bob starts.

---

## Choice 3

Take three stones.

```
1+2+3=6
```

Remaining

```
7
↑
```

Bob starts.

Alice wants to know

Which of these three choices finally makes her win?

Notice,

she cannot decide immediately.

She must know

```
How Bob will play.
```

---

# Why Greedy Fails

A very common first idea is

```
Always take the maximum sum available.
```

For example,

```
1 2 3 7
```

The immediate sums are

Take one

```
1
```

Take two

```
3
```

Take three

```
6
```

Greedy says

```
Take 6
```

Sounds correct.

But then Bob gets

```
7
```

Final scores

```
Alice = 6

Bob = 7
```

Bob wins.

Greedy only thinks about

```
Current Turn
```

The problem asks about

```
Entire Game
```

These are completely different.

---

# The Biggest Intuition

Instead of asking

```
Which move gives me

the maximum score now?
```

Ask

```
If I choose this move,

what is the BEST score

my opponent can get later?
```

Now the problem becomes

```
Current Gain

minus

Opponent's Future Gain
```

This single observation completely changes the problem.

---

# Why Storing Alice's Score and Bob's Score Separately is Difficult

Suppose

Alice currently has

```
10
```

Bob currently has

```
7
```

Can we say Alice will win?

No.

Many stones are still left.

Bob may later collect

```
20
```

points.

Therefore,

storing

```
Alice Score

Bob Score
```

is not a good DP state.

There are too many possibilities.

---

# The Smart Trick

Instead of storing

```
Alice Score

Bob Score
```

store only

```
Current Player's Advantage
```

Suppose

Current player finally scores

```
15
```

Opponent finally scores

```
10
```

Advantage

```
15 - 10 = 5
```

Suppose

Opponent scores more.

Example

```
10 - 18 = -8
```

Negative means

the current player loses.

This single number is enough.

---

# Defining the DP State

Let

```
dp(i)
```

represent

```
Maximum Score Difference

(Current Player − Opponent)

starting from index i.
```

Notice

We are not storing

```
Alice

or

Bob
```

The DP always represents

```
Whoever's turn it is.
```

That is why the same recurrence works for both players.

---

# Understanding the Score Difference

Suppose

Current player takes

```
5
```

points.

Now,

the opponent starts playing.

Suppose

the opponent can achieve an advantage of

```
3
```

That means

Opponent will finally beat the current player by

```
3
```

Therefore,

Current Player's final advantage becomes

```
5 - 3 = 2
```

Notice the subtraction.

That is why every game DP recurrence contains

```
Minus DP
```

instead of

```
Plus DP
```

This is the biggest idea in this problem.

---

# Building the Recurrence

Suppose we are standing at

```
i
```

Current player has three choices.

---

## Choice 1

Take one stone.

Immediate gain

```
stoneValue[i]
```

Now opponent starts from

```
i+1
```

Opponent's advantage

```
dp(i+1)
```

Current player's final advantage

```
stoneValue[i]

-

dp(i+1)
```

---

## Choice 2

Take two stones.

Immediate gain

```
stoneValue[i]

+

stoneValue[i+1]
```

Opponent starts from

```
i+2
```

Final advantage

```
stoneValue[i]

+

stoneValue[i+1]

-

dp(i+2)
```

---

## Choice 3

Take three stones.

Immediate gain

```
stoneValue[i]

+

stoneValue[i+1]

+

stoneValue[i+2]
```

Opponent starts from

```
i+3
```

Final advantage

```
stoneValue[i]

+

stoneValue[i+1]

+

stoneValue[i+2]

-

dp(i+3)
```

---

# Recurrence Relation

Therefore,

```
dp(i)

=

max(

Take One,

Take Two,

Take Three

)
```

or mathematically,

```
dp(i)

=

max(

stone[i]

-

dp(i+1),

stone[i]+stone[i+1]

-

dp(i+2),

stone[i]+stone[i+1]+stone[i+2]

-

dp(i+3)

)
```

This is the heart of the entire problem.

---

# Finding the Base Case

Suppose

```
i == n
```

No stones remain.

Current player cannot gain anything.

Difference becomes

```
0
```

Therefore,

```
if i >= n

↓

return 0
```

---

# Recursive Thinking

Suppose

```
stoneValue = [1,2,3,7]
```

Initially,

Alice is at

```
1 2 3 7
↑
```

She explores

Take

```
1
```

↓

Bob starts from

```
2 3 7
```

Take

```
1+2
```

↓

Bob starts from

```
3 7
```

Take

```
1+2+3
```

↓

Bob starts from

```
7
```

Bob again explores

three choices.

Again,

Alice explores three choices.

Eventually,

all recursive paths reach

```
No Stones Left
```

which returns

```
0
```

---

# Recursion Tree

For

```
stoneValue = [1,2,3]
```

```
                     dp(0)
              /         |         \
          Take1      Take2      Take3
            |           |           |
         dp(1)       dp(2)       dp(3)
        / | \        / | \         |
      ... ...      ... ...        0
```

Notice

```
dp(2)
```

can be reached

from multiple paths.

The same state gets solved

again and again.

These are

```
Overlapping Subproblems.
```

Hence,

```
Dynamic Programming
```

is required.

---

# Recursive Solution

```python
from functools import cache

class Solution:

    def stoneGameIII(self, stoneValue):

        n = len(stoneValue)

        @cache
        def solve(i):

            if i >= n:
                return 0

            best = float("-inf")
            curr = 0

            for k in range(3):

                if i + k >= n:
                    break

                curr += stoneValue[i + k]

                best = max(best, curr - solve(i + k + 1))

            return best

        diff = solve(0)

        if diff > 0:
            return "Alice"

        elif diff < 0:
            return "Bob"

        return "Tie"
```

---

# Complexity of Pure Recursion

Without memoization,

the recursion explores many repeated states.

Time Complexity

```
Exponential
```

With memoization,

every index is solved only once.

Time Complexity becomes

```
O(n)
```

---

# What's Next?

In the next part,

we will optimize this recursive solution using

```
Memoization

↓

Tabulation

↓

Space Optimization
```

and perform a complete dry run to understand exactly how the DP array is built.

# Memoization (Top-Down DP)

The recursive solution repeatedly solves the same state.

For example,

```
dp(3)
```

can be reached from multiple recursive paths.

Instead of solving it every time,

we store the answer the first time it is computed.

This technique is called

```
Memoization
```

---

# DP State

```
dp(i)

=

Maximum Score Difference

(Current Player − Opponent)

starting from index i.
```

Initially,

every state is

```
Not Computed
```

---

# Memoization Steps

## Step 1

If no stones remain,

```
return 0
```

because neither player can gain any more points.

---

## Step 2

If this state has already been solved,

return the stored answer.

```python
if dp[i] != INF:
    return dp[i]
```

---

## Step 3

Try taking

```
1 stone
```

Compute the current score.

Subtract the opponent's best possible advantage.

---

## Step 4

Try taking

```
2 stones
```

Again subtract the opponent's future advantage.

---

## Step 5

Try taking

```
3 stones
```

Again subtract the opponent's future advantage.

---

## Step 6

Store the maximum among all three choices.

---

# Memoization Code

```python
from functools import cache

class Solution:

    def stoneGameIII(self, stoneValue):

        n = len(stoneValue)

        @cache
        def solve(i):

            if i >= n:
                return 0

            best = float("-inf")
            current = 0

            for k in range(3):

                if i + k >= n:
                    break

                current += stoneValue[i + k]

                best = max(best, current - solve(i + k + 1))

            return best

        difference = solve(0)

        if difference > 0:
            return "Alice"

        elif difference < 0:
            return "Bob"

        return "Tie"
```

---

# Dry Run (Memoization)

Suppose

```
stoneValue = [-1,-2,-3]
```

Initially

```
solve(0)
```

---

## Choice 1

Take

```
-1
```

Immediate gain

```
-1
```

Remaining

```
[-2,-3]
```

Opponent advantage

```
solve(1)
```

Difference

```
-1 - solve(1)
```

---

## Choice 2

Take

```
-1 + (-2)

=

-3
```

Remaining

```
[-3]
```

Difference

```
-3 - solve(2)
```

---

## Choice 3

Take

```
-1 + (-2) + (-3)

=

-6
```

Remaining

```
[]
```

Difference

```
-6 - solve(3)

=

-6
```

---

Now solve

```
solve(2)
```

Remaining

```
[-3]
```

Only one move.

Take

```
-3
```

Opponent gets

```
0
```

Difference

```
-3
```

Therefore

```
solve(2) = -3
```

---

Now solve

```
solve(1)
```

Remaining

```
[-2,-3]
```

Take one

```
-2

-

(-3)

=

1
```

Take two

```
-5

-

0

=

-5
```

Choose maximum

```
1
```

Therefore

```
solve(1)=1
```

---

Now return to

```
solve(0)
```

Choice 1

```
-1

-

1

=

-2
```

Choice 2

```
-3

-

(-3)

=

0
```

Choice 3

```
-6

-

0

=

-6
```

Maximum

```
0
```

Therefore

```
solve(0)=0
```

Difference

```
0
```

Answer

```
Tie
```

---

# Time Complexity

Every index is computed only once.

```
O(n)
```

---

# Space Complexity

```
DP Cache

+

Recursion Stack

=

O(n)
```

---

# Tabulation (Bottom-Up DP)

Instead of solving recursively,

start from the smallest problem.

We already know

```
dp[n]=0
```

because

there are no stones left.

Now build the answer backwards.

---

# DP State

```
dp[i]

=

Maximum Score Difference

(Current Player − Opponent)

starting from index i.
```

---

# Transition

At every position,

try taking

```
1

2

3
```

stones.

Compute

```
Current Score

-

Future Difference
```

Store the maximum.

---

# Why Do We Traverse Right to Left?

Observe the recurrence.

```
dp[i]

depends on

dp[i+1]

dp[i+2]

dp[i+3]
```

Therefore,

before computing

```
dp[i]
```

we must already know

```
dp[i+1]

dp[i+2]

dp[i+3]
```

Hence,

fill the table

from

```
Right

↓

Left
```

---

# Tabulation Dry Run

Suppose

```
stoneValue = [-1,-2,-3]
```

Length

```
n=3
```

Create

```
dp=[0,0,0,0]
```

Initially

```
Index

0 1 2 3

DP

0 0 0 0
```

---

## i = 2

Current

```
-3
```

Take one

```
-3

-

dp[3]

=

-3
```

Best

```
-3
```

Table

```
0 0 -3 0
```

---

## i = 1

Current

```
-2
```

Take one

```
-2

-

(-3)

=

1
```

Take two

```
-5

-

0

=

-5
```

Best

```
1
```

Table

```
0 1 -3 0
```

---

## i = 0

Current

```
-1
```

Take one

```
-1

-

1

=

-2
```

Take two

```
-3

-

(-3)

=

0
```

Take three

```
-6

-

0

=

-6
```

Best

```
0
```

Final DP

```
0 1 -3 0
```

Answer

```
dp[0]=0
```

Result

```
Tie
```

---

# Tabulation Code

```python
class Solution:

    def stoneGameIII(self, stoneValue):

        n = len(stoneValue)

        dp = [0] * (n + 1)

        for i in range(n - 1, -1, -1):

            best = float("-inf")
            current = 0

            for k in range(3):

                if i + k >= n:
                    break

                current += stoneValue[i + k]

                best = max(best, current - dp[i + k + 1])

            dp[i] = best

        if dp[0] > 0:
            return "Alice"

        elif dp[0] < 0:
            return "Bob"

        return "Tie"
```

---

# Time Complexity

```
O(n)
```

---

# Space Complexity

```
O(n)
```

---

# Space Optimization

Observe carefully.

```
dp[i]

depends only on

dp[i+1]

dp[i+2]

dp[i+3]
```

Nothing else.

Therefore,

storing the entire DP array is unnecessary.

We only need

```
Three Future States
```

---

# Variable Meaning

```
next1

↓

dp[i+1]
```

```
next2

↓

dp[i+2]
```

```
next3

↓

dp[i+3]
```

---

# Space Optimized Code

```python
class Solution:

    def stoneGameIII(self, stoneValue):

        n = len(stoneValue)

        next1 = 0
        next2 = 0
        next3 = 0

        for i in range(n - 1, -1, -1):

            best = float("-inf")
            current = 0

            for k in range(3):

                if i + k >= n:
                    break

                current += stoneValue[i + k]

                if k == 0:
                    future = next1
                elif k == 1:
                    future = next2
                else:
                    future = next3

                best = max(best, current - future)

            next3 = next2
            next2 = next1
            next1 = best

        if next1 > 0:
            return "Alice"

        elif next1 < 0:
            return "Bob"

        return "Tie"
```

---

# Space Optimization Dry Run

Initially

```
next1 = 0

next2 = 0

next3 = 0
```

Process

```
-3
```

Current Difference

```
-3
```

Update

```
next1=-3
```

---

Process

```
-2
```

Difference

```
max(

-2-(-3),

-5

)

=

1
```

Update

```
next1=1

next2=-3
```

---

Process

```
-1
```

Difference

```
max(

-1-1,

-3-(-3),

-6

)

=

0
```

Update

```
next1=0
```

Answer

```
Tie
```

---

# Complexity

## Memoization

```
Time

O(n)

Space

O(n)
```

---

## Tabulation

```
Time

O(n)

Space

O(n)
```

---

## Space Optimization

```
Time

O(n)

Space

O(1)
```

---

# Pattern Recognition

Whenever you see

- Two Players
- Alternate Turns
- Both Play Optimally
- Take 1 / 2 / 3 Moves
- Maximum Final Score

Immediately think

```
Game DP
```

Never think greedily.

Instead,

store

```
Current Player's Advantage
```

instead of

```
Alice Score

Bob Score
```

---

# Common Mistakes

❌ Maximizing the immediate score.

❌ Storing Alice's score and Bob's score separately.

❌ Forgetting to subtract the opponent's future advantage.

❌ Returning the maximum collected score instead of the score difference.

❌ Traversing the DP table from left to right.

---

# Similar Problems

- Predict the Winner
- Stone Game
- Stone Game II
- Stone Game VII
- Stone Game VIII
- Stone Game IX

---

# Quick Revision

### DP State

```
dp(i)

=

Maximum Score Difference

(Current Player − Opponent)
```

---

### Recurrence

```
Take 1

↓

current

-

dp(i+1)
```

```
Take 2

↓

current

-

dp(i+2)
```

```
Take 3

↓

current

-

dp(i+3)
```

Choose

```
Maximum
```

---

### Base Case

```
i>=n

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

Whenever a problem involves **two players taking turns optimally**, don't try to maximize only the current move.

Instead, think:

> **"After I make this move, my opponent will also play optimally. My true gain is my immediate score minus my opponent's best future advantage."**

This idea naturally leads to the **Score Difference DP** pattern, which is the key to solving many two-player game problems efficiently.