# Heap Problems

Welcome to the heap problems section! Here you will find various data structure and algorithm problems related to Heaps, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [373. Find K Pairs with Smallest Sums](#373-find-k-pairs-with-smallest-sums)

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