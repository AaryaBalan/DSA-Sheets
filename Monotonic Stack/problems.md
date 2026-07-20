# Monotonic Stack Problems

Welcome to the monotonic stack problems section! Here you will find various data structure and algorithm problems related to Monotonic Stack, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [316. Remove Duplicate Letters](#316-remove-duplicate-letters)
- [1574. Shortest Subarray to be Removed to Make Array Sorted](#1574-shortest-subarray-to-be-removed-to-make-array-sorted)
- [1673. Find the Most Competitive Subsequence](#1673-find-the-most-competitive-subsequence)
- [853. Car Fleet](#853-car-fleet)
- [962. Maximum Width Ramp](#962-maximum-width-ramp)
- [581. Shortest Unsorted Continuous Subarray](#581-shortest-unsorted-continuous-subarray)
- [456. 132 Pattern](#456-132-pattern)
- [907. Sum of Subarray Minimums](#907-sum-of-subarray-minimums)
- [1130. Minimum Cost Tree From Leaf Values](#1130-minimum-cost-tree-from-leaf-values)
- [1856. Maximum Subarray Min-Product](#1856-maximum-subarray-min-product)
- [3523. Make Array Non-decreasing](#3523-make-array-non-decreasing)

<br><br><br><br><br>

---

# 316. Remove Duplicate Letters

> **Difficulty:** Medium  
> **Technique:** Greedy + Monotonic Stack + Hash Set + Last Occurrence

---

# 📚 Problem Statement

Given a string `s`, remove duplicate letters so that:

1. Every letter appears **exactly once**.
2. The final string is the **smallest lexicographical string** possible.

---

## Example 1

Input

```text
s = "bcabc"
```

Output

```text
abc
```

---

## Example 2

Input

```text
s = "cbacdcbc"
```

Output

```text
acdb
```

---

# 🤔 What is Lexicographical Order?

Think of it like dictionary order.

```
abc < acb < bac < bca < cab
```

Smaller letters should appear as early as possible.

Example

```
abc
```

is smaller than

```
bac
```

because

```
a < b
```

---

# 🎯 What is the Goal?

Suppose

```
s = bcabc
```

Characters are

```
b
c
a
b
c
```

We want

```
Every character only once.
```

Possible answers

```
bca
cab
abc
```

Smallest among them

```
abc
```

---

# ❌ First Thought (Wrong)

We can use a set.

```
set(s)

↓

{'a','b','c'}
```

Sort it

```
abc
```

Will this always work?

No.

Example

```
cbacdcbc
```

Sorted unique

```
abcd
```

But

```
abcd
```

is **NOT** a subsequence of the original string.

We must preserve the original order.

---

# ❌ Second Thought

Take the first occurrence.

```
cbacdcbc

Take first occurrence

c
b
a
d

↓

cbad
```

Not smallest.

---

# 💡 Key Observation

We cannot freely rearrange letters.

We can only decide

```
Keep

or

Remove
```

while maintaining order.

So we need a greedy strategy.

---

# 🧠 Greedy Thinking

Suppose

```
Stack

c
```

Now we see

```
b
```

Question

Should we keep

```
c
```

before

```
b
```

No.

Because

```
b < c
```

Smaller character should come first.

But...

Can we remove c?

Only if another c appears later.

Otherwise we lose c forever.

This gives our rule.

---

# ⭐ The Three Rules

Whenever a new character arrives

## Rule 1

If it is already inside the answer

Ignore it.

Example

```
abcbc

Second b

↓

Ignore
```

---

## Rule 2

If current character is smaller than stack top

```
Current

↓

b

Top

↓

c
```

We would like

```
bc

↓

cb ❌

Better

↓

bc ✅
```

So pop.

BUT...

Only if c appears again later.

---

## Rule 3

If stack top never appears again

Don't pop.

Because

```
Lose character forever.
```

---

# 🔥 Why Do We Need Last Occurrence?

We must know

```
Will this character appear again?
```

Example

```
cbacdcbc
```

Last positions

```
c → 7

b → 6

a → 2

d → 4
```

Now while processing

if

```
current index

<

last occurrence
```

then

```
Safe to remove.
```

Otherwise

Don't remove.

---

# 🧠 Data Structures Needed

## 1. Stack

Build answer.

---

## 2. Set

Check quickly

```
Already in answer?
```

O(1)

---

## 3. Dictionary

Store last occurrence.

Example

```
{
'c':7,
'b':6,
'a':2,
'd':4
}
```

---

# 🚶 Dry Run 1

```
bcabc
```

---

## Step 1

```
b
```

Stack

```
b
```

---

## Step 2

```
c
```

Top

```
b
```

Is

```
c < b ?

No.
```

Push

```
bc
```

---

## Step 3

```
a
```

Current

```
a
```

Top

```
c
```

```
a < c

Yes
```

Does c appear later?

Yes

Pop

```
b
```

Now

Top

```
b
```

```
a < b

Yes
```

Does b appear later?

Yes

Pop

Stack

```
[]
```

Push

```
a
```

---

## Step 4

Current

```
b
```

Push

```
ab
```

---

## Step 5

Current

```
c
```

Push

```
abc
```

Finished.

Answer

```
abc
```

---

# 🚶 Dry Run 2

```
cbacdcbc
```

Last occurrence

```
c → 7
b → 6
a → 2
d → 4
```

---

### Read c

```
c
```

---

### Read b

```
b < c

Yes

c appears later

Pop
```

Stack

```
b
```

---

### Read a

```
a < b

Yes

b appears later

Pop
```

Stack

```
a
```

---

### Read c

Push

```
ac
```

---

### Read d

Push

```
acd
```

---

### Read c

Already exists

Ignore

---

### Read b

Compare

```
b < d

Yes
```

Can remove d?

No

Last occurrence of d

Already passed.

Cannot pop.

Push

```
acdb
```

---

Remaining characters

Already present

Ignore.

Answer

```
acdb
```

---

# 🎯 Algorithm

1. Store last occurrence of every character.
2. Traverse string.
3. Ignore duplicate characters already in stack.
4. While

```
Current character

<

Stack Top

AND

Stack Top appears later
```

Pop.

5. Push current character.

6. Return stack.

---

# ✅ Python Solution

```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:

        last = {}

        # Store last occurrence
        for i, ch in enumerate(s):
            last[ch] = i

        stack = []
        visited = set()

        for i, ch in enumerate(s):

            # Ignore duplicates
            if ch in visited:
                continue

            # Maintain lexicographical order
            while (
                stack
                and ch < stack[-1]
                and last[stack[-1]] > i
            ):
                visited.remove(stack.pop())

            stack.append(ch)
            visited.add(ch)

        return "".join(stack)
```

---

# ⏱ Complexity

### Time

```
O(N)
```

Each character

- pushed once
- popped at most once

---

### Space

```
O(26)

≈ O(1)
```

because only lowercase English letters exist.

---

# 🧠 Pattern Recognition

Whenever you see

- Remove duplicate characters
- Keep relative order
- Smallest lexicographical string
- Remove characters greedily

Think

```
Greedy

+

Monotonic Stack

+

Last Occurrence

+

Visited Set
```

---

# 📝 Key Takeaways

```
Need unique characters
        ↓
Need smallest lexicographical order
        ↓
Maintain answer using a stack
        ↓
If current character is smaller
        ↓
Pop larger characters
        ↓
But only if they appear again later
        ↓
Use last occurrence dictionary
        ↓
Use visited set to avoid duplicates
        ↓
Final Answer
```

<br/><br/><br/><br/><br/>

---

# 1574. Shortest Subarray to be Removed to Make Array Sorted

> **Difficulty:** Medium  
> **Technique:** Two Pointers + Greedy

---

# 📚 Problem Statement

You are given an integer array.

You are allowed to remove **exactly one contiguous subarray** (it can also be empty).

After removing that subarray, the remaining elements should become **non-decreasing**.

Return the **minimum length** of the subarray to remove.

---

## Example

Input

```text
arr = [1,2,3,10,4,2,3,5]
```

Output

```text
3
```

Remove

```text
[10,4,2]
```

Remaining

```text
[1,2,3,3,5]
```

Sorted.

---

# 🤔 Understanding the Question

Notice carefully.

We are **NOT** sorting the array.

We are only allowed to

```
Remove ONE continuous subarray.
```

Example

```
1 2 3 10 4 2 3 5
```

You can remove

```
10 4 2
```

Result

```
1 2 3 3 5
```

Sorted.

---

You CANNOT remove

```
10

and

2
```

because that is **not contiguous**.

---

# Step 1 : Brute Force Thinking

What will every beginner think?

```
Try every possible subarray.

Remove it.

Check if remaining array is sorted.
```

Example

```
Remove

[1]

Check

Remove

[1,2]

Check

Remove

[2,3]

Check

...
```

How many subarrays?

```
N²
```

Checking sorted again

```
O(N)
```

Overall

```
O(N³)
```

Impossible.

---

# Step 2 : Observe the Array

Take

```
1 2 3 10 4 2 3 5
```

Look carefully.

```
1 2 3 10
```

Already sorted.

```
4 2 3 5
```

Not sorted.

But

```
2 3 5
```

at the end

is also sorted.

Interesting.

The array already has

```
Sorted Prefix

+

Bad Middle

+

Sorted Suffix
```

This is the biggest observation.

---

# Step 3 : Find the Longest Sorted Prefix

Start from left.

```
1 < 2 ✓

2 < 3 ✓

3 < 10 ✓

10 < 4 ✗
```

Stop.

Prefix ends here.

```
1 2 3 10
```

Index

```
0 1 2 3
```

---

# Step 4 : Find the Longest Sorted Suffix

Start from right.

```
3 < 5 ✓

2 < 3 ✓

4 < 2 ✗
```

Stop.

Suffix starts here.

```
2 3 5
```

Index

```
5 6 7
```

---

Current situation

```
Prefix

1 2 3 10

-----------

Bad Middle

4

-----------

Suffix

2 3 5
```

---

# Step 5 : Can We Remove Everything Between Them?

Yes.

Remove

```
4 2
```

No.

Because

```
10

>

3
```

Still unsorted.

Need something smarter.

---

# Step 6 : The Biggest Idea

Imagine

```
Prefix

1 2 3 10
```

We don't have to keep the ENTIRE prefix.

Maybe

```
1 2 3
```

is enough.

Similarly,

maybe we don't keep the entire suffix.

We need to connect them.

Think like joining two roads.

```
Left Road

↓

Right Road
```

Can they connect?

Only if

```
Left Value

<=

Right Value
```

---

# Step 7 : Two Pointer Thinking

Pointer

```
i
```

moves inside prefix.

Pointer

```
j
```

moves inside suffix.

Initially

```
i

↓

1
```

```
j

↓

2
```

Need

```
arr[i]

<=

arr[j]
```

If true

they connect.

Otherwise

move

```
j
```

forward.

---

# Dry Run

Array

```
1 2 3 10 4 2 3 5
```

Prefix ends

```
i = 3
```

Suffix starts

```
j = 5
```

---

Current

```
10

2
```

```
10<=2

No
```

Move

```
j
```

---

```
10

3
```

Still

No.

Move

```
j
```

---

```
10

5
```

Still

No.

End.

So

10

cannot stay.

Move

```
i
```

back?

No.

Instead,

restart

with

```
i=2
```

Now

```
3

2
```

No.

Move

```
j
```

```
3

3

Yes
```

Success.

Remaining

```
1 2 3

3 5
```

Remove

```
10 4 2
```

Length

```
3
```

This becomes the answer.

---

# Why Two Pointers?

Both parts

are already sorted.

Whenever two sorted arrays are involved,

always ask yourself

```
Can I merge them?

Can I compare them with two pointers?
```

Exactly the same idea as Merge Sort.

---

# Step 8 : Initial Answer

Suppose

remove

everything after prefix.

```
Keep only prefix.
```

Length removed

```
n-prefixLength
```

Suppose

remove everything before suffix.

```
Keep only suffix.
```

Length removed

```
suffixStart
```

Take minimum.

Then improve using two pointers.

---

# Algorithm

### Step 1

Find longest sorted prefix.

---

### Step 2

Find longest sorted suffix.

---

### Step 3

If entire array sorted

Return

```
0
```

---

### Step 4

Initial answer

```
Remove prefix

or

Remove suffix
```

---

### Step 5

Use two pointers

```
Prefix Pointer

↓

Suffix Pointer
```

Whenever

```
arr[i]

<=

arr[j]
```

Possible removal

```
j-i-1
```

Update answer.

Move

```
i
```

Otherwise

move

```
j
```

---

# Why

```
j-i-1
```

Suppose

```
1 2 3

10 4 2

3 5
```

Pointers

```
i

↓

3
```

```
j

↓

3
```

Everything between them

must disappear.

Indices

```
i=2

j=6
```

Remove

```
3

4

5
```

Length

```
6-2-1

=

3
```

General Formula

```
Remove Length

=

j-i-1
```

Remember this.

---

# Complete Step-by-Step Dry Run

Let's dry run the algorithm very slowly.

Example

```text
arr = [1,2,3,10,4,2,3,5]
```

Index

```text
0 1 2 3 4 5 6 7
```

---

# Step 1 : Find the Longest Sorted Prefix

Start from the beginning.

Current

```text
1 2
```

Check

```text
1 <= 2
```

✅ Yes

Move ahead.

---

Current

```text
2 3
```

Check

```text
2 <= 3
```

✅ Yes

Move ahead.

---

Current

```text
3 10
```

Check

```text
3 <= 10
```

✅ Yes

Move ahead.

---

Current

```text
10 4
```

Check

```text
10 <= 4
```

❌ No

Stop.

Prefix is

```text
1 2 3 10
```

Prefix ends at

```text
left = 3
```

Visualization

```text
1 2 3 10 | 4 2 3 5
          ^
        left
```

---

# Step 2 : Find the Longest Sorted Suffix

Now start from the end.

Current

```text
3 5
```

Check

```text
3 <= 5
```

✅ Yes

Move left.

---

Current

```text
2 3
```

Check

```text
2 <= 3
```

✅ Yes

Move left.

---

Current

```text
4 2
```

Check

```text
4 <= 2
```

❌ No

Stop.

Suffix begins at

```text
right = 5
```

Suffix

```text
2 3 5
```

Visualization

```text
1 2 3 10 4 | 2 3 5
             ^
           right
```

---

# Step 3 : Initial Answer

There are two easy choices.

---

## Choice 1

Keep only prefix.

Remove

```text
4 2 3 5
```

Length

```text
4
```

Formula

```text
n-left-1

=

8-3-1

=

4
```

---

## Choice 2

Keep only suffix.

Remove

```text
1 2 3 10 4
```

Length

```text
5
```

Formula

```text
right

=

5
```

---

Current answer

```text
min(4,5)

=

4
```

So

```text
ans = 4
```

---

# Step 4 : Start Two Pointers

Initialize

```text
i = 0

j = right = 5
```

Current

```text
1 2 3 10 4 2 3 5
↑         ↑
i         j
```

We ask

```text
Can these two values be connected?
```

Check

```text
arr[i]

=

1
```

```text
arr[j]

=

2
```

Check

```text
1 <= 2
```

✅ Yes

So

everything between them can be removed.

Remove

```text
2 3 10 4
```

Indices

```text
1 2 3 4
```

Length

```text
5-0-1

=

4
```

Update

```text
ans

=

min(4,4)

=

4
```

Move

```text
i++
```

Now

```text
i = 1
```

---

# Step 5

Current

```text
1 2 3 10 4 2 3 5
  ↑       ↑
  i       j
```

Values

```text
2

2
```

Check

```text
2 <= 2
```

✅ Yes

Remove

```text
3 10 4
```

Length

```text
5-1-1

=

3
```

Update

```text
ans

=

3
```

Move

```text
i++
```

Now

```text
i=2
```

---

# Step 6

Current

```text
1 2 3 10 4 2 3 5
    ↑     ↑
    i     j
```

Values

```text
3

2
```

Check

```text
3 <= 2
```

❌ No

Cannot connect.

If we remove the middle,

array becomes

```text
1 2 3 2 3 5
```

Still unsorted.

Need a bigger suffix value.

Move

```text
j++
```

Now

```text
j=6
```

---

# Step 7

Current

```text
1 2 3 10 4 2 3 5
    ↑       ↑
    i       j
```

Values

```text
3

3
```

Check

```text
3 <= 3
```

✅ Yes

Now

remove

```text
10 4 2
```

Length

```text
6-2-1

=

3
```

Update

```text
ans

=

min(3,3)

=

3
```

Move

```text
i++
```

Now

```text
i=3
```

---

# Step 8

Current

```text
1 2 3 10 4 2 3 5
      ↑     ↑
      i     j
```

Values

```text
10

3
```

Check

```text
10 <= 3
```

❌ No

Move

```text
j++
```

Now

```text
j=7
```

---

# Step 9

Current

```text
1 2 3 10 4 2 3 5
      ↑       ↑
      i       j
```

Values

```text
10

5
```

Check

```text
10 <= 5
```

❌ No

Move

```text
j++
```

Now

```text
j=8
```

Loop ends.

---

# Final Answer

```text
3
```

Remove

```text
10 4 2
```

Remaining

```text
1 2 3 3 5
```

Sorted ✅

---

# Why Do We Move Only One Pointer?

When

```text
arr[i] <= arr[j]
```

The current suffix already works.

Can we keep **more** of the prefix?

Yes.

So move

```text
i++
```

---

When

```text
arr[i] > arr[j]
```

The current suffix value is too small.

Keeping the same prefix won't help.

We need a larger value on the right.

So move

```text
j++
```

---

# Visual Summary

```text
Prefix                     Suffix

1 2 3 10 | 4 | 2 3 5
          ↑     ↑
          i     j

10 > 2

Move j
```

```text
1 2 3 10 | 4 2 | 3 5
          ↑       ↑
          i       j

10 > 3

Move j
```

```text
1 2 3 | 10 4 2 | 3 5
      ↑         ↑
      i         j

3 <= 3

Answer = 3
```

This is the optimal removal.

Example

```
1 2 3 10 4 2 3 5
```

Prefix

```
Ends at

3
```

Suffix

```
Starts at

5
```

Initial answer

```
min(

8-4,

5

)

=

4
```

Now merge.

```
1

2

✓

Answer

4
```

```
2

2

✓

Answer

3
```

```
3

3

✓

Answer

3
```

```
10

5

✗
```

Done.

Final

```
3
```

---

# Python Solution

```python
class Solution:
    def findLengthOfShortestSubarray(self, arr):
        n = len(arr)

        # Find longest sorted prefix
        left = 0
        while left + 1 < n and arr[left] <= arr[left + 1]:
            left += 1

        # Already sorted
        if left == n - 1:
            return 0

        # Find longest sorted suffix
        right = n - 1
        while right > 0 and arr[right - 1] <= arr[right]:
            right -= 1

        # Remove either prefix or suffix
        ans = min(n - left - 1, right)

        i = 0
        j = right

        while i <= left and j < n:

            if arr[i] <= arr[j]:
                ans = min(ans, j - i - 1)
                i += 1
            else:
                j += 1

        return ans
```

---

# Complexity Analysis

### Time Complexity

Finding Prefix

```
O(N)
```

Finding Suffix

```
O(N)
```

Two Pointer Merge

```
O(N)
```

Overall

```
O(N)
```

---

### Space Complexity

```
O(1)
```

---

# 🎯 Pattern Recognition

Whenever you see

```
Remove one subarray

↓

Remaining should satisfy a property
```

Ask yourself

```
Can I keep

a Prefix

+

a Suffix

and remove the middle?
```

If both sides are already sorted,

think

```
Two Pointers

+

Merge
```

---

# 🧠 Interview Thinking

Instead of asking

```
What should I remove?
```

Train yourself to ask

```
What can I KEEP?
```

Keeping a sorted prefix and a sorted suffix is much easier than guessing the middle to remove.

That single change in thinking is the key intuition behind this problem.

---

# 📝 Final Cheat Sheet

```text
Need Sorted Array
        │
        ▼
Find Longest Sorted Prefix
        │
        ▼
Find Longest Sorted Suffix
        │
        ▼
Already Sorted?
        │
       Yes ─────► Answer = 0
        │
       No
        │
        ▼
Initial Answer
(Removing Prefix or Suffix)
        │
        ▼
Merge Prefix & Suffix
Using Two Pointers
        │
        ▼
arr[i] <= arr[j] ?
        │
   Yes ─────► Update Answer
        │
   No
        │
Move Right Pointer
        │
        ▼
Minimum Removal Length
```

<br/><br/><br/><br/><br/>

---

# 1673. Find the Most Competitive Subsequence

> **Difficulty:** Medium  
> **Technique:** Greedy + Monotonic Stack

---

# 📚 Problem Statement

You are given

- an integer array `nums`
- an integer `k`

You need to select **exactly `k` elements** while maintaining their original order.

Among all possible subsequences of length `k`, return the **lexicographically smallest (most competitive)** one.

---

# 🤔 What is a Subsequence?

A subsequence means

- You may remove elements.
- You **cannot change the order**.

Example

```
nums

3 5 2 6
```

Valid subsequences

```
3 5

3 2

3 6

5 2

5 6

2 6
```

Invalid

```
6 2
```

because order changed.

---

# What Does "Most Competitive" Mean?

This is simply another name for

```
Lexicographically Smallest
```

Think exactly like dictionary order.

Example

```
A = [1,3,4]

B = [1,3,5]
```

Compare

First number

```
1 = 1
```

Second

```
3 = 3
```

Third

```
4 < 5
```

Therefore

```
A is smaller.
```

---

Another example

```
[2,5,7]

[3,1,1]
```

Compare first element

```
2 < 3
```

Immediately

```
First array wins.
```

We never compare the remaining elements.

---

# Example

```
nums = [3,5,2,6]

k = 2
```

Possible subsequences

```
3 5

3 2

3 6

5 2

5 6

2 6
```

Now compare

```
2 6
```

starts with

```
2
```

Everything else starts with

```
3

or

5
```

Clearly

```
2 6
```

is the smallest.

Answer

```
[2,6]
```

---

# Step 1 : Brute Force Thinking

Generate every subsequence.

Then compare all.

Example

```
Choose every combination

↓

Compare

↓

Return smallest
```

How many?

```
nCk
```

Impossible.

---

# Step 2 : Think Like an Interviewer

Question

```
Can we improve the first element?
```

Suppose

```
3 5 2 6
```

Current answer starts

```
3
```

Suddenly

```
2
```

appears.

Question

```
Should we keep 3?
```

No.

Because

```
2

<

3
```

A smaller first element always makes the subsequence better.

---

# Biggest Observation

Whenever

```
Current Number

<

Previous Number
```

we should remove the previous number.

This sounds familiar.

Exactly!

This is the same idea as

```
Remove K Digits

316 Remove Duplicate Letters

402 Remove K Digits
```

This is a

```
Monotonic Stack
```

problem.

---

# Step 3 : Can We Always Pop?

No.

Suppose

```
nums

3 5 2 6

k = 2
```

Imagine stack

```
3

5
```

Now

```
2
```

comes.

We want

```
2
```

instead of

```
5
```

Can we remove

```
5
```

?

Yes.

Still enough numbers remain.

---

Can we remove

```
3
```

?

Yes.

Still

```
2

6
```

exist.

---

But suppose

```
nums

3 2

k=2
```

Can we remove

```
3
```

?

No.

Because then

only

```
2
```

remains.

Need two elements.

Impossible.

---

# Important Condition

We can pop only if

```
After popping

there are still enough elements

to build size k.
```

This is the heart of the problem.

---

# Deriving the Formula

Suppose

```
Stack Size

=

s
```

Current index

```
i
```

Remaining elements

```
n-i
```

If we pop one element

stack becomes

```
s-1
```

Maximum elements we can finally have

```
(s-1)

+

(n-i)
```

This must be at least

```
k
```

Otherwise

we cannot complete the answer.

Therefore

```
(s-1)+(n-i)>=k
```

This is exactly the condition used in the solution.

---

# Greedy Logic

Whenever

```
Top of Stack

>

Current Number
```

AND

```
Enough elements remain
```

Pop.

Because

keeping a smaller number earlier

always gives a better subsequence.

---

# Dry Run

Example

```
nums

3 5 2 6

k=2
```

---

Start

```
Stack

[]
```

---

Current

```
3
```

Push

```
[3]
```

---

Current

```
5
```

Top

```
3
```

```
3>5
```

No.

Push.

```
3 5
```

---

Current

```
2
```

Top

```
5
```

```
5>2
```

Yes.

Enough elements remain?

Current

```
Stack Size

2
```

After pop

```
1
```

Remaining elements

```
2

(2 and 6)
```

Maximum possible

```
1+2=3

>=2
```

Safe.

Pop

```
3
```

---

Again

Top

```
3
```

```
3>2
```

Yes.

After pop

```
0

+

2

=

2
```

Still enough.

Pop.

Stack

```
[]
```

Push

```
2
```

Stack

```
[2]
```

---

Current

```
6
```

Top

```
2
```

```
2>6
```

No.

Push

```
2 6
```

Done.

Answer

```
2 6
```

---

# Another Dry Run

```
nums

2 4 3 3 5 4 9 6

k=4
```

Start

```
[]
```

Push

```
2
```

```
2
```

---

Push

```
4
```

```
2 4
```

---

Current

```
3
```

Top

```
4>3
```

Pop.

Stack

```
2
```

Push

```
3
```

Stack

```
2 3
```

---

Current

```
3
```

Top

```
3>3
```

No.

Push

```
2 3 3
```

---

Current

```
5
```

Push

```
2 3 3 5
```

---

Current

```
4
```

Top

```
5>4
```

Enough elements remain.

Pop.

Push

```
4
```

Stack

```
2 3 3 4
```

---

Current

```
9
```

Already have

```
4 elements
```

Cannot improve.

Skip extra pushes.

---

Current

```
6
```

Still cannot improve.

Final

```
2 3 3 4
```

---

# Algorithm

Step 1

Create empty stack.

---

Step 2

Traverse every number.

---

Step 3

While

```
Stack not empty

AND

Top > Current

AND

Enough elements remain
```

Pop.

---

Step 4

If stack size

```
<k
```

Push current element.

---

Step 5

Return stack.

---

# Python Solution

```python
class Solution:
    def mostCompetitive(self, nums, k):

        stack = []
        n = len(nums)

        for i, num in enumerate(nums):

            while (
                stack
                and stack[-1] > num
                and (len(stack) - 1) + (n - i) >= k
            ):
                stack.pop()

            if len(stack) < k:
                stack.append(num)

        return stack
```

---

# Why Does This Work?

Suppose

Current stack

```
3 5
```

Current number

```
2
```

Question

```
Should 5 stay?
```

No.

Replacing

```
5

with

2
```

makes every future subsequence smaller.

This is called a

```
Greedy Choice.
```

A smaller number earlier is **always** better.

---

# Why Do We Need the Condition?

Suppose

```
nums

3 2

k=2
```

Current stack

```
3
```

Current

```
2
```

Without checking remaining elements,

we pop

```
3
```

Now stack

```
[]
```

Push

```
2
```

Answer

```
2
```

Need

```
2 elements
```

Impossible.

Therefore

always verify

```
Can I still build k elements?
```

before popping.

---

# Complexity Analysis

### Time Complexity

Every element is

- pushed once
- popped at most once

Therefore

```
O(N)
```

---

### Space Complexity

Maximum stack size

```
k
```

Therefore

```
O(k)
```

---

# 🧠 Pattern Recognition

Whenever you see

```
Smallest

Subsequence

Lexicographically Smallest

Most Competitive

Remove Bigger Earlier Elements

Keep Order
```

Think immediately

```
Greedy

+

Monotonic Increasing Stack
```

---

# 🎯 Interview Thinking

When you first read this problem, don't think

> "Which subsequence should I generate?"

Instead ask

> "If I see a smaller number later, should I replace a bigger number I already picked?"

That single question naturally leads to a **greedy + monotonic stack** solution.

---

# 📝 Final Cheat Sheet

```text
Need Smallest Subsequence
        │
        ▼
Smaller Number Appears?
        │
        ▼
Can Replace Bigger One?
        │
        ▼
Enough Elements Remaining?
        │
     Yes │
        ▼
Pop Bigger Element
        │
        ▼
Push Current Element
        │
        ▼
Maintain Size ≤ k
        │
        ▼
Answer = Stack
```

<br/><br/><br/><br/><br/>

---

# 853. Car Fleet

> **Difficulty:** Medium  
> **Technique:** Sorting + Greedy + Monotonic Stack Thinking

---

# 📚 Problem Statement

There are several cars driving toward the same destination.

You are given

- Target position
- Starting position of every car
- Speed of every car

Your task is to find

> **How many separate car fleets reach the target?**

---

# 🚗 What is a Car Fleet?

A fleet can be

- One car
- Multiple cars moving together

Example

```
A --->

B --->

```

If

```
B catches A
```

Then

```
A B
```

become

```
One Fleet
```

From that point,

they travel together.

---

# Important Rule

Cars

```
CANNOT

OVERTAKE
```

Example

```
Target
↓

----------------------

10

8

```

Suppose

```
Car at 8

is faster
```

It catches

```
Car at 10
```

Can it go ahead?

No.

It must

```
Slow down

and

follow
```

Now both move together.

---

# Example 1

```
Target = 12

Position

10 8 5 3 0

Speed

2 4 1 3 1
```

Let's draw.

```
0-------------------------12

0

3

5

8

10
```

---

# Step 1 : Ignore Speed

First look only at positions.

Cars nearest to target

```
10

↓

8

↓

5

↓

3

↓

0
```

Notice something.

Cars only interact with

the car

directly ahead.

Not behind.

---

# Biggest Observation

Suppose

```
Car A

↓

10
```

Behind it

```
Car B

↓

8
```

Behind that

```
Car C

↓

5
```

Can

```
Car C

directly catch

Car A?
```

No.

It must first deal with

```
Car B.
```

Therefore

we should process

```
Nearest

↓

Farthest
```

This is why

we sort by position.

---

# Step 2 : Calculate Arrival Time

Forget fleets for a moment.

Ask

```
How long

will each car take

to reach target?
```

Formula

```
Time

=

Distance

/

Speed
```

Distance

```
Target

-

Position
```

Formula

```
(target-position)

/ speed
```

---

Example

```
Target

12

Car

Position=10

Speed=2
```

Distance

```
2
```

Time

```
2/2

=

1 hour
```

---

Next

```
Position=8

Speed=4
```

Distance

```
4
```

Time

```
4/4

=

1 hour
```

Interesting.

Both reach

```
exactly together.
```

So

they become

one fleet.

---

# Example Times

Sorted

|Position|Speed|Time|
|--------|-----|----|
|10|2|1|
|8|4|1|
|5|1|7|
|3|3|3|
|0|1|12|

---

# Step 3 : The Greedy Observation

Process

```
Nearest

↓

Farthest
```

First

```
Position

10

Time

1
```

Nothing ahead.

New Fleet.

```
Fleet Time

1
```

Fleet Count

```
1
```

---

Next

```
Position

8

Time

1
```

Question

```
Can this car

catch

the fleet ahead?
```

Fleet ahead

needs

```
1 hour
```

Current car

also

needs

```
1 hour
```

They meet

at target.

They become

one fleet.

Fleet count

still

```
1
```

---

Next

```
Position

5

Time

7
```

Fleet ahead

needs

```
1 hour
```

Can

7-hour car

catch

1-hour fleet?

No.

Fleet already reaches

target

before this car.

Therefore

New Fleet.

Fleet count

```
2
```

Current Fleet Time

```
7
```

---

Next

```
Position

3

Time

3
```

Fleet ahead

takes

```
7
```

Current

takes

```
3
```

Current is faster.

It reaches

before

7.

So

it catches

the fleet.

Become

one fleet.

Fleet count

still

```
2
```

Fleet Time

remains

```
7
```

Because

slowest car

controls fleet.

---

Next

```
Position

0

Time

12
```

Fleet ahead

needs

```
7
```

Current

needs

```
12
```

Cannot catch.

New Fleet.

Fleet Count

```
3
```

Answer

```
3
```

---

# Why Compare Times?

Suppose

Ahead Car

```
needs

5 hours
```

Behind Car

```
needs

3 hours
```

Who reaches earlier?

Behind.

Therefore

it must catch

the car ahead.

Fleet.

---

Suppose

Ahead

```
3 hours
```

Behind

```
6 hours
```

Ahead reaches first.

Impossible to catch.

Different fleets.

---

# Biggest Intuition

Forget cars.

Think

```
Arrival Time
```

Cars don't matter.

Only

```
Arrival Times
```

matter.

---

# Why Process From Right?

Suppose

```
10

↓

8

↓

5
```

Can

```
10

be affected

by

5?
```

No.

Only

cars

behind

can catch

cars ahead.

Therefore

always process

```
Right

↓

Left
```

Nearest

↓

Farthest.

---

# Monotonic Stack Thinking

Notice the times

```
1

1

7

3

12
```

Process

```
1

↓

1

↓

7

↓

3

↓

12
```

Whenever

Current Time

```
<=

Fleet Time
```

Merge.

Otherwise

New Fleet.

Notice

Fleet Times become

```
1

7

12
```

Always

```
Increasing
```

This is exactly

Monotonic Stack Thinking.

You don't actually need a stack.

Only the last fleet time.

---

# Dry Run

```
Target

12
```

Cars

```
Position

10 8 5 3 0

Time

1 1 7 3 12
```

---

Start

```
Fleet Count

0
```

Last Fleet Time

```
0
```

---

Car

```
10

Time

1
```

```
1>0
```

New Fleet.

Fleet Count

```
1
```

Fleet Time

```
1
```

---

Car

```
8

Time

1
```

```
1<=1
```

Merge.

Fleet Count

still

```
1
```

---

Car

```
5

Time

7
```

```
7>1
```

New Fleet.

Fleet Count

```
2
```

Fleet Time

```
7
```

---

Car

```
3

Time

3
```

```
3<=7
```

Merge.

Fleet Count

```
2
```

---

Car

```
0

Time

12
```

```
12>7
```

New Fleet.

Fleet Count

```
3
```

Finished.

---

# Algorithm

Step 1

Pair

```
Position

+

Speed
```

---

Step 2

Sort

by position.

---

Step 3

Traverse

from

largest position

to

smallest.

---

Step 4

Compute

```
Time

=

(target-position)

/speed
```

---

Step 5

If

```
Current Time

>

Last Fleet Time
```

Create

new fleet.

Otherwise

merge.

---

# Python Solution

```python
class Solution:
    def carFleet(self, target, position, speed):

        cars = sorted(zip(position, speed))

        fleets = 0

        last_time = 0

        for pos, spd in reversed(cars):

            time = (target - pos) / spd

            if time > last_time:

                fleets += 1
                last_time = time

        return fleets
```

---

# Complexity Analysis

### Sorting

```
O(N log N)
```

Traversal

```
O(N)
```

Overall

```
O(N log N)
```

---

### Space Complexity

Sorting list

```
O(N)
```

---

# 🧠 How to Think in Interviews

When you first read the problem, your brain may focus on:

- positions
- speeds
- collisions
- simulation

That quickly becomes complicated.

Instead, simplify the problem by asking:

> "What determines whether two cars become one fleet?"

The answer is **their arrival times**.

Once you compute each car's arrival time:

- If a car behind arrives **earlier or at the same time** as the fleet ahead, it must catch up and join that fleet.
- If it arrives **later**, it can never catch the fleet ahead, so it forms a new fleet.

This transforms a difficult simulation problem into a simple greedy traversal.

---

# 🎯 Pattern Recognition

Whenever you see

```text
Objects moving in one direction

↓

Cannot overtake

↓

Can merge

↓

Need number of groups
```

Think

```text
Sort by position

↓

Compute arrival time

↓

Process from nearest to farthest

↓

Greedy comparison

↓

Monotonic Stack Thinking
```

---

# 📝 Final Cheat Sheet

```text
Cars
        │
        ▼
Sort by Position
        │
        ▼
Compute Arrival Time
        │
        ▼
Process Right → Left
        │
        ▼
Current Time > Last Fleet Time ?
        │
   Yes ─────────► New Fleet
        │
   No
        │
        ▼
Merge into Existing Fleet
        │
        ▼
Count Fleets
```

<br/><br/><br/><br/><br>

---

# 962. Maximum Width Ramp

> **Difficulty:** Medium  
> **Technique:** Monotonic Stack + Greedy + Reverse Traversal

---

# 📚 Problem Statement

You are given an array.

A **ramp** is a pair of indices

```
(i, j)
```

such that

```
i < j

AND

nums[i] <= nums[j]
```

Its width is

```
j - i
```

Return the **maximum width** among all valid ramps.

---

# 🤔 Understanding the Problem

Suppose

```
nums = [6,0,8,2,1,5]
```

Index

```
0 1 2 3 4 5
```

Values

```
6 0 8 2 1 5
```

Can we choose

```
(0,2)
```

?

Check

```
6 <= 8
```

Yes.

Width

```
2
```

---

Can we choose

```
(1,5)
```

?

```
0 <= 5
```

Yes.

Width

```
5-1=4
```

---

Answer

```
4
```

---

# Step 1 : Brute Force Thinking

Every beginner starts here.

For every

```
i
```

check every

```
j
```

```
for i

    for j

        if nums[i]<=nums[j]:

            answer=max(answer,j-i)
```

---

## Complexity

```
O(N²)
```

For

```
N = 50000
```

Impossible.

---

# Step 2 : What Are We Maximizing?

The question asks

```
Maximum Width
```

Meaning

```
j-i
```

To maximize width,

what do we want?

```
Small i

Large j
```

Immediately write this on paper.

```
Need

↓

Smallest possible left index

Largest possible right index
```

---

# Step 3 : What Makes a Valid Ramp?

Need

```
nums[i]

<=

nums[j]
```

Question

Which left indices are useful?

Example

```
6 0 8 2 1 5
```

Look at

```
6
```

Then

```
0
```

Question

If

```
0
```

appears after

```
6
```

will we ever choose

```
6
```

instead?

No.

Because

```
0

is smaller

AND

comes later
```

Actually let's compare.

Candidate 1

```
i=0

value=6
```

Candidate 2

```
i=1

value=0
```

Any future number

that works for

```
6
```

will also work for

```
0
```

And

```
0
```

is even smaller.

Therefore

```
6

is useless.
```

---

# Biggest Observation

The only useful left indices are

those having

```
New Minimum
```

Example

```
6 0 8 2 1 5
```

Minimums appear at

```
6

↓

0
```

Stack becomes

```
6

0
```

Indices

```
0

1
```

Everything else

```
8

2

1

5
```

cannot become

better left endpoints.

---

# Why Monotonic Stack?

We want

```
Decreasing Values
```

Example

```
6

0
```

Each new value

is smaller than previous.

Store

indices.

Stack

```
0

1
```

Values

```
6

0
```

This is a

```
Monotonic Decreasing Stack
```

---

# Step 4 : Why Traverse From Right?

We need

```
Maximum Width
```

Largest

```
j
```

comes first

if we start

from right.

Suppose

```
j=5
```

If

```
nums[5]

>=

nums[i]
```

Then

```
5-i
```

is already

the largest possible width

for that

```
i.
```

No need

to check smaller

```
j.
```

Amazing.

---

# Complete Algorithm

Step 1

Build stack

of

```
indices

having decreasing values
```

---

Example

```
6 0 8 2 1 5
```

Stack

```
0

1
```

---

Step 2

Traverse

from right.

```
j=5

↓

4

↓

3

↓

2

↓

1

↓

0
```

Whenever

```
nums[j]

>=

nums[stack top]
```

Found a ramp.

Update answer.

Pop.

Why pop?

Because

this

```
j
```

already gives

maximum width

for that index.

No future

smaller

```
j
```

can improve it.

---

# Dry Run

Example

```
nums

6 0 8 2 1 5
```

Index

```
0 1 2 3 4 5
```

---

## Step 1

Build stack.

Start

```
[]
```

---

Index

```
0

value=6
```

Push

```
[0]
```

---

Index

```
1

value=0
```

```
0<6
```

Push

```
[0,1]
```

---

Index

```
2

value=8
```

```
8<0

No
```

Skip.

---

Index

```
3

value=2
```

```
2<0

No
```

Skip.

---

Index

```
4

value=1
```

```
1<0

No
```

Skip.

---

Index

```
5

value=5
```

Skip.

Final Stack

```
Indices

0

1
```

Values

```
6

0
```

---

# Step 2

Traverse

from right.

---

### j=5

Value

```
5
```

Top

```
0
```

Check

```
5>=0
```

Yes.

Width

```
5-1

=

4
```

Answer

```
4
```

Pop.

Stack

```
0
```

---

Again

Top

```
6
```

Check

```
5>=6
```

No.

---

### j=4

Value

```
1
```

Top

```
6
```

```
1>=6

No
```

---

### j=3

Value

```
2
```

```
2>=6

No
```

---

### j=2

Value

```
8
```

Top

```
6
```

```
8>=6
```

Yes.

Width

```
2-0

=

2
```

Answer

still

```
4
```

Pop.

Stack empty.

Done.

Final Answer

```
4
```

---

# Why Can We Pop?

Suppose

```
Current Right Index

=

8
```

Matched

```
Left Index

=

2
```

Width

```
6
```

Will

```
Right Index

=

7
```

ever give

larger width?

No.

```
7-2

<

8-2
```

Impossible.

Therefore

remove it forever.

---

# Python Solution

```python
class Solution:
    def maxWidthRamp(self, nums):

        stack = []

        # Build decreasing stack
        for i, num in enumerate(nums):

            if not stack or nums[stack[-1]] > num:
                stack.append(i)

        ans = 0

        # Traverse from right
        for j in range(len(nums) - 1, -1, -1):

            while stack and nums[j] >= nums[stack[-1]]:

                ans = max(ans, j - stack[-1])

                stack.pop()

        return ans
```

---

# Complexity Analysis

Building stack

```
O(N)
```

Reverse traversal

```
O(N)
```

Every index

is pushed once

and popped once.

Overall

```
O(N)
```

Space

```
O(N)
```

---

# 🧠 How to Think Like an Interviewer

Most beginners think:

> "For every left index, let me find the farthest right index."

That leads to an `O(N²)` solution.

Instead, ask yourself:

### Question 1

```
What am I maximizing?
```

Answer

```
Width

=

j-i
```

So we want

```
Small i

Large j
```

---

### Question 2

```
Which left indices are actually useful?
```

If a smaller value appears later,

the earlier larger value is never a better starting point.

Keep only

```
New Minimums
```

---

### Question 3

```
Which direction should I traverse?
```

Need

```
Largest j
```

So start

```
Right

↓

Left
```

---

### Question 4

```
Can I permanently discard some candidates?
```

Yes.

Once a left index finds the farthest possible right index,

it can never get a better answer.

Pop it.

---

# 🎯 Pattern Recognition

Whenever you see

```
Maximum Width

OR

Maximum Distance

OR

Farthest Pair

with a condition
```

Think

```
Can I keep only the best left candidates?

↓

Monotonic Stack

↓

Process from opposite direction

↓

Greedy Matching
```

---

# 📝 Final Cheat Sheet

```text
Need Maximum Width
        │
        ▼
Need Small Left Index
        │
        ▼
Keep Only New Minimums
(Monotonic Decreasing Stack)
        │
        ▼
Traverse From Right
        │
        ▼
nums[j] >= nums[stack.top] ?
        │
   Yes ─────► Update Width
        │
             Pop Left Index
        │
   No
        │
Continue
        │
        ▼
Maximum Width Ramp
```

<br/><br/><br/><br/><br>

---

# 581. Shortest Unsorted Continuous Subarray

> **Difficulty:** Medium  
> **Technique:** Monotonic Stack (Increasing + Decreasing) / Array Boundary Detection

---

# 📚 Problem Statement

You are given an integer array.

Find the **smallest continuous subarray** such that:

- If you sort only that subarray,
- the **entire array becomes sorted**.

Return the length of that subarray.

---

# Example

```
nums = [2,6,4,8,10,9,15]
```

Current array

```
2 6 4 8 10 9 15
```

Only this part is wrong

```
6 4 8 10 9
```

If we sort it

```
4 6 8 9 10
```

Whole array becomes

```
2 4 6 8 9 10 15
```

Answer

```
5
```

---

# 🤔 Understanding the Problem

Notice something.

The array is

```
Sorted

↓

Wrong

↓

Sorted
```

Our job is simply

```
Find where disorder starts

Find where disorder ends
```

---

# Step 1 : Brute Force

Try every subarray.

Sort it.

Check if whole array becomes sorted.

```
for every L

    for every R

        sort

        check
```

Complexity

```
O(N³ logN)
```

Impossible.

---

# Step 2 : What Makes an Array Sorted?

A sorted array satisfies

```
nums[i]

<=

nums[i+1]
```

everywhere.

So ask

```
Where does this rule break?
```

Example

```
2 6 4 8 10 9 15
```

At

```
6 > 4
```

and

```
10 > 9
```

Interesting...

Those are exactly the places where the order is violated.

---

# Step 3 : Finding the Left Boundary

Suppose

```
2 6 4
```

Question

```
Can 6 stay before 4?
```

No.

So

```
6
```

must belong to the unsorted part.

But maybe something even earlier?

If more elements violate the order,

the boundary moves left.

So we need the **leftmost index** involved in any inversion.

---

# Why Monotonic Increasing Stack?

We want indices whose values stay increasing.

Example

```
2 6
```

Stack

```
[0,1]
```

Now

```
4
```

comes.

```
6 > 4
```

Order breaks.

Pop

```
6
```

Now

```
start = min(start, popped_index)
```

Continue until increasing order is restored.

---

# Step 4 : Finding the Right Boundary

Now think from the opposite direction.

Traverse

```
Right

↓

Left
```

Maintain a **decreasing stack**.

Whenever

```
nums[i]

>

stack top
```

the order is broken.

That popped index belongs to the unsorted region.

Update

```
end = max(end, popped_index)
```

---

# Why Two Passes?

The first pass only tells us

```
How far left

the disorder reaches.
```

The second pass tells us

```
How far right

the disorder reaches.
```

Together they define the smallest subarray.

---

# Dry Run

Example

```
nums

2 6 4 8 10 9 15
```

Indices

```
0 1 2 3 4 5 6
```

---

## First Pass (Increasing Stack)

Start

```
stack = []
start = n
```

---

### i = 0

```
2
```

Push

```
[0]
```

---

### i = 1

```
6
```

Increasing.

Push

```
[0,1]
```

---

### i = 2

```
4
```

Top

```
6
```

```
6 > 4
```

Pop index

```
1
```

Update

```
start = 1
```

Push

```
2
```

Stack

```
0 2
```

---

### i = 3

```
8
```

Push

---

### i = 4

```
10
```

Push

---

### i = 5

```
9
```

Top

```
10
```

Pop

```
4
```

start

still

```
1
```

Push

```
5
```

---

### i = 6

Push.

Done.

Left boundary

```
1
```

---

# Second Pass

Traverse

```
Right

↓

Left
```

---

### i = 6

Push

---

### i = 5

Push

---

### i = 4

```
10
```

Top

```
9
```

```
10 > 9
```

Pop

```
5
```

Update

```
end = 5
```

Push

```
4
```

---

Continue.

Nothing extends farther.

Right boundary

```
5
```

---

Length

```
5-1+1

=

5
```

Answer

```
5
```

---

# Why Does This Work?

Suppose

```
2 6 4
```

The moment we see

```
6 > 4
```

we know

```
6

cannot stay there.
```

Therefore

its index must belong

to the unsorted subarray.

The stack helps us find **every such index** in one pass.

---

# Python Solution

```python
class Solution:
    def findUnsortedSubarray(self, nums):

        n = len(nums)

        start = n
        end = 0

        stack = []

        # Find left boundary
        for i in range(n):

            while stack and nums[stack[-1]] > nums[i]:

                start = min(start, stack.pop())

            stack.append(i)

        if start == n:
            return 0

        stack = []

        # Find right boundary
        for i in range(n - 1, -1, -1):

            while stack and nums[stack[-1]] < nums[i]:

                end = max(end, stack.pop())

            stack.append(i)

        return end - start + 1
```

---

# Complexity Analysis

### Time

Every index is

- pushed once
- popped once

```
O(N)
```

---

### Space

Stack

```
O(N)
```

---

# 🧠 How to Think Like an Interviewer

Most people see this problem and think:

> "Which subarray should I sort?"

That leads to checking many subarrays.

Instead, ask:

### Question 1

```
What property does a sorted array satisfy?
```

Answer

```
nums[i] <= nums[i+1]
```

---

### Question 2

```
Where is this property violated?
```

Those violations must be inside the answer.

---

### Question 3

```
How do I efficiently find the earliest and latest violations?
```

A monotonic stack naturally keeps the array in sorted order.

Whenever that order breaks, we've found part of the unsorted region.

---

# 🎯 Pattern Recognition

Whenever you see

```
Find the smallest range

OR

Find disorder

OR

Find first/last violation
```

Think

```
Monotonic Stack

↓

Detect where sorted order breaks

↓

Left boundary + Right boundary

↓

Answer
```

---

# 📌 Connection to Other Problems

This problem belongs to the same family as:

- **739. Daily Temperatures** → Next greater element
- **84. Largest Rectangle in Histogram** → Previous/next smaller
- **907. Sum of Subarray Minimums** → Previous/next smaller
- **962. Maximum Width Ramp** → Monotonic stack + greedy
- **1673. Most Competitive Subsequence** → Greedy + monotonic stack
- **402. Remove K Digits** → Greedy + monotonic stack

The common theme is:

> **Maintain a monotonic order. When that order breaks, use the popped indices to answer the question.**

Once you start recognizing **"order breaks"**, you'll naturally think about monotonic stacks.

<br/><br/><br/><br/><br>

---

# 456. 132 Pattern

> **Difficulty:** Medium  
> **Technique:** Monotonic Stack + Reverse Traversal + Greedy

---

# 📚 Problem Statement

You are given an array.

Determine whether there exist **three indices**

```
i < j < k
```

such that

```
nums[i] < nums[k] < nums[j]
```

This is called a **132 Pattern**.

If such a pattern exists,

return

```
True
```

otherwise

```
False
```

---

# 🤔 First Understand the Pattern

The name

```
132
```

does **NOT** mean

```
1

3

2
```

It means

```
Small

Large

Middle
```

because

```
1 < 2 < 3
```

but their positions are

```
1

3

2
```

So we need

```
nums[i]

<

nums[k]

<

nums[j]
```

and

```
i < j < k
```

---

Example

```
[3,1,4,2]
```

Indices

```
0 1 2 3
```

Values

```
3 1 4 2
```

Choose

```
1

4

2
```

Check

```
1<2<4
```

Yes.

Indices

```
1<2<3
```

Also yes.

Answer

```
True
```

---

# Step 1 : Brute Force Thinking

Try every triple.

```
for i

    for j

        for k

            check
```

Complexity

```
O(N³)
```

Impossible.

---

# Step 2 : Reduce the Problem

Whenever I solve a problem,

I ask

```
Which value is hardest to find?
```

We need

```
Small

Large

Middle
```

Suppose

we already know

```
Large

and

Middle
```

Then

finding

```
Small
```

is easy.

Or

suppose

we already know

```
Small

and

Large
```

Finding

```
Middle
```

is difficult.

So maybe

we should process

```
Large

↓

Middle
```

first.

---

# Step 3 : Which Direction?

Look carefully.

```
i < j < k
```

Notice

```
j

and

k
```

are on the right side.

If we traverse

```
Right

↓

Left
```

then

when we stand at

```
i
```

everything to the right

is already processed.

This feels useful.

---

# Step 4 : Rearranging the Pattern

Instead of

thinking

```
Find

1

3

2
```

Let's ask

```
Can current number become

1 ?
```

Suppose

current number

is

```
1
```

Then we only need

```
Some

2

and

3

already seen
```

Great!

Reverse traversal makes this possible.

---

# Step 5 : What Should We Store?

Suppose

we process

```
3

1

4

2
```

from right.

Order becomes

```
2

4

1

3
```

Question

When we see

```
4
```

what should we remember?

Answer

```
Possible

Middle Values (2)
```

Because

future numbers

may become

```
1.
```

---

# Biggest Observation

We need

```
Largest Value

↓

Middle Value
```

Notice

the "middle"

must always be

```
Less than

some larger number.
```

A stack naturally helps us maintain

possible

```
3's
```

while remembering

the best

```
2.
```

---

# The Secret Variable

We maintain

```
second
```

It represents

```
Largest possible

2
```

that we've seen so far.

Initially

```
second = -∞
```

---

# Why Reverse Traversal?

Suppose

Current

```
1
```

If

```
Current

<

second
```

Then

we already know

```
Current

↓

second

↓

Some Larger Number
```

Which means

```
1

3

2
```

exists.

Done.

---

# Building the Stack

We maintain

a

```
Decreasing Stack
```

containing

possible

```
3's.
```

Whenever

```
Current

>

Stack Top
```

those smaller elements

become candidates for

```
2.
```

So we pop them

and update

```
second.
```

---

# Why?

Example

```
Stack

8

6

4
```

Current

```
7
```

Since

```
7>4
```

The

```
4
```

can become

our

```
2.
```

Pop.

Update

```
second=4
```

Continue.

---

# Dry Run

Example

```
nums

3 1 4 2
```

Reverse

```
2

4

1

3
```

---

Start

```
stack=[]

second=-∞
```

---

### Current = 2

```
2<second?

No
```

Push

```
2
```

Stack

```
2
```

---

### Current = 4

```
4<second?

No
```

Top

```
2
```

```
4>2
```

Pop

```
2
```

Update

```
second=2
```

Push

```
4
```

Stack

```
4
```

---

### Current = 1

Check

```
1<second

1<2

Yes
```

Pattern found.

Return

```
True
```

Notice

```
1

↓

4

↓

2
```

Exactly

```
132
```

---

# Another Dry Run

```
nums

1 2 3 4
```

Reverse

```
4

3

2

1
```

Stack

```
4

↓

4 3

↓

4 3 2

↓

4 3 2 1
```

Nothing pops.

```
second
```

never changes.

Never satisfy

```
current<second
```

Return

```
False
```

---

# Why Does This Work?

Suppose

```
second=5
```

This means

we already found

```
Some larger number

>

5
```

Now

if current

is

```
3
```

Then

```
3<5
```

Automatically

```
3

↓

Larger Number

↓

5
```

forms

```
132
```

No further searching needed.

---

# Python Solution

```python
class Solution:
    def find132pattern(self, nums):

        stack = []

        second = float("-inf")

        for num in reversed(nums):

            if num < second:
                return True

            while stack and num > stack[-1]:
                second = stack.pop()

            stack.append(num)

        return False
```

---

# Complexity Analysis

Each element

is

- pushed once
- popped once

Time

```
O(N)
```

Space

```
O(N)
```

---

# 🧠 How to Think Like an Interviewer

This is the part most people skip.

Let's learn **how to discover** this solution instead of memorizing it.

---

## ❌ Beginner Thinking

Most beginners immediately think:

```
Need three numbers

↓

Let's try every triple
```

or

```
Need i

Find j

Find k
```

This becomes very complicated.

---

## ✅ Interview Thinking

### Question 1

```
What am I searching for?
```

Answer

```
Three numbers

with inequalities
```

Whenever there are **3 or more numbers with relationships**, ask:

> **Can I fix one of them and search for the others?**

---

### Question 2

Which one should I fix?

We need

```
1

3

2
```

Finding all three together is hard.

Instead ask:

```
If I already knew

2

could I recognize

1?
```

Yes.

Just check

```
current < second
```

Much easier.

---

### Question 3

Can I process the array in a direction that makes this possible?

Since

```
i < j < k
```

everything we need for

```
i
```

is to its right.

So process

```
Right

↓

Left
```

---

### Question 4

What information should I remember?

Not every element.

Only the useful ones.

Useful "3" values are maintained using a

```
Monotonic Decreasing Stack.
```

Useful "2" value is stored in

```
second.
```

---

# 🎯 Pattern Recognition

Whenever you see

```
Three numbers

↓

Inequalities

↓

Order matters
```

Ask yourself

```
Can I fix one number?

↓

Can I process backwards?

↓

Can I store candidates in a stack?

↓

Can I reduce three-variable search
to one-variable checking?
```

This is the mental trick behind many hard interview problems.

---

# 🔗 Similar Problems

This thinking appears in

- **739. Daily Temperatures**
- **496. Next Greater Element**
- **84. Largest Rectangle in Histogram**
- **962. Maximum Width Ramp**
- **402. Remove K Digits**
- **1673. Most Competitive Subsequence**

The common idea is:

> **Use a monotonic stack to keep only useful candidates and discard everything else.**

---

# 📝 Final Cheat Sheet

```text
Need Pattern

1 < 2 < 3

But Order

1 → 3 → 2

        │
        ▼
Process From Right
        │
        ▼
Maintain Decreasing Stack
(Possible 3's)
        │
        ▼
Pop Smaller Values
        │
        ▼
Largest Popped = second (2)
        │
        ▼
Current < second ?
        │
   Yes ─────► Found 132 Pattern
        │
   No
        │
        ▼
Push Current
```

<br/><br/><br/><br/><br>

---

# 907. Sum of Subarray Minimums

> **Difficulty:** Medium  
> **Technique:** Monotonic Stack + Contribution Technique

---

# 📚 Problem Statement

You are given an array.

For **every possible contiguous subarray**, find its **minimum element**.

Finally,

```
Add all those minimums together.
```

Return the answer.

---

# Example

```
arr = [3,1,2,4]
```

All subarrays

```
[3]        min = 3

[1]        min = 1

[2]        min = 2

[4]        min = 4

[3,1]      min = 1

[1,2]      min = 1

[2,4]      min = 2

[3,1,2]    min = 1

[1,2,4]    min = 1

[3,1,2,4]  min = 1
```

Sum

```
3+1+2+4+1+1+2+1+1+1 = 17
```

---

# 🤔 Step 1 : How Does a Common Person Think?

When we first read the problem,

we naturally think

```
Generate every subarray

↓

Find minimum

↓

Add it
```

---

# Brute Force

```
for every starting index

    for every ending index

         find minimum
```

---

Example

```
3 1 2 4
```

Generate

```
3

3 1

3 1 2

3 1 2 4

1

1 2

1 2 4

2

2 4

4
```

Complexity

Generating

```
O(N²)
```

Finding minimum

```
O(N)
```

Total

```
O(N³)
```

Impossible.

---

# 🤔 Step 2 : Improve the Brute Force

Instead of finding the minimum again,

keep updating it.

```
mini = INF

for j:

    mini=min(mini,arr[j])
```

Now

Time

```
O(N²)
```

Still too slow.

---

# 🤔 Step 3 : Think Differently

This is where interview thinking starts.

Instead of asking

```
For every subarray

find minimum
```

Ask

```
For every element

How many subarrays

consider THIS element

their minimum?
```

This is the biggest idea.

---

# Example

```
3 1 2 4
```

Instead of

```
Subarray

↓

Minimum
```

Think

```
Element

↓

How many subarrays choose me?
```

---

Suppose

```
1
```

is the minimum in

```
[1]

[3,1]

[1,2]

[1,2,4]

[3,1,2]

[3,1,2,4]
```

Instead of adding

```
1

1

1

1

1

1
```

Just compute

```
Contribution

=

1 × 6
```

Huge simplification.

---

# The New Problem

For every element

find

```
How many subarrays

have me as minimum?
```

---

# Step 4 : Let's Focus on One Element

Example

```
3 1 2 4
```

Focus only on

```
2
```

Question

How many subarrays

have

```
2
```

as the minimum?

---

Let's draw.

```
3 1 2 4
    ↑
```

To the left

when do we stop?

At the first element

smaller than

```
2
```

which is

```
1
```

---

To the right

when do we stop?

At the first element

smaller than

```
2
```

There isn't one.

---

Interesting.

This looks exactly like

```
Previous Smaller Element

Next Smaller Element
```

Monotonic Stack!

---

# Step 5 : Why Previous Smaller?

Suppose

```
3 1 2 4
```

Current

```
2
```

Can we include

```
1
```

inside the subarray?

No.

Because

```
1
```

would become the minimum.

So

```
1
```

is the boundary.

---

Similarly

on the right

we stop

when a smaller element appears.

---

# Step 6 : Count Choices

Suppose

```
Previous Smaller

↓

distance = L
```

```
Next Smaller

↓

distance = R
```

Then

Left side has

```
L

choices
```

Right side has

```
R

choices
```

Total subarrays

```
L × R
```

---

# Why Multiply?

Suppose

Current

```
2
```

Left choices

```
2
```

Right choices

```
3
```

Possible left boundaries

```
A

B
```

Possible right boundaries

```
X

Y

Z
```

Every left

can combine

with every right.

```
A-X

A-Y

A-Z

B-X

B-Y

B-Z
```

Total

```
2×3=6
```

This is simple combinatorics.

---

# Step 7 : Dry Run

Example

```
3 1 2 4
```

---

## Element

```
3
```

Previous Smaller

```
None
```

Distance

```
1
```

Next Smaller

```
1
```

Distance

```
1
```

Contribution

```
3×1×1=3
```

---

## Element

```
1
```

Previous Smaller

```
None
```

Distance

```
2
```

Choices

```
Start

0

1
```

Right

No smaller.

Distance

```
3
```

Contribution

```
1×2×3=6
```

---

## Element

```
2
```

Previous Smaller

```
1
```

Distance

```
1
```

Right

None

Distance

```
2
```

Contribution

```
2×1×2=4
```

---

## Element

```
4
```

Previous Smaller

```
2
```

Distance

```
1
```

Right

None

Distance

```
1
```

Contribution

```
4×1×1=4
```

---

Total

```
3

+

6

+

4

+

4

=

17
```

Correct.

---

# Step 8 : Finding Previous Smaller

We use

```
Increasing Stack
```

Whenever

```
stack top

>= current
```

Pop.

Then

Top

becomes

Previous Smaller.

---

Example

```
3 1 2 4
```

---

Current

```
3
```

Stack

```
[]
```

Previous

```
-1
```

Push

```
3
```

---

Current

```
1
```

Pop

```
3
```

Stack

empty

Previous

```
-1
```

Push

```
1
```

---

Current

```
2
```

Top

```
1
```

Previous Smaller

```
1
```

Push.

---

# Step 9 : Finding Next Smaller

Traverse

```
Right

↓

Left
```

Again

use

Increasing Stack.

---

# Handling Duplicates

This is extremely important.

If duplicates exist,

one side should use

```
>
```

the other side should use

```
>=
```

Otherwise

same subarray

gets counted twice.

Standard choice

Previous Smaller

```
>
```

Next Smaller

```
>=
```

---

# Algorithm

```
Find Previous Smaller

Find Next Smaller

For every element

Contribution

=

Value

×

Left Distance

×

Right Distance

Add all contributions
```

---

# Python Code

```python
class Solution:
    def sumSubarrayMins(self, arr):
        MOD = 10**9 + 7
        n = len(arr)

        prev = [-1] * n
        nxt = [n] * n

        stack = []

        # Previous Smaller
        for i in range(n):
            while stack and arr[stack[-1]] > arr[i]:
                stack.pop()

            if stack:
                prev[i] = stack[-1]

            stack.append(i)

        stack = []

        # Next Smaller or Equal
        for i in range(n - 1, -1, -1):
            while stack and arr[stack[-1]] >= arr[i]:
                stack.pop()

            if stack:
                nxt[i] = stack[-1]

            stack.append(i)

        ans = 0

        for i in range(n):
            left = i - prev[i]
            right = nxt[i] - i

            ans += arr[i] * left * right
            ans %= MOD

        return ans
```

---

# Complexity Analysis

### Time

```
O(N)
```

Each element

Push once

Pop once.

---

### Space

```
O(N)
```

For stack

and

boundary arrays.

---

# 🧠 How to Think Like an Interviewer

This problem teaches one of the most important interview transformations.

---

## Beginner Thinking

```
Need minimum

↓

Generate every subarray
```

---

## Intermediate Thinking

```
Can I reuse minimum?
```

Still

```
O(N²)
```

---

## Advanced Thinking

```
Instead of

Subarray

↓

Minimum

Reverse it

↓

Element

↓

How many subarrays choose me?
```

This idea is called the **Contribution Technique**.

---

# Pattern Recognition

Whenever you see

```
Sum of

Maximums

Minimums

Contribution

Every Subarray
```

Don't think

```
Generate every subarray
```

Instead ask

```
How much does each element contribute?
```

Then ask

```
How far can this element extend

left

and

right

before losing its property?
```

This naturally leads to

```
Previous Smaller

Next Smaller

or

Previous Greater

Next Greater
```

using a **monotonic stack**.

---

# Similar Problems

Once you understand this thinking, these problems become much easier:

- **907. Sum of Subarray Minimums**
- **2104. Sum of Subarray Ranges**
- **84. Largest Rectangle in Histogram**
- **85. Maximal Rectangle**
- **739. Daily Temperatures**
- **496. Next Greater Element**
- **503. Next Greater Element II**

All of them ask:

> **"How far can this element extend before it is blocked?"**

That's the mental trigger for a monotonic stack.

---

# Interview Tips

Whenever you see

```
Every Subarray

+

Minimum

Maximum

Contribution
```

Follow this checklist:

```
Need answer for every subarray?

↓

Can I reverse the thinking?

↓

Compute contribution of each element.

↓

How many subarrays choose me?

↓

Need Left Boundary

Need Right Boundary

↓

Previous Smaller / Next Smaller

↓

Monotonic Stack
```

---

# Key Takeaways

- Don't generate every subarray.
- Reverse the problem: let each element count its own contribution.
- Count how many subarrays an element is the minimum for.
- Number of such subarrays = **Left Choices × Right Choices**.
- Use a monotonic increasing stack to find the nearest smaller elements efficiently.
- This "contribution" mindset is one of the most powerful techniques in array interview problems.

<br/><br/><br/><br/><br>

---

# 1130. Minimum Cost Tree From Leaf Values

> **Difficulty:** Medium (Looks like DP, but has an elegant Greedy + Monotonic Stack solution)
>
> **Technique:** Greedy + Monotonic Decreasing Stack

---

# 📚 Problem Statement

You are given an array.

```
arr = [6,2,4]
```

Every number represents a **leaf node**.

You need to build **one binary tree**.

Rules:

1. Every internal node has exactly **2 children**.
2. The **leaf nodes must appear in the same inorder order** as the array.
3. Every internal node value is

```
Maximum leaf in Left Subtree

×

Maximum leaf in Right Subtree
```

Finally,

```
Add every internal node value.
```

Return the **minimum possible sum.**

---

# 🤔 First Understand the Tree

Suppose

```
arr

6 2 4
```

Leaves are fixed.

```
      ?
     / \
    ?   4
   / \
  6   2
```

Internal node

```
6×2=12
```

Root

```
max(6,2)=6

max(4)=4

↓

6×4=24
```

Total

```
12+24=36
```

---

Another tree

```
      ?
     / \
    6   ?
       / \
      2   4
```

Internal

```
2×4=8
```

Root

```
6×4=24
```

Total

```
24+8=32
```

Smaller.

Answer

```
32
```

---

# Step 1 : Think Like a Common Person

When beginners see this,

they think

```
Need every tree

↓

Calculate cost

↓

Take minimum
```

---

How many trees?

For

```
3 leaves

↓

2 trees
```

For

```
4 leaves

↓

5 trees
```

For

```
10 leaves

↓

16796 trees
```

These are **Catalan Numbers**.

Impossible.

---

# Step 2 : Forget Trees

The biggest trick is

**stop thinking about trees.**

Instead ask

```
What actually contributes
to the answer?
```

Every internal node contributes

```
Largest Left

×

Largest Right
```

Notice

the **small numbers disappear**.

Only

```
Maximums
```

remain.

Interesting.

---

# Step 3 : Observe the Smallest Number

Example

```
6 2 4
```

Look only at

```
2
```

Question

Can

```
2
```

ever become

the maximum

of a subtree?

No.

Because

```
6

>

2
```

and

```
4

>

2
```

So eventually

```
2
```

must be multiplied

with someone bigger.

It cannot escape.

---

# The Important Observation

Every element except the largest

will be multiplied

**exactly once.**

Think carefully.

```
Largest element

never disappears.
```

Every smaller element

gets merged into another subtree.

So

every element except the maximum

pays exactly one multiplication cost.

---

# Step 4 : The Big Question

Suppose

```
2
```

must be multiplied.

Should we multiply

with

```
6
```

or

```
4
```

Which gives smaller cost?

```
2×6=12
```

```
2×4=8
```

Obviously

```
8
```

is better.

So

```
Always multiply
the smaller element
with the smaller neighbour.
```

This is the greedy rule.

---

# Why?

Suppose

```
8
```

must be multiplied sometime.

Choices

```
8×20=160
```

or

```
8×10=80
```

Why ever choose

```
160
```

if

```
80
```

is possible?

You wouldn't.

---

# Greedy Rule

Whenever an element disappears,

pair it with

```
Smaller of

Left Greater

Right Greater
```

This minimizes the total cost.

---

# Step 5 : But How Do We Find Neighbours Efficiently?

Example

```
6 2 4 7
```

Need

nearest greater on left

nearest greater on right.

Whenever you hear

```
Nearest Greater
```

think

```
Monotonic Stack
```

---

# Why a Decreasing Stack?

Suppose

```
Stack

8

6

3
```

Current

```
5
```

Since

```
5>3
```

the

```
3
```

can never be useful again.

It should disappear now.

We calculate its cost immediately.

---

# The Algorithm

Maintain a

```
Monotonic Decreasing Stack
```

Whenever

```
Current

>

Stack Top
```

Pop.

The popped element

must disappear.

Multiply it with

```
min(

Current,

New Stack Top

)
```

---

# Why min()?

Example

```
Stack

6

2
```

Current

```
4
```

Popped

```
2
```

Neighbours

```
6

4
```

Smaller neighbour

```
4
```

Cost

```
2×4=8
```

Exactly the greedy rule.

---

# Dry Run

Example

```
6 2 4
```

---

Initial

```
Stack

∞
```

(The infinity is a sentinel to avoid empty-stack checks.)

---

Current

```
6
```

Stack

```
∞

6
```

---

Current

```
2
```

Stack

```
∞

6

2
```

---

Current

```
4
```

Now

```
4>2
```

Pop

```
2
```

Neighbours

```
6

4
```

Smaller

```
4
```

Cost

```
2×4=8
```

Answer

```
8
```

Stack

```
∞

6
```

Push

```
4
```

Stack

```
∞

6

4
```

---

Traversal finished.

Still

```
6

4
```

remain.

They must merge.

Pop

```
4
```

Cost

```
4×6=24
```

Answer

```
8+24=32
```

Done.

---

# Another Dry Run

```
4 11
```

Stack

```
∞

4
```

Current

```
11
```

Pop

```
4
```

Cost

```
4×11=44
```

Answer

```
44
```

Done.

---

# Why Does the Stack Work?

Imagine

```
10

8

3
```

Current

```
9
```

Now

```
3
```

has found

both neighbours

```
8

9
```

The smaller neighbour is

```
8
```

No future element

can make a better choice.

So

we safely remove

```
3
```

forever.

---

# Python Solution

```python
class Solution:
    def mctFromLeafValues(self, arr):

        stack = [float("inf")]
        ans = 0

        for num in arr:

            while stack[-1] <= num:

                mid = stack.pop()

                ans += mid * min(stack[-1], num)

            stack.append(num)

        while len(stack) > 2:

            ans += stack.pop() * stack[-1]

        return ans
```

---

# Complexity Analysis

### Time

```
O(N)
```

Every element

Push once

Pop once.

---

### Space

```
O(N)
```

---

# 🧠 How to Think Like an Interviewer

This is the most important part.

---

## Beginner Thinking

```
Need minimum tree

↓

Generate trees
```

Wrong.

---

## Intermediate Thinking

```
Can I use DP?
```

Possible.

There is an

```
O(N³)
```

DP solution.

---

## Advanced Thinking

Forget trees.

Ask

```
What really creates cost?
```

Answer

```
Products.
```

Then ask

```
Which numbers disappear?
```

Every number except

the largest.

Then ask

```
If a number disappears,

who should it multiply with?
```

Obviously

```
Smallest possible neighbour.
```

This greedy observation completely removes the need to build trees.

---

# Pattern Recognition

Whenever you see

```
Merge

Tree

Combine

Minimum Cost
```

Ask yourself

```
Which element disappears?

↓

Can I decide its partner greedily?

↓

Need nearest greater?

↓

Monotonic Stack
```

---

# Similar Problems

This thinking appears in

- **907. Sum of Subarray Minimums**
- **84. Largest Rectangle in Histogram**
- **496. Next Greater Element**
- **739. Daily Temperatures**
- **402. Remove K Digits**
- **1673. Most Competitive Subsequence**

All of them revolve around one idea:

> **An element becomes "finished" (or can be removed) as soon as you've found the information that determines its future contribution.**

---

# 🎯 Interview Tips

Whenever you see a problem involving trees or merges, don't immediately think "build the tree."

Ask these questions instead:

```
1. What actually contributes to the cost?

↓

2. Which elements disappear?

↓

3. Can I choose the best partner greedily?

↓

4. How do I find that partner efficiently?

↓

Nearest Greater / Smaller

↓

Monotonic Stack
```

This change in thinking is what separates someone who memorizes solutions from someone who can derive them in an interview.

---

# Key Takeaways

- Don't build every possible tree.
- Focus on **which element disappears next**.
- Every non-maximum element contributes exactly once.
- Always pair the disappearing element with the **smaller of its two greater neighbours**.
- A monotonic decreasing stack lets us discover that partner in **O(N)** time.
- This is one of the most elegant examples of combining **Greedy** and **Monotonic Stack** into a linear-time solution.

<br/><br/><br/><br/><br/>

---

# 1856. Maximum Subarray Min-Product

> **Difficulty:** Medium (Actually an Advanced Monotonic Stack Problem)
>
> **Techniques Used**
>
> - Monotonic Stack
> - Prefix Sum
> - Contribution Thinking

---

# Step 1 : Understand the Problem Like a Common Person

Let's forget DSA.

Suppose someone gives you

```
[1,2,3,2]
```

Now they ask

> Choose ANY continuous subarray.

Example

```
[2,3]

or

[2,3,2]

or

[3]

or

[1,2]
```

For every chosen subarray

do two things

```
1. Find minimum

2. Find sum
```

Multiply them.

```
Minimum × Sum
```

Return the largest answer.

---

Example

```
Subarray

[2,3,2]
```

Minimum

```
2
```

Sum

```
7
```

Product

```
14
```

---

# Example

```
[3,2,5]
```

Minimum

```
2
```

Sum

```
10
```

Answer

```
20
```

Nothing difficult yet.

---

# Step 2 : What Will a Beginner Do?

Generate every subarray.

```
for every start

    for every end

        find minimum

        find sum
```

Example

```
1

1 2

1 2 3

1 2 3 2

2

2 3

2 3 2

3

3 2

2
```

For every subarray

calculate

```
minimum

×

sum
```

Take maximum.

---

# Complexity

Subarrays

```
O(N²)
```

Finding minimum

```
O(N)
```

Finding sum

```
O(N)
```

Overall

```
O(N³)
```

Impossible.

---

# Step 3 : Improve the Brute Force

We already know

Prefix Sum

can compute

```
Sum
```

in

```
O(1)
```

So

now

```
Finding Sum

↓

O(1)
```

Still

minimum

takes

```
O(N)
```

Overall

```
O(N²)
```

Still too slow.

---

# Step 4 : Think Like an Interviewer

This is the biggest jump.

Instead of asking

```
Which subarray gives

maximum product?
```

Reverse the question.

Ask

```
Suppose

THIS element

is the minimum.

How much answer can it produce?
```

Exactly the same trick as

```
907

Sum of Subarray Minimums
```

---

# Why Reverse the Thinking?

Look

```
1 2 3 2
```

Suppose

we only focus on

```
3
```

Question

```
Can

3

be the minimum

for

[2,3] ?
```

No.

Because

```
2
```

is smaller.

---

Can

```
3
```

be minimum

for

```
[3]
```

Yes.

---

Instead of checking

millions of subarrays,

let every element

try becoming

the minimum.

---

# The New Problem

For every index

```
i
```

find

```
Largest subarray

where

nums[i]

is the minimum.
```

Then

calculate

```
nums[i]

×

sum(of that subarray)
```

Take maximum.

---

# Why Largest?

This is an extremely important observation.

Notice

```
Product

=

Minimum

×

Sum
```

The minimum

is already fixed.

Suppose

minimum

```
4
```

Now

```
4×10=40

4×15=60

4×22=88
```

The larger the sum,

the larger the answer.

So

once

minimum is fixed,

we simply want

the **largest possible subarray**

where it remains the minimum.

---

# Example

Suppose

```
5
```

is minimum.

Possible ranges

```
[5]

Sum=5

Answer=25
```

```
[7,5]

Sum=12

Answer=60
```

```
[7,5,8]

Sum=20

Answer=100
```

Obviously

take the largest.

---

# New Goal

For every element

find

```
Largest range

where

it stays

minimum.
```

---

# Step 5 : How Do We Find That Range?

Example

```
1 2 3 2
```

Focus on

```
3
```

Can we move left?

```
2

<3
```

No.

Because

then

minimum becomes

```
2
```

Stop.

---

Move right?

```
2

<3
```

Again stop.

Therefore

largest range

is

```
[3]
```

---

Now

look at

```
2

(index 1)
```

Move left

```
1

<2

stop
```

Move right

```
3

greater

continue
```

Next

```
2

equal
```

Still okay

because

minimum is still

```
2
```

Entire range

```
2 3 2
```

---

Question

How do we find

```
Previous Smaller

Next Smaller
```

efficiently?

Answer

```
Monotonic Stack
```

---

# Step 6 : Previous Smaller

Exactly like

907.

Maintain

Increasing Stack.

Whenever

```
StackTop >= Current

pop
```

Top becomes

Previous Smaller.

---

# Step 7 : Next Smaller

Traverse

```
Right

↓

Left
```

Again

Increasing Stack.

---

Now

every element knows

its boundary.

---

# Step 8 : Why Prefix Sum?

Suppose

largest range

is

```
2 3 2
```

Need

```
Sum
```

Should we

loop?

No.

Prefix Sum.

---

Example

```
1 2 3 2
```

Prefix

```
0

1

3

6

8
```

Need

```
2 3 2
```

Indices

```
1

to

3
```

Sum

```
prefix[4]-prefix[1]

=

8-1

=

7
```

Done.

---

# Formula

Suppose

current

index

```
i
```

Previous Smaller

```
left
```

Next Smaller

```
right
```

Largest valid range

```
left+1

...

right-1
```

Range Sum

```
prefix[right]

-

prefix[left+1]
```

Answer

```
nums[i]

×

Range Sum
```

---

# Dry Run

Example

```
nums

1 2 3 2
```

---

## Prefix Sum

```
Index

0 1 2 3
```

Prefix

```
0

1

3

6

8
```

---

## Previous Smaller

```
Value

1

↓

-1
```

```
2

↓

0
```

```
3

↓

1
```

```
2

↓

0
```

So

```
prev

[-1,0,1,0]
```

---

## Next Smaller

```
1

↓

4
```

```
2

↓

4
```

```
3

↓

3
```

```
2

↓

4
```

So

```
next

[4,4,3,4]
```

---

## Now Compute

---

### Index 0

```
Value

1
```

Range

```
0

to

3
```

Sum

```
8
```

Answer

```
8
```

---

### Index 1

Value

```
2
```

Range

```
1

to

3
```

Sum

```
7
```

Answer

```
2×7=14
```

Current Maximum

```
14
```

---

### Index 2

Value

```
3
```

Range

```
2

to

2
```

Sum

```
3
```

Answer

```
9
```

---

### Index 3

Value

```
2
```

Range

```
1

to

3
```

Sum

```
7
```

Answer

```
14
```

Maximum

```
14
```

Correct.

---

# Python Solution

```python
class Solution:
    def maxSumMinProduct(self, nums):
        MOD = 10**9 + 7
        n = len(nums)

        # Prefix Sum
        prefix = [0] * (n + 1)
        for i in range(n):
            prefix[i + 1] = prefix[i] + nums[i]

        prev = [-1] * n
        nxt = [n] * n

        stack = []

        # Previous Smaller
        for i in range(n):
            while stack and nums[stack[-1]] >= nums[i]:
                stack.pop()

            if stack:
                prev[i] = stack[-1]

            stack.append(i)

        stack = []

        # Next Smaller
        for i in range(n - 1, -1, -1):
            while stack and nums[stack[-1]] > nums[i]:
                stack.pop()

            if stack:
                nxt[i] = stack[-1]

            stack.append(i)

        ans = 0

        for i in range(n):
            left = prev[i] + 1
            right = nxt[i]

            total = prefix[right] - prefix[left]

            ans = max(ans, total * nums[i])

        return ans % MOD
```

---

# Complexity

Finding Prefix

```
O(N)
```

Previous Smaller

```
O(N)
```

Next Smaller

```
O(N)
```

Answer

```
O(N)
```

Overall

```
O(N)
```

Space

```
O(N)
```

---

# The Interview Thinking (Most Important)

Most beginners think like this:

```
Generate every subarray

↓

Find minimum

↓

Find sum
```

This is **subarray-first thinking**.

Strong interview candidates flip the perspective:

```
Pick one element.

↓

Assume it is the minimum.

↓

How far can it expand while remaining the minimum?

↓

That gives the largest possible sum for this minimum.

↓

Compute its contribution.
```

This is called **element-first thinking**.

---

# The Mental Transformation

Whenever you see:

```
Maximum/Minimum

+

Subarray

+

One element controls the property
```

Train yourself to ask:

> **"Can I fix one element instead of generating every subarray?"**

If the answer is yes, then the next questions are:

```
How far can this element extend?

↓

Need Previous Smaller / Greater?

↓

Monotonic Stack

↓

Need range sum?

↓

Prefix Sum
```

---

# Pattern Recognition Cheat Sheet

If a problem contains:

```
✓ Subarray
✓ Minimum or Maximum
✓ Need largest/smallest range
✓ Need range sum
```

Think immediately:

```
Monotonic Stack
        +
Prefix Sum
```

---

# Similar Problems

This exact thinking appears in:

- 907. Sum of Subarray Minimums
- 2104. Sum of Subarray Ranges
- 84. Largest Rectangle in Histogram
- 1856. Maximum Subarray Min-Product
- 2281. Sum of Total Strength of Wizards (hard)

These problems all follow the same interview pattern:

```
Fix one element
        ↓
Find its valid boundary
        ↓
Use Prefix Sum (if range sums are needed)
        ↓
Compute its contribution
```

---

# Final Takeaways

- Don't enumerate every subarray.
- Reverse the thinking: let each element try to be the minimum.
- Once the minimum is fixed, maximize the range because all numbers are positive, so a larger range means a larger sum.
- Use a **monotonic increasing stack** to find the largest range where the element remains the minimum.
- Use **prefix sums** to compute the sum of that range in O(1).
- This combination of **Monotonic Stack + Prefix Sum + Contribution Thinking** is a classic advanced interview pattern used by top product companies.

<br/><br/><br/><br/><br/>

---

# 3523. Make Array Non-decreasing

## Intuition

At first, the operation looks confusing:

> We can choose any subarray and replace it with **one element equal to the maximum** of that subarray.

But instead of thinking about **which subarray to merge**, think about **why we need to merge at all**.

The final array must be **non-decreasing**, i.e.,

```text
a[i] <= a[i+1]
```

So whenever we find

```text
current < previous
```

the order is broken.

This broken part is called a **dip**.

Our goal is to remove the minimum number of elements (merge as little as possible), because we want the **maximum possible final size**.

---

# Step 1: Understand the Operation

Suppose

```text
nums = [4,2,5,3,5]
```

Choose the subarray

```text
[2,5]
```

The maximum is

```text
5
```

So it becomes

```text
[4,5,3,5]
```

Notice:

- Two elements become one.
- The size decreases by 1.
- The maximum value remains.

---

# Step 2: When Do We Need to Merge?

If the array is already sorted,

```text
1 2 3 4
```

no operation is needed.

The answer is simply

```text
4
```

So we only need to worry when we find

```text
nums[i] < nums[last]
```

Example

```text
4 2
```

Since

```text
4 > 2
```

the array is no longer non-decreasing.

---

# Step 3: What Happens After a Dip?

Example

```text
6 2 3 4 7
```

We keep

```text
6
```

Now we see

```text
2
```

Since

```text
2 < 6
```

we cannot keep it separately.

Move ahead.

```text
3 < 6
```

Still cannot keep it.

Move ahead.

```text
4 < 6
```

Still cannot keep it.

Finally,

```text
7 >= 6
```

Now we can stop.

Everything between

```text
2 3 4
```

must become one merged block.

Final array becomes

```text
6 7
```

---

# The Important Observation

Once a dip starts,

every element smaller than the last kept value **must be merged**.

They cannot remain separate because they would always violate the non-decreasing order.

So we simply keep moving until we find

```text
nums[i] >= nums[last]
```

Everything in between disappears.

---

# Step 4: Variables Used

## last

Index of the last element that we decided to keep.

Initially,

```python
last = 0
```

---

## dipped

Indicates whether we are currently inside a decreasing segment.

Initially,

```python
dipped = False
```

---

## ans

Current size of the array.

Initially,

```python
ans = n
```

because no elements have been removed yet.

---

# Step 5: Algorithm

For every element,

### Case 1

If

```text
nums[i] < nums[last]
```

then

- a dip starts (or continues)
- mark

```python
dipped = True
```

and continue.

---

### Case 2

If

```text
nums[i] >= nums[last]
```

and we were inside a dip,

then the dip ends.

Everything between

```text
last
```

and

```text
i
```

must disappear.

Number of removed elements is

```text
i - last - 1
```

Subtract them from the answer.

Then

```python
last = i
```

---

### Case 3

If the array ends while we are still inside a dip,

everything after

```text
last
```

must disappear.

Remove

```text
n - 1 - last
```

elements.

---

# Dry Run

## Example

```text
nums = [4,2,5,3,5]
```

Initially

```text
last = 0
ans = 5
dipped = False
```

---

### i = 1

```text
2 < 4
```

Dip starts.

```text
dipped = True
```

---

### i = 2

```text
5 >= 4
```

Dip ends.

Removed elements

```text
2 - 0 - 1 = 1
```

So

```text
ans = 4
```

Now

```text
last = 2
```

---

### i = 3

```text
3 < 5
```

Another dip starts.

---

### i = 4

```text
5 >= 5
```

Dip ends.

Removed elements

```text
4 - 2 - 1 = 1
```

Now

```text
ans = 3
```

Finished.

Answer

```text
3
```

---

# Another Dry Run

```text
nums = [6,5,3,9,2,7]
```

Initially

```text
last = 0
ans = 6
```

---

### i = 1

```text
5 < 6
```

Dip starts.

---

### i = 2

```text
3 < 6
```

Continue.

---

### i = 3

```text
9 >= 6
```

Remove

```text
3 - 0 - 1 = 2
```

Removed

```text
5
3
```

Now

```text
ans = 4
last = 3
```

---

### i = 4

```text
2 < 9
```

Dip starts.

---

### i = 5

```text
7 < 9
```

Still inside the dip.

Array ends.

Remove

```text
5 - 3 = 2
```

Answer

```text
2
```

Final array can be

```text
6 9
```

---

# Why Does This Greedy Approach Work?

Suppose we have

```text
10 3 5 7
```

Can we keep

```text
3
```

No.

Can we keep

```text
5
```

Still no.

Because

```text
10 > 5
```

Can we keep

```text
7
```

Still no.

Because

```text
10 > 7
```

So all of

```text
3 5 7
```

must become **one merged block**.

There is no better choice.

That is why this greedy algorithm is correct.

---

# Python Solution

```python
from typing import List

class Solution:
    def maximumPossibleSize(self, nums: List[int]) -> int:
        n = len(nums)

        if n == 1:
            return 1

        last = 0
        dipped = False
        ans = n

        for i in range(1, n):

            if nums[i] < nums[last]:
                dipped = True
                continue

            if dipped:
                ans -= i - last - 1
                dipped = False

            last = i

        if dipped:
            ans -= (n - 1 - last)

        return ans
```

---

# Complexity Analysis

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

---

# Key Intuition to Remember

Whenever you see a problem involving **merging subarrays**, don't immediately think about trying every possible merge.

Instead ask yourself:

- **What property must the final array satisfy?**
- **Which elements make that property impossible?**
- **Are those elements forced to merge?**

Here, every element that is **smaller than the last kept element** is **forced** to belong to the same merged block until we encounter an element that is at least as large as the last kept value.

The solution doesn't decide *where* to merge—it simply identifies the elements that **cannot survive individually**.