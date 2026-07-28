# Heap Problems

Welcome to the heap problems section! Here you will find various data structure and algorithm problems related to Heaps, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [373. Find K Pairs with Smallest Sums](#373-find-k-pairs-with-smallest-sums)
- [1642. Furthest Building You Can Reach](#1642-furthest-building-you-can-reach)
- [767. Reorganize String](#767-reorganize-string)

<br><br><br><br><br>

---

# 373. Find K Pairs with Smallest Sums

- **Difficulty:** Medium
- **Pattern:** Heap + K-Way Merge + Best First Search
- **Data Structures:** Min Heap
- **Time Complexity:** O(k log(min(n, k)))
- **Space Complexity:** O(min(n, k))

---

# 🧠 Problem Statement (Explain Like a Beginner)

You are given two **sorted arrays**.

```text
nums1 = [1,7,11]
nums2 = [2,4,6]
```

You can pick **one number from nums1** and **one number from nums2**.

Every such selection forms a pair.

```
(1,2)
(1,4)
(1,6)

(7,2)
(7,4)
(7,6)

(11,2)
(11,4)
(11,6)
```

Each pair has a sum.

```
(1,2)  -> 3
(1,4)  -> 5
(1,6)  -> 7

(7,2)  -> 9
(7,4)  -> 11
(7,6)  -> 13

(11,2) -> 13
(11,4) -> 15
(11,6) -> 17
```

The problem asks:

> Return the **first k pairs having the smallest sums.**

For

```
k = 3
```

Answer is

```
[
 [1,2],
 [1,4],
 [1,6]
]
```

---

# Step 1 — Understand the Brute Force

Whenever you see

> Pick one from A
> Pick one from B

Your first thought should be

**Generate every possible pair.**

```
for every number in nums1
    for every number in nums2
```

Example

```
1 with 2
1 with 4
1 with 6

7 with 2
7 with 4
7 with 6

11 with 2
11 with 4
11 with 6
```

Store

```
(sum, pair)
```

Sort by sum.

Return first k.

---

## Brute Force Code

```python
pairs = []

for x in nums1:
    for y in nums2:
        pairs.append((x+y,[x,y]))

pairs.sort()

answer = []

for i in range(k):
    answer.append(pairs[i][1])

return answer
```

---

## Complexity

Suppose

```
n = len(nums1)
m = len(nums2)
```

Possible pairs

```
n × m
```

If

```
100000 × 100000
```

that is

```
10^10 pairs
```

Impossible.

So brute force cannot work.

---

# Step 2 — Read the Constraints Carefully

Look again.

```
nums1 is sorted

nums2 is sorted
```

This line is NOT extra information.

Interviewers intentionally give this.

Whenever you see

```
sorted
```

stop.

Ask yourself

> How can I use sorting to avoid checking everything?

---

# Step 3 — Visualize the Problem

Instead of thinking about arrays...

Think about a matrix.

Rows represent nums1.

Columns represent nums2.

```
         2   4   6
      ----------------
1   |    3   5   7
7   |    9  11  13
11  |   13  15  17
```

Each cell stores

```
nums1[i] + nums2[j]
```

Notice something?

Every row is sorted.

```
3
5
7
```

Every column is sorted.

```
3
9
13
```

This is the biggest clue.

---

# Step 4 — Important Observation

Suppose I ask

"What is the smallest sum?"

Easy.

```
3
```

which is

```
1+2
```

located here

```
(0,0)
```

---

Now suppose we remove it.

Question

Where can the SECOND smallest be?

Can it suddenly become

```
13 ?
```

No.

It must be close to the smallest.

There are only TWO possibilities.

Move RIGHT

```
1+4 = 5
```

Move DOWN

```
7+2 = 9
```

Picture

```
      3 -----> 5
      |
      |
      V
      9
```

Nothing else can be smaller.

Why?

Because rows and columns are sorted.

This is the entire intuition.

---

# Step 5 — This Looks Like BFS

Think of every cell as a graph node.

```
(0,0)
```

has neighbors

```
Right

(0,1)
```

and

```
Down

(1,0)
```

Exactly like BFS.

Difference?

Instead of

```
Queue
```

we need

```
Smallest sum first.
```

So we replace

```
Queue
```

with

```
Min Heap
```

---

# Step 6 — Why Heap?

Ask yourself

After exploring

```
5
9
```

Which should be processed next?

```
5
```

After exploring more

Heap may contain

```
7
11
9
13
```

Again

Need smallest.

Heap gives

```
O(log n)
```

instead of searching every time.

---

# Step 7 — Can We Improve Further?

Look carefully.

Matrix

```
         2   4   6
      ----------------
1   |    3   5   7
7   |    9  11  13
11  |   13  15  17
```

Every row is already sorted.

Instead of beginning with only

```
3
```

Let's begin with the FIRST element of every row.

Heap

```
3
9
13
```

These are

```
(0,0)

(1,0)

(2,0)
```

Now what happens?

Whenever we remove one element from a row...

We simply move RIGHT.

Because

```
5
```

is the next candidate from that row.

We NEVER need to move DOWN.

Every row already entered the heap.

This removes duplicates completely.

---

# Why Does This Work?

Suppose we pop

```
3
```

Next possible element from SAME row

```
5
```

must be the next smallest candidate of that row.

Heap automatically compares

```
5

9

13
```

and chooses

```
5
```

Exactly what we want.

---

# Step 8 — Dry Run

Example

```
nums1=[1,7,11]

nums2=[2,4,6]

k=3
```

---

## Initial Heap

Insert first element of every row.

```
3 (0,0)

9 (1,0)

13 (2,0)
```

Heap

```
3
9
13
```

Answer

```
[]
```

---

## Pop

Take

```
3
```

Answer

```
[1,2]
```

Push next element in same row

```
5
```

Heap

```
5
9
13
```

---

## Pop

Take

```
5
```

Answer

```
[1,2]

[1,4]
```

Push

```
7
```

Heap

```
7
9
13
```

---

## Pop

Take

```
7
```

Answer

```
[1,2]

[1,4]

[1,6]
```

Done.

---

# Step 9 — Why Only Move Right?

Imagine each row separately.

Row 1

```
3
5
7
```

Once

```
3
```

is removed

The only unseen candidate in this row is

```
5
```

Not

```
7
```

because

```
5 < 7
```

So moving right always gives the next candidate.

---

# Thinking Process During Interviews

Whenever you see

```
Multiple sorted arrays
```

Immediately ask

```
Can I merge them?
```

If yes

Think

```
Heap
```

---

Whenever you see

```
Need first k

Need smallest

Need largest

Need next smallest
```

Think

```
Heap
```

---

# Pattern Recognition

If problem says

```
Two sorted arrays
```

Think

```
Matrix
```

If matrix rows are sorted

Think

```
K-Way Merge
```

If repeatedly need

```
Smallest candidate
```

Think

```
Min Heap
```

---

# Common Mistake

Many beginners think

```
Generate everything

Sort

Take k
```

Instead ask

```
Do I really need every pair?
```

Answer

```
No.
```

Need only

```
k
```

Therefore

Generate pairs **only when required.**

This idea is called

```
Lazy Generation
```

instead of

```
Complete Generation.
```

---

# Python Solution

```python
import heapq

class Solution:
    def kSmallestPairs(self, nums1, nums2, k):

        if not nums1 or not nums2:
            return []

        heap = []

        # Put the first element from each row into the heap
        for i in range(min(len(nums1), k)):
            heapq.heappush(heap, (nums1[i] + nums2[0], i, 0))

        answer = []

        while heap and len(answer) < k:

            total, i, j = heapq.heappop(heap)

            answer.append([nums1[i], nums2[j]])

            # Push the next element from the same row
            if j + 1 < len(nums2):
                heapq.heappush(
                    heap,
                    (nums1[i] + nums2[j + 1], i, j + 1)
                )

        return answer
```

---

# Complexity

```
Heap Size = min(n, k)
```

Each heap operation

```
O(log(min(n,k)))
```

We perform at most

```
k pops

k pushes
```

Overall

```
Time  : O(k log(min(n,k)))

Space : O(min(n,k))
```

---

# Final Intuition (Remember Forever)

Whenever you encounter a problem with:

- Multiple **sorted** sources
- Need only the **first k** smallest/largest results
- Generating all combinations is too expensive

Don't think **"generate everything."**

Instead think:

1. **Start with the smallest candidate from each sorted source.**
2. **Always process the current smallest using a min heap.**
3. **After using one candidate, generate only its next possible candidate.**

This is the **K-Way Merge / Best First Search** pattern, and it appears in many advanced heap problems.

<br/><br/><br/><br/><br/>

---

# 1642. Furthest Building You Can Reach

- **Difficulty:** Medium
- **Pattern:** Greedy + Min Heap
- **Data Structure:** Min Heap

---

# 🧠 Step 1: Understand the Problem Like a Common Man

Imagine you are walking through a city.

Every building has a different height.

```
4 → 2 → 7 → 6 → 9 → 14 → 12
```

You start from the first building.

Your goal is simple:

> Reach as far as possible.

---

Whenever you move,

there are three possibilities.

---

## Case 1 : Next building is shorter

```
7 → 5
```

Easy.

You simply walk down.

Need

```
Nothing
```

---

## Case 2 : Same height

```
7 → 7
```

Again

Need

```
Nothing
```

---

## Case 3 : Next building is taller

```
2 → 7
```

Need to climb

```
5 units
```

Now you have two resources.

```
Bricks

or

Ladder
```

---

Bricks

```
Need exactly

height difference
```

Example

```
2 → 7

Need

5 bricks
```

---

Ladder

Amazing thing about ladder

Whether climb is

```
2

or

200

or

100000
```

Still

```
Only one ladder.
```

---

# Step 2 : What is Actually Asked?

The question is NOT

```
Can you reach the last building?
```

The question is

```
How far can you go
if you use your resources wisely?
```

This word

```
wisely
```

is the biggest clue.

---

# Step 3 : First Thought (Brute Force)

Suppose

```
Bricks = 5

Ladders = 1
```

Climbs

```
5

3

8
```

At every climb,

you have two choices.

```
Bricks

or

Ladder
```

Decision Tree

```
              Start

          /             \

      Bricks          Ladder

      /    \          /     \

   ...
```

If there are

```
20 climbs
```

Total possibilities

```
2^20
```

Impossible.

---

# Step 4 : Why This Problem Feels Hard

Let's say

```
Bricks = 10

Ladders = 1
```

Climbs

```
2

8

3
```

Question

Where should ladder be used?

```
2 ?

8 ?

3 ?
```

Nobody knows.

Because

Future is unknown.

This is exactly why people get stuck.

---

# Step 5 : Think Like a Normal Person

Imagine you're hiking.

Someone asks

> "Use your ladder now?"

Would you?

Probably NOT.

Because

You don't know whether

a much taller wall

comes later.

A normal person naturally thinks

```
I'll save my ladder.
```

That is already the correct direction.

---

# Step 6 : Biggest Observation

Bricks depend on climb.

```
Need

2 bricks

5 bricks

20 bricks
```

Ladder does NOT.

```
1 ladder

works for

2

or

20

or

1000
```

So

What saves more resources?

Using ladder on

```
20
```

instead of

```
2
```

Obviously.

---

# First Greedy Idea

Always use ladders for

```
Largest climbs.
```

This is the most important intuition.

---

# Step 7 : New Problem

How do we know

which climbs are largest?

Example

```
5

3

8

2
```

When we see

```
5
```

We don't know

```
8
```

is coming later.

So we cannot decide immediately.

---

# Step 8 : The Genius Trick

Instead of deciding immediately,

pretend

```
Every climb uses ladder.
```

Example

```
5

3

8
```

Heap

```
3

5

8
```

Suppose

```
Ladders = 2
```

Oops.

Need

```
3 ladders.
```

Only have

```
2
```

Question

Which climb should lose the ladder?

Obviously

```
3
```

Because

Using bricks on

```
3
```

is much cheaper than

```
8
```

So

```
3

→ Bricks

5

→ Ladder

8

→ Ladder
```

This is the entire greedy solution.

---

# Step 9 : Why Min Heap?

Heap stores

```
All climbs currently using ladders.
```

Example

```
Heap

3

5

8
```

Need only

```
2 ladders
```

So remove

```
smallest climb
```

Min Heap removes

```
smallest
```

instantly.

Exactly what we need.

---

# Step 10 : Mathematical Logic

Suppose

Largest climbs are

```
20

12

8

3
```

Ladders = 2

Wrong assignment

```
20 → Bricks

3 → Ladder
```

Cost

```
20 bricks
```

Better assignment

```
20 → Ladder

3 → Bricks
```

Cost

```
3 bricks
```

Huge improvement.

Therefore

Every optimal solution must satisfy

```
Largest climbs

↓

Ladders
```

This is called the

**Greedy Exchange Argument**.

---

# Step 11 : Dry Run

Example

```
heights

4

2

7

6

9

14

12

Bricks = 5

Ladders = 1
```

---

## Building 0 → 1

```
4 → 2
```

Going down.

Need nothing.

Heap

```
[]
```

Bricks

```
5
```

---

## Building 1 → 2

```
2 → 7
```

Need climb

```
5
```

Pretend ladder.

Heap

```
5
```

Heap size

```
1
```

Equal to ladders.

Good.

Bricks

```
5
```

---

## Building 2 → 3

```
7 → 6
```

Down.

Nothing.

Heap

```
5
```

---

## Building 3 → 4

```
6 → 9
```

Need

```
3
```

Heap

```
3

5
```

Need

```
2 ladders
```

Have

```
1
```

Remove smallest

```
3
```

Use bricks instead.

Bricks

```
5-3=2
```

Heap

```
5
```

Meaning

Ladder is now reserved for

```
5
```

---

## Building 4 → 5

```
9 → 14
```

Need

```
5
```

Heap

```
5

5
```

Again

Need

```
2 ladders
```

Only

```
1
```

Remove smallest

```
5
```

Use bricks.

Bricks

```
2-5=-3
```

Negative.

Cannot continue.

Return

```
4
```

Correct.

---

# Step 12 : Why Does This Always Work?

Suppose climbs are

```
2

6

10

15
```

Ladders = 2

Would any smart person use ladder on

```
2
```

instead of

```
15
```

Never.

Because

```
Ladder on 15

saves

15 bricks
```

whereas

```
Ladder on 2

saves only

2 bricks.
```

Therefore

Largest climbs

↓

Ladders

Smallest climbs

↓

Bricks

This is the greedy proof.

---

# Step 13 : Algorithm

For every building

Find

```
climb

=

heights[i+1]-heights[i]
```

If

```
climb <= 0
```

Continue.

Otherwise

Push climb into heap.

Heap means

```
Currently using ladder.
```

If

```
Heap size > ladders
```

Remove smallest climb.

Convert it into bricks.

```
bricks -= smallest climb
```

If

```
bricks < 0
```

Cannot continue.

Return current building.

Otherwise

Reach end.

---

# Python Code

```python
import heapq

class Solution:
    def furthestBuilding(self, heights, bricks, ladders):

        heap = []

        for i in range(len(heights)-1):

            climb = heights[i+1]-heights[i]

            if climb <= 0:
                continue

            heapq.heappush(heap, climb)

            if len(heap) > ladders:
                bricks -= heapq.heappop(heap)

            if bricks < 0:
                return i

        return len(heights)-1
```

---

# Complexity

Let

```
n

=

number of buildings
```

Every climb

is inserted once.

Removed once.

Heap size

never exceeds

```
ladders+1
```

Therefore

```
Time

O(n log L)
```

where

```
L

=

number of ladders
```

Space

```
O(L)
```

---

# How Should a Beginner Think?

When solving any interview problem,

don't immediately ask

```
Which data structure?
```

Instead ask these questions.

---

## Question 1

Can I decide immediately?

Here

No.

Future climbs are unknown.

---

## Question 2

If I look back later,

what would be the perfect decision?

Answer

```
Largest climbs

↓

Ladders
```

---

## Question 3

What information must I remember?

Answer

```
Every climb that currently has a ladder.
```

---

## Question 4

If I have more ladder-assigned climbs than ladders,

which one should lose its ladder?

Answer

```
Smallest climb.
```

---

## Question 5

Which data structure always gives me the smallest climb quickly?

Answer

```
Min Heap.
```

---

# Interview Thinking Pattern

Whenever you see

```
Limited premium resource

+

Unlimited variable-cost resource
```

Examples

- Ladders vs Bricks
- Coupons vs Money
- Free passes vs Ticket cost

Think

```
Use premium resource where it saves the MOST.
```

If the best assignment is only known after seeing more data,

maintain the current best choices using a **heap**, and whenever you exceed the limit, replace the **least valuable** premium usage with the cheaper resource.

This is the core intuition behind this problem and many advanced **Greedy + Heap** interview questions.

<br/><br/><br/><br/><br/>

---

# 767. Reorganize String

- **Difficulty:** Medium
- **Pattern:** Greedy + Frequency Counting
- **Data Structures:** Counter / Sorting (Heap is another possible solution)

---

# 🧠 Step 1: Understand the Problem Like a Common Man

Suppose you are arranging students in a row.

Each student is represented by a letter.

```
aab
```

means

```
Student A
Student A
Student B
```

Your task is

> Arrange them so that **no two same students sit next to each other.**

Valid arrangement

```
A B A
```

Invalid

```
A A B
```

because

```
AA
```

are adjacent.

---

Another example

```
aaab
```

Can we arrange?

Try

```
A A A B
```

No.

Try

```
A B A A
```

Still

```
AA
```

Try

```
A A B A
```

Again

```
AA
```

Impossible.

Return

```
""
```

---

# Step 2 : What is the Real Goal?

The problem is NOT asking

```
Sort characters.
```

It is asking

```
Keep same characters apart.
```

That word

```
apart
```

is the biggest clue.

---

# Step 3 : First Brute Force Thinking

Suppose

```
aab
```

Generate every arrangement.

```
aab

aba

baa
```

Check each one.

```
Does it contain

AA ?
```

If yes

Reject.

---

## Complexity

If

```
n = 10
```

Total permutations

```
10!
```

Which is

```
3,628,800
```

Impossible.

So brute force is not practical.

---

# Step 4 : Observe the Problem

Example

```
aaabbc
```

Frequency

```
a → 3

b → 2

c → 1
```

Which character is most dangerous?

Obviously

```
a
```

Because it appears most.

The more times a character appears,

the harder it is to separate.

---

# Step 5 : Think Like a Common Man

Imagine you are arranging chairs.

```
_ _ _ _ _ _
```

Student

```
A
```

must sit

```
3
```

times.

Where should you place them?

Not like

```
AAA___
```

because

```
AA
```

becomes adjacent.

Instead,

spread them first.

```
A _ A _ A _
```

Now,

fill the remaining empty seats.

```
A B A C A B
```

Done.

No two

```
A
```

are adjacent.

---

This is the biggest intuition.

---

# Step 6 : Why Place the Most Frequent Character First?

Suppose

```
A appears 5 times.
```

If you leave it for the end,

there may not be enough gaps.

Example

```
BBBBCCCCAAAAA
```

Now impossible to spread

```
AAAAA
```

Instead,

place

```
AAAAA
```

first.

```
A _ A _ A _ A _ A
```

Now other characters naturally fill the gaps.

This is a Greedy strategy.

---

# Step 7 : Biggest Mathematical Observation

Suppose

Length

```
7
```

Most frequent character

```
A appears 5 times.
```

Can we separate them?

Need arrangement

```
A _ A _ A _ A _ A
```

Count positions

Need

```
9 positions
```

But only

```
7
```

exist.

Impossible.

---

General Rule

If

```
Maximum Frequency

>

(n+1)//2
```

Then

Impossible.

Return

```
""
```

Example

```
aaab
```

Length

```
4
```

Maximum frequency

```
3
```

Maximum allowed

```
(4+1)//2 = 2
```

Since

```
3 > 2
```

Impossible.

---

# Step 8 : Why Alternate Positions?

Suppose

```
Length = 8
```

Indices

```
0 1 2 3 4 5 6 7
```

First fill

```
0

2

4

6
```

These positions are already separated.

```
A _ A _ A _ A _
```

Now continue

```
1

3

5

7
```

```
A B A C A D A E
```

Automatically,

same letters never touch.

---

# Step 9 : Algorithm

Count frequency.

Sort characters by frequency.

Start filling

```
0

2

4

6
```

When even positions finish,

continue

```
1

3

5

7
```

Return the final string.

---

# Step 10 : Dry Run

Input

```
s = "aab"
```

---

## Frequency

```
a → 2

b → 1
```

Sorted

```
a

b
```

---

## Initial Answer

```
_ _ _
```

Current Position

```
0
```

---

### Place first a

```
A _ _
```

Next

```
2
```

---

### Place second a

```
A _ A
```

Next

```
4
```

Out of range.

Move to

```
1
```

---

### Place b

```
A B A
```

Done.

Answer

```
ABA
```

---

# Another Dry Run

```
s = "aaabbc"
```

Frequency

```
a = 3

b = 2

c = 1
```

Answer

```
_ _ _ _ _ _
```

Fill

```
0

2

4
```

```
A _ A _ A _
```

Now

```
b
```

Fill

```
1

3
```

```
A B A B A _
```

Now

```
c
```

Fill

```
5
```

```
A B A B A C
```

Perfect.

---

# Why Does This Always Work?

The most frequent character is the hardest to place.

If even positions are enough to hold all copies,

then

every remaining character has fewer occurrences,

so they can always fit into the remaining empty positions.

Therefore

placing the largest frequency first

is always safe.

This is the Greedy proof.

---

# Python Solution

```python
from collections import Counter

class Solution:
    def reorganizeString(self, s: str) -> str:

        n = len(s)

        freq = Counter(s)

        # Impossible case
        if max(freq.values()) > (n + 1) // 2:
            return ""

        ans = [""] * n

        # Sort by decreasing frequency
        chars = sorted(freq.items(), key=lambda x: -x[1])

        index = 0

        for ch, count in chars:

            while count > 0:

                ans[index] = ch

                index += 2

                if index >= n:
                    index = 1

                count -= 1

        return "".join(ans)
```

---

# Complexity

Let

```
n

=

length of string
```

Counting frequency

```
O(n)
```

Sorting

At most

```
26 letters
```

So

```
O(26 log 26)

≈ O(1)
```

Placing characters

```
O(n)
```

Overall

```
Time

O(n)
```

Space

```
O(n)
```

---

# How Should a Beginner Think?

Whenever you see a problem like this,

don't immediately think

```
How do I arrange characters?
```

Instead ask

### Question 1

Which character is hardest to place?

Answer

```
Most frequent character.
```

---

### Question 2

How can I keep identical characters apart?

Answer

```
Leave one empty space between them.
```

---

### Question 3

What is the safest way to create gaps?

Answer

Fill

```
0

2

4

6
```

first.

---

### Question 4

Once the hardest character is placed,

what happens?

Answer

All remaining characters have smaller frequencies,

so they naturally fit into the remaining spaces.

---

# Pattern Recognition

Whenever you see

- Rearrange elements
- Avoid adjacent duplicates
- Most frequent element is the main obstacle

Think

```
Frequency Counting

+

Greedy

+

Place the highest frequency element first.
```

---

# Real-Life Analogy

Imagine organizing guests at a wedding.

One family has

```
5 members
```

and they keep arguing if they sit together.

What would you do?

You wouldn't seat them like

```
A A A A A
```

Instead,

spread them across the hall.

```
A _ A _ A _ A _ A
```

Then fill the remaining seats with guests from other families.

Exactly the same idea is used in this problem.

---

# Final Intuition to Remember Forever

> The character with the highest frequency is the hardest to separate. Place it first with maximum spacing (every alternate position). Once the hardest character is handled, every remaining character has fewer occurrences and can safely fill the remaining gaps.