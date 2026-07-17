# Monotonic Stack Problems

Welcome to the monotonic stack problems section! Here you will find various data structure and algorithm problems related to Monotonic Stack, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [316. Remove Duplicate Letters](#316-remove-duplicate-letters)
- [1574. Shortest Subarray to be Removed to Make Array Sorted](#1574-shortest-subarray-to-be-removed-to-make-array-sorted)
- [1673. Find the Most Competitive Subsequence](#1673-find-the-most-competitive-subsequence)

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