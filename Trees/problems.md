# Tree Problems

Welcome to the tree problems section! Here you will find various data structure and algorithm problems related to Trees, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [1008. Construct Binary Search Tree from Preorder Traversal](#1008-construct-binary-search-tree-from-preorder-traversal)
- [102. Binary Tree Level Order Traversal](#102-binary-tree-level-order-traversal)
- [2265. Count Nodes Equal to Average of Subtree](#2265-count-nodes-equal-to-average-of-subtree)
- [1026. Maximum Difference Between Node and Ancestor](#1026-maximum-difference-between-node-and-ancestor)
- [1315. Sum of Nodes with Even-Valued Grandparent](#1315-sum-of-nodes-with-even-valued-grandparent)
- [979. Distribute Coins in Binary Tree](#979-distribute-coins-in-binary-tree)
- [1022. Sum of Root To Leaf Binary Numbers](#1022-sum-of-root-to-leaf-binary-numbers)

<br><br><br><br><br>

---

# <span style="color: limegreen;">1008. Construct Binary Search Tree from Preorder Traversal</span>

- **Difficulty:** Medium
- **Topics:** Binary Search Tree, Tree, Array

## Problem Statement

Given an array of integers `preorder`, which represents the **preorder traversal** of a **Binary Search Tree (BST)**, construct the tree and return its root.

It is guaranteed that there is always a valid Binary Search Tree corresponding to the given preorder traversal.

A **Binary Search Tree (BST)** is a binary tree where:

- Every node in the left subtree has a value **strictly less** than the node's value.
- Every node in the right subtree has a value **strictly greater** than the node's value.

A **Preorder Traversal** visits the nodes in the following order:

```text
Root → Left → Right
```

---

## Example 1

**Input**

```text
preorder = [8,5,1,7,10,12]
```

**Output**

```text
[8,5,10,1,7,null,12]
```

**Explanation**

The constructed BST is:

```text
          8
        /   \
       5     10
      / \      \
     1   7      12
```

The preorder traversal of this tree is:

```text
8 → 5 → 1 → 7 → 10 → 12
```

---

## Example 2

**Input**

```text
preorder = [1,3]
```

**Output**

```text
[1,null,3]
```

**Explanation**

The constructed BST is:

```text
    1
     \
      3
```

The preorder traversal is:

```text
1 → 3
```

### Intuition

In a **BST**:

* Left subtree values `< root`
* Right subtree values `> root`

In **Preorder Traversal**:

```text
Root → Left → Right
```

Example:

```python
preorder = [8,5,1,7,10,12]
```

The first element is always the root:

```text
        8
```

Everything smaller than `8` belongs to the left subtree:

```text
[5,1,7]
```

Everything greater than `8` belongs to the right subtree:

```text
[10,12]
```

Recursively:

```text
        8
      /   \
     5     10
    / \      \
   1   7      12
```

---

# Optimal O(n) Solution

Instead of searching for left/right boundaries repeatedly, we use:

* A global index `i`
* A valid range `(min_val, max_val)`

A node can only be created if its value lies within the allowed range.

---

### Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def bstFromPreorder(self, preorder):
        self.i = 0

        def build(bound):
            if self.i == len(preorder) or preorder[self.i] > bound:
                return None

            root = TreeNode(preorder[self.i])
            self.i += 1

            root.left = build(root.val)
            root.right = build(bound)

            return root

        return build(float('inf'))
```

---

# Dry Run

Input:

```python
preorder = [8,5,1,7,10,12]
```

### Step 1

```python
build(inf)
```

Create:

```text
8
```

Move index → 1

---

### Step 2 (Left of 8)

```python
build(8)
```

Create:

```text
5
```

Move index → 2

---

### Step 3 (Left of 5)

```python
build(5)
```

Create:

```text
1
```

Move index → 3

Next value:

```python
7 > 1
```

Cannot go left.

Return.

---

### Step 4 (Right of 1)

```python
build(5)
```

Current value:

```python
7 > 5
```

Return None.

So:

```text
   1
```

completed.

---

### Step 5 (Right of 5)

```python
build(8)
```

Create:

```text
7
```

Move index → 4

---

### Step 6

Current value:

```python
10 > 8
```

Stop left subtree.

Return.

---

### Step 7 (Right of 8)

```python
build(inf)
```

Create:

```text
10
```

Move index → 5

---

### Step 8

Left:

```python
12 > 10
```

No left child.

Right:

Create:

```text
12
```

Done.

Final BST:

```text
        8
      /   \
     5     10
    / \      \
   1   7      12
```

---

# Why does this work?

The preorder sequence is:

```text
Root → Left → Right
```

When building a subtree:

* All values must be less than the parent's upper bound.
* If a value exceeds the bound, it belongs to some ancestor's right subtree.

That's why we use:

```python
build(root.val)   # left subtree
build(bound)      # right subtree
```

---

# Complexity

| Operation | Complexity |
| --------- | ---------- |
| Time      | O(n)       |
| Space     | O(h)       |

Where:

* `n` = number of nodes
* `h` = height of BST

Worst case:

```text
1
 \
  2
   \
    3
```

Space = `O(n)`

Balanced BST:

```text
Space = O(log n)
```

This is the most efficient solution commonly expected in interviews for LeetCode 1008.

<br><br><br><br><br>

---

# <span style="color: limegreen;">102. Binary Tree Level Order Traversal</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, Breadth-First Search (BFS), Queue

---

# Problem Statement

Given the `root` of a binary tree, return the **level order traversal** of its nodes' values.

In other words, visit the tree **level by level**, from **left to right**.

---

## Example 1

### Input

```text
        3
       / \
      9   20
         /  \
        15   7
```

```text
root = [3,9,20,null,null,15,7]
```

### Output

```python
[[3], [9,20], [15,7]]
```

---

## Example 2

**Input**

```text
root = [1]
```

Output

```python
[[1]]
```

---

## Example 3

**Input**

```text
root = []
```

Output

```python
[]
```

---

# Constraints

- Number of nodes is in the range **[0, 2000]**
- `-1000 <= Node.val <= 1000`

---

# Intuition

The problem asks us to traverse the tree **level by level**.

For the tree:

```text
        3
       / \
      9   20
         /  \
        15   7
```

We should visit nodes like this:

```text
Level 0

3

↓

Level 1

9   20

↓

Level 2

15   7
```

Result

```python
[[3], [9,20], [15,7]]
```

---

# Approach: Breadth First Search (BFS)

Since we want to process nodes **level by level**, the best choice is **Breadth First Search (BFS)**.

BFS uses a **Queue (FIFO)**.

A queue always processes nodes in the same order they were inserted, making it perfect for level-wise traversal.

---

# Algorithm

1. If the tree is empty, return `[]`.
2. Create an empty answer list.
3. Put the root into a queue.
4. While the queue is not empty:
   - Find the number of nodes in the current level (`level_size`).
   - Process exactly those nodes.
   - Store their values in a temporary list.
   - Add their left and right children to the queue.
5. Append the current level to the answer.
6. Return the answer.

---

# Visualization

Tree

```text
        3
       / \
      9   20
         /  \
        15   7
```

---

### Initial State

```text
Queue

[3]
```

Output

```text
[]
```

---

### Process Level 0

Remove

```text
3
```

Add children

```text
9
20
```

Queue becomes

```text
[9,20]
```

Output

```text
[[3]]
```

---

### Process Level 1

Remove

```text
9
20
```

Add children of 20

```text
15
7
```

Queue becomes

```text
[15,7]
```

Output

```text
[[3],[9,20]]
```

---

### Process Level 2

Remove

```text
15
7
```

No children remain.

Queue

```text
[]
```

Output

```text
[[3],[9,20],[15,7]]
```

---

# Python Code

```python
from collections import deque

class Solution:
    def levelOrder(self, root):
        # If the tree is empty
        if not root:
            return []

        ans = []

        # Queue for BFS
        q = deque([root])

        # Continue until all nodes are processed
        while q:

            # Number of nodes in the current level
            level_size = len(q)

            # Store nodes of the current level
            level = []

            # Process one complete level
            for _ in range(level_size):

                node = q.popleft()

                # Store node value
                level.append(node.val)

                # Add left child
                if node.left:
                    q.append(node.left)

                # Add right child
                if node.right:
                    q.append(node.right)

            # Store the completed level
            ans.append(level)

        return ans
```

---

# Dry Run

Input Tree

```text
        3
       / \
      9   20
         /  \
        15   7
```

---

## Step 1

Queue

```text
[3]
```

Process

```text
3
```

Queue

```text
[9,20]
```

Answer

```python
[[3]]
```

---

## Step 2

Queue

```text
[9,20]
```

Process

```text
9
20
```

Queue

```text
[15,7]
```

Answer

```python
[[3],[9,20]]
```

---

## Step 3

Queue

```text
[15,7]
```

Process

```text
15
7
```

Queue

```text
[]
```

Answer

```python
[[3],[9,20],[15,7]]
```

---

# Why Do We Use `level_size`?

Consider this queue:

```text
[9,20]
```

Both nodes belong to the **same level**.

If we simply keep removing nodes without knowing the level size, we would also process `15` and `7`, mixing two different levels.

So we first save:

```python
level_size = len(queue)
```

Then process exactly that many nodes.

This guarantees that each iteration of the `while` loop processes **one complete level**.

---

# Complexity Analysis

## Time Complexity

Every node is visited exactly once.

```text
O(n)
```

where `n` is the number of nodes.

---

## Space Complexity

The queue can contain an entire level of the tree.

Worst case (complete binary tree):

```text
O(n)
```

---

# Pattern Recognition

Whenever you see words like:

- Level Order Traversal
- Level by Level
- Breadth First Search
- Nodes at Distance K
- Minimum Depth
- Shortest Path in an Unweighted Tree

Think immediately:

```text
Tree

↓

Level-wise Processing

↓

BFS

↓

Queue
```

---

# Key Takeaways

- **Level Order Traversal** is a **Breadth First Search (BFS)** problem.
- A **Queue** is the ideal data structure because it processes nodes in **FIFO (First In, First Out)** order.
- `level_size` ensures that each iteration processes exactly one level.
- Every node is visited only once, giving an **O(n)** time complexity.

<br><br><br><br><br>

---

# <span style="color: limegreen;">2265. Count Nodes Equal to Average of Subtree</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, Postorder Traversal

---

# 💡 How to Think About This Problem

Whenever you see words like:

- Subtree Sum
- Subtree Count
- Subtree Average
- Subtree Maximum
- Subtree Minimum

Ask yourself:

> **Can I calculate this node's answer before visiting its children?**

For this problem, the answer is **No**.

To calculate the average of a node's subtree, we need:

```
1. Sum of all nodes in its subtree
2. Number of nodes in its subtree
```

Where do these values come from?

From the **left subtree** and **right subtree**.

So we must process the children first.

This immediately tells us:

```text
Left
↓

Right
↓

Current Node
```

which is exactly **Postorder DFS**.

---

# Step 1: What should DFS return?

The parent needs two pieces of information from every child:

```text
Sum of subtree

Count of subtree
```

So let DFS return

```python
(sum, count)
```

For every node.

---

# Example

```text
        4
       / \
      8   5
     / \   \
    0   1   6
```

Let's solve it exactly like the recursion does.

---

# Visit Node 0

Subtree

```text
0
```

Sum

```
0
```

Count

```
1
```

Average

```
0 // 1 = 0
```

Node Value

```
0
```

Match ✅

Answer = 1

Return to parent

```python
(0, 1)
```

---

# Visit Node 1

Subtree

```text
1
```

Sum

```
1
```

Count

```
1
```

Average

```
1 // 1 = 1
```

Match ✅

Answer = 2

Return

```python
(1,1)
```

---

# Visit Node 8

Now both children have already returned.

Left child returned

```python
(0,1)
```

Right child returned

```python
(1,1)
```

Current node

```
8
```

Total Sum

```
0 + 1 + 8

=

9
```

Total Count

```
1 + 1 + 1

=

3
```

Average

```
9 // 3

=

3
```

Node value

```
8
```

Not Equal ❌

Return

```python
(9,3)
```

---

# Visit Node 6

Subtree

```text
6
```

Average

```
6//1

=

6
```

Match ✅

Answer = 3

Return

```python
(6,1)
```

---

# Visit Node 5

Left child

```
None

↓

(0,0)
```

Right child

```python
(6,1)
```

Current value

```
5
```

Total Sum

```
0 + 6 + 5

=

11
```

Total Count

```
0 + 1 + 1

=

2
```

Average

```
11 // 2

=

5
```

Node value

```
5
```

Match ✅

Answer = 4

Return

```python
(11,2)
```

---

# Visit Root 4

Left child returned

```python
(9,3)
```

Right child returned

```python
(11,2)
```

Current node

```
4
```

Total Sum

```
9 + 11 + 4

=

24
```

Total Count

```
3 + 2 + 1

=

6
```

Average

```
24 // 6

=

4
```

Node Value

```
4
```

Match ✅

Final Answer = **5**

---

# Recursion Tree

```text
                    4
                 /     \
               8         5
             /   \        \
           0      1        6
            ↑      ↑        ↑
          (0,1)  (1,1)    (6,1)
                \   /        |
                 (9,3)    (11,2)
                      \     /
                      (24,6)
```

Notice something important:

Every parent waits for its children.

This is why Postorder is perfect.

---

# Python Code

```python
class Solution:
    def averageOfSubtree(self, root: Optional[TreeNode]) -> int:

        ans = 0

        def dfs(node):
            nonlocal ans

            # Empty subtree
            if node is None:
                return (0, 0)

            # Get information from left subtree
            left_sum, left_count = dfs(node.left)

            # Get information from right subtree
            right_sum, right_count = dfs(node.right)

            # Calculate current subtree
            total_sum = left_sum + right_sum + node.val
            total_count = left_count + right_count + 1

            average = total_sum // total_count

            # Check if current node equals average
            if average == node.val:
                ans += 1

            # Return information to parent
            return (total_sum, total_count)

        dfs(root)

        return ans
```

---

# Why Does DFS Return `(sum, count)`?

Imagine the parent is node **8**.

To compute its average, it asks its children:

Left child says:

```python
(sum=0, count=1)
```

Right child says:

```python
(sum=1, count=1)
```

Now parent already has everything.

It simply does

```text
Total Sum

=

left_sum

+

right_sum

+

current value
```

and

```text
Total Count

=

left_count

+

right_count

+

1
```

Then returns the new values upward.

This is exactly how recursion builds the answer from the bottom to the top.

---

# Complexity Analysis

## Time Complexity

Every node is visited exactly once.

```text
O(n)
```

---

## Space Complexity

The recursion stack stores at most one path from the root to a leaf.

```text
O(h)
```

where:

- `n` = number of nodes
- `h` = height of the tree

---

# Pattern Recognition

Whenever a tree problem asks for:

- Subtree Sum
- Subtree Size
- Subtree Average
- Maximum of Subtree
- Minimum of Subtree
- Information that the parent depends on

Immediately think:

```text
Parent needs information from children

↓

Postorder DFS

↓

Return multiple values upward
```

---

# ⭐ Interview Trick

A quick way to identify Postorder DFS problems:

Ask yourself:

> **"Can I solve the current node without first solving its children?"**

- **Yes** → Consider **Preorder**.
- **No** → It's almost always **Postorder DFS**.

For this problem, the current node's average depends on the **sum** and **count** of its children, so the solution naturally becomes a **Postorder DFS** that returns `(sum, count)` from each subtree.

<br><br><br><br><br>

---

# <span style="color: limegreen;">1026. Maximum Difference Between Node and Ancestor</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, Recursion

---

# 📖 Problem Statement

Given the `root` of a binary tree, return the **maximum absolute difference** between the values of any **ancestor** node and its **descendant** node.

An ancestor of a node is any node on the path from the root to that node.

---

## Example 1

### Input

```text
            8
          /   \
         3     10
        / \      \
       1   6      14
          / \     /
         4   7   13
```

### Output

```text
7
```

### Explanation

Some ancestor-descendant differences are:

```text
|8 - 3| = 5
|3 - 7| = 4
|8 - 1| = 7
|10 - 13| = 3
```

The maximum difference is

```text
|8 - 1| = 7
```

---

## Example 2

### Input

```text
        1
         \
          2
         /
        0
       /
      3
```

### Output

```text
3
```

---

# 🤔 My Initial Approach

A natural idea is to keep all ancestor values in an array.

For every node:

```python
arr.append(root.val)

dfs(root.left)
dfs(root.right)

ans = max(ans, max(arr) - min(arr))

arr.pop()
```

The array always contains the path from the root to the current node.

Example:

```text
        8
       /
      3
     /
    1
```

At node `1`:

```python
arr = [8,3,1]
```

Maximum difference:

```text
max(arr) - min(arr)

=

8 - 1

=

7
```

This gives the correct answer.

---

# ❌ Drawback of This Approach

The problem is these two lines:

```python
max(arr)

min(arr)
```

They are **not O(1)**.

Both functions scan the entire array.

---

## Example

Suppose the tree is skewed.

```text
1
 \
  2
   \
    3
     \
      4
       \
        5
```

At node 5

```python
arr = [1,2,3,4,5]
```

Computing

```python
max(arr)
```

takes

```text
O(5)
```

Similarly

```python
min(arr)
```

also takes

```text
O(5)
```

This happens for **every node**.

---

# Complexity of My Approach

For every node

```text
max(arr) → O(h)

min(arr) → O(h)
```

where

```text
h = height of the tree
```

Total complexity

```text
O(n × h)
```

Worst case

```text
O(n²)
```

because

```text
h = n
```

for a skewed tree.

---

# 💡 Key Observation

Do we really need the **entire path**?

The answer is **No**.

To find the maximum difference with the current node, we only need:

- The **smallest ancestor value**
- The **largest ancestor value**

Nothing else matters.

Suppose the current path is

```text
8 → 3 → 6 → 4
```

The values are

```text
[8,3,6,4]
```

Current node = 4

Possible differences

```text
|8-4| = 4

|3-4| = 1

|6-4| = 2
```

The maximum difference is always obtained using

```text
Smallest ancestor

or

Largest ancestor
```

So instead of carrying

```python
[8,3,6,4]
```

we only carry

```python
minValue = 3

maxValue = 8
```

---

# Optimized Idea

During DFS, pass two values:

```python
currentMin

currentMax
```

Every time we visit a node

Update

```python
currentMin = min(currentMin, node.val)

currentMax = max(currentMax, node.val)
```

Then continue DFS.

---

# Dry Run

Tree

```text
        8
       /
      3
     /
    1
```

---

## Step 1

Start

```python
dfs(8,8,8)
```

Current

```text
Minimum = 8

Maximum = 8
```

---

## Step 2

Move to node 3

Update

```text
Minimum = min(8,3)

=

3
```

```text
Maximum = max(8,3)

=

8
```

Call

```python
dfs(3,3,8)
```

---

## Step 3

Move to node 1

Update

```text
Minimum = min(3,1)

=

1
```

```text
Maximum = max(8,1)

=

8
```

Call

```python
dfs(1,1,8)
```

Now

```text
Maximum Difference

=

8 - 1

=

7
```

---

# Another Example

```text
        8
       / \
      3   10
```

At node

```text
10
```

Current values

```text
Minimum = 8

Maximum = 10
```

Difference

```text
10 - 8

=

2
```

Global answer remains

```text
7
```

---

# Python Code

```python
class Solution:
    def maxAncestorDiff(self, root: Optional[TreeNode]) -> int:

        def dfs(node, minVal, maxVal):

            if node is None:
                return maxVal - minVal

            # Update the minimum and maximum values seen so far
            minVal = min(minVal, node.val)
            maxVal = max(maxVal, node.val)

            # Explore both subtrees
            left = dfs(node.left, minVal, maxVal)
            right = dfs(node.right, minVal, maxVal)

            # Return the best answer from either subtree
            return max(left, right)

        return dfs(root, root.val, root.val)
```

---

# Code Explanation

### Step 1

Start DFS from the root.

```python
dfs(root, root.val, root.val)
```

Initially

```text
Minimum = Root Value

Maximum = Root Value
```

---

### Step 2

Whenever we visit a node

Update

```python
minVal = min(minVal, node.val)

maxVal = max(maxVal, node.val)
```

These variables always represent

```text
Minimum ancestor value

Maximum ancestor value
```

along the current path.

---

### Step 3

Continue DFS

```python
left = dfs(node.left, minVal, maxVal)

right = dfs(node.right, minVal, maxVal)
```

Each recursive call gets its own updated minimum and maximum.

---

### Step 4

If we reach the end of a path

```python
if node is None:
    return maxVal - minVal
```

This path is complete.

The largest ancestor difference on this path is simply

```text
Maximum Value

-

Minimum Value
```

---

### Step 5

Return the best answer

```python
return max(left, right)
```

Among all paths, return the largest difference.

---

# Why Does This Work?

Suppose the current path is

```text
8 → 3 → 6 → 4
```

Instead of storing

```python
[8,3,6,4]
```

we only store

```python
Minimum = 3

Maximum = 8
```

For every new node, we only update these two values.

Since the maximum difference on a path is always between the smallest and largest values, storing the entire path is unnecessary.

---

# Complexity Analysis

## Time Complexity

Every node is visited exactly once.

```text
O(n)
```

---

## Space Complexity

The recursion stack stores one root-to-leaf path.

```text
O(h)
```

where

- `n` = number of nodes
- `h` = height of the tree

Worst case

```text
O(n)
```

for a skewed tree.

---

# Pattern Recognition

When solving tree problems, ask yourself:

> **Do I really need the entire path, or only some summary of it?**

Many problems only require carrying a few values during DFS.

| Problem | Information Carried During DFS |
|----------|-------------------------------|
| Path Sum | Current Sum |
| Maximum Ancestor Difference | Minimum & Maximum Values |
| Root to Leaf Numbers | Current Number |
| Even Grandparent | Parent & Grandparent |
| Pseudo-Palindromic Paths | Frequency / Bitmask |

Instead of storing the complete path, store only the information needed to compute the answer.

---

# ⭐ Key Takeaways

- My initial approach stores the entire root-to-node path.
- Finding `max(arr)` and `min(arr)` for every node increases the complexity to **O(n²)** in the worst case.
- The optimized solution observes that only the **minimum** and **maximum** values along the current path matter.
- By carrying `(minVal, maxVal)` during DFS, each node is processed exactly once.
- This reduces the time complexity to **O(n)** while using only **O(h)** recursion stack space.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">1315. Sum of Nodes with Even-Valued Grandparent</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, Recursion

---

# 📖 Problem Statement

Given the `root` of a binary tree, return the **sum of all nodes whose grandparent has an even value**.

A **grandparent** of a node is the parent of its parent.

If no node has an even-valued grandparent, return `0`.

---

## Example 1

### Input

```text
                6
             /     \
            7       8
          /  \     /  \
         2    7   1    3
        /    / \         \
       9    1   4         5
```

Output

```text
18
```

Explanation

The even-valued grandparents are:

```text
6

8
```

Nodes having **6** as grandparent

```text
2

7

1

3
```

Sum

```text
2 + 7 + 1 + 3 = 13
```

Nodes having **8** as grandparent

```text
5
```

Total

```text
13 + 5 = 18
```

---

## Example 2

Input

```text
1
```

Output

```text
0
```

---

# 💡 Two Different Ways of Thinking

There are two common DFS approaches.

Although both run in **O(n)** time, their thinking process is completely different.

---

# Approach 1: Look Down From Every Even Node (Your Solution)

## Main Idea

Instead of asking

> "Does this node have an even grandparent?"

you ask

> "Is this node even?"

If yes,

look exactly **two levels below** and add all grandchildren.

---

## Code

```python
class Solution:
    def sumEvenGrandparent(self, root: Optional[TreeNode]) -> int:

        ans = 0

        def traverse(root):
            nonlocal ans

            if not root:
                return

            if root.val % 2 == 0:

                if root.left:

                    if root.left.left:
                        ans += root.left.left.val

                    if root.left.right:
                        ans += root.left.right.val

                if root.right:

                    if root.right.left:
                        ans += root.right.left.val

                    if root.right.right:
                        ans += root.right.right.val

            traverse(root.left)
            traverse(root.right)

        traverse(root)

        return ans
```

---

# How This Thinks

Tree

```text
                6
             /     \
            7       8
          /  \     /  \
         2    7   1    3
```

DFS reaches

```text
6
```

Question:

```text
Is 6 even?

YES
```

Immediately inspect

```text
Left.Left

↓

2
```

```text
Left.Right

↓

7
```

```text
Right.Left

↓

1
```

```text
Right.Right

↓

3
```

Add

```text
2 + 7 + 1 + 3
```

Continue DFS.

---

# Visualization

```text
          6
       /      \
      7        8
     / \      / \
    2   7    1   3

When visiting 6

Look two levels below

↓

2

7

1

3
```

---

# Why It Works

Every time we encounter an even-valued node,

we directly add all of its grandchildren.

Since every node is visited exactly once,

no grandchild is counted multiple times.

---

# Complexity

Each node is visited once.

Each visit performs at most

```text
4 grandchild checks
```

which is constant work.

Therefore

```text
Time Complexity

O(n)
```

```text
Space Complexity

O(h)
```

where

```text
h = height of tree
```

---

# Drawback

Although efficient,

the code contains many pointer checks.

```python
root.left.left

root.left.right

root.right.left

root.right.right
```

This becomes harder to write and easier to make mistakes.

---

# Approach 2: Carry Parent & Grandparent During DFS

Instead of looking **down** two levels,

carry information **downward** while traversing.

---

# Main Idea

For every node,

already know

```text
Parent

Grandparent
```

When we reach a node,

simply ask

```text
Is my grandparent even?
```

If yes,

add myself.

---

# Meaning of Parameters

```python
dfs(node, parent, grandparent)
```

where

```text
node

Current node
```

```text
parent

Value of parent
```

```text
grandparent

Value of grandparent
```

---

# How Parameters Move

Suppose

```text
        6
       /
      7
     /
    2
```

Initially

```python
dfs(6,1,1)
```

Current

```text
Node = 6

Parent = 1

Grandparent = 1
```

---

Move Left

```python
dfs(7,6,1)
```

Current

```text
Node = 7

Parent = 6

Grandparent = 1
```

---

Move Left

```python
dfs(2,7,6)
```

Current

```text
Node = 2

Parent = 7

Grandparent = 6
```

Now

```python
6 % 2 == 0
```

True.

Add

```python
2
```

---

# Dry Run

Tree

```text
                6
             /     \
            7       8
          /  \     /  \
         2    7   1    3
```

---

Visit

```text
6
```

Grandparent

```text
1
```

Odd

Do nothing.

---

Visit

```text
7
```

Grandparent

```text
1
```

Odd

Do nothing.

---

Visit

```text
2
```

Grandparent

```text
6
```

Even

Add

```text
2
```

---

Visit

```text
7
```

Grandparent

```text
6
```

Even

Add

```text
7
```

---

Visit

```text
1
```

Grandparent

```text
6
```

Even

Add

```text
1
```

---

Visit

```text
3
```

Grandparent

```text
6
```

Even

Add

```text
3
```

---

Final Sum

```text
2 + 7 + 1 + 3

=

13
```

Later,

when DFS reaches

```text
8
```

its grandchild

```text
5
```

is also added.

Final answer

```text
18
```

---

# Code

```python
class Solution:
    def sumEvenGrandparent(self, root: Optional[TreeNode]):

        arr = []

        def dfs(node, parent, grandparent):

            if not node:
                return

            if grandparent % 2 == 0:
                arr.append(node.val)

            dfs(node.left, node.val, parent)
            dfs(node.right, node.val, parent)

        dfs(root, 1, 1)

        return sum(arr)
```

---

# Why Does This Work?

Instead of asking

```text
Is current node even?
```

we ask

```text
Does current node have an even grandparent?
```

which is exactly what the problem statement requires.

No need to manually inspect grandchildren.

---

# Even Better Solution

We don't even need an array.

Simply return the sum directly.

```python
class Solution:

    def sumEvenGrandparent(self, root):

        def dfs(node, parent, grandparent):

            if not node:
                return 0

            current = node.val if grandparent % 2 == 0 else 0

            return (
                current
                + dfs(node.left, node.val, parent)
                + dfs(node.right, node.val, parent)
            )

        return dfs(root, 1, 1)
```

This avoids storing extra values.

---

# Comparing Both Approaches

| Your Approach | Parent & Grandparent DFS |
|---------------|--------------------------|
| Check whether current node is even | Check whether current node has an even grandparent |
| Look down two levels | Carry information downward |
| Manually inspect grandchildren | No pointer manipulation |
| More pointer checks | Cleaner recursion |
| Very intuitive | More reusable DFS pattern |
| Time: **O(n)** | Time: **O(n)** |
| Space: **O(h)** | Space: **O(h)** |

---

# Which One Is Better?

### Your Approach

✅ Very intuitive

✅ Easy to understand

❌ Requires many pointer checks

```python
left.left

left.right

right.left

right.right
```

---

### Parent & Grandparent DFS

✅ Cleaner recursion

✅ Easier to generalize

✅ Common interview pattern

This is usually the preferred interview solution.

---

# Pattern Recognition

Whenever a tree problem mentions

- Parent
- Grandparent
- Ancestor
- Distance K from Ancestor

Think

```text
Carry information downward during DFS
```

instead of repeatedly looking upward or manually searching descendants.

---

# ⭐ Key Takeaways

- There are two valid DFS approaches to this problem.
- Your approach starts from **even-valued grandparents** and explicitly visits their grandchildren.
- The optimized approach starts from the **current node** and carries its **parent** and **grandparent** values during recursion.
- Both solutions have **O(n)** time complexity and **O(h)** recursion stack space.
- The parent/grandparent approach is cleaner, avoids repeated pointer checks, and demonstrates a reusable DFS pattern for many tree problems.

<br/><br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">979. Distribute Coins in Binary Tree</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, Postorder Traversal

---

# 📖 Problem Statement

You are given the root of a binary tree containing **n nodes**.

Each node has `node.val` coins, and the total number of coins in the tree is exactly **n**.

In one move, you can move **one coin** between two adjacent nodes (parent ↔ child).

Your goal is to make **every node contain exactly one coin**.

Return the **minimum number of moves** required.

---

## Example 1

### Input

```text
      3
     / \
    0   0
```

Output

```text
2
```

Explanation

```text
Move one coin to the left child.

Move one coin to the right child.
```

---

## Example 2

### Input

```text
      0
     / \
    3   0
```

Output

```text
3
```

Explanation

```text
Move one coin from left child → root.

Move another coin from left child → root.

Move one coin from root → right child.
```

Total

```text
3 moves
```

---

# 🤔 First Thoughts

Most people initially think:

```text
Should I simulate every move?

Should I use BFS?

Should I move coins one by one?

How do I find the shortest path?
```

Unfortunately, all of these approaches become complicated.

Instead...

**Forget about the moves for a moment.**

---

# 💡 The Key Question

Instead of asking:

> **"How do I move coins?"**

Ask:

> **"What information does a parent need from its children?"**

Imagine a subtree.

```text
        ?
       / \
      ?   ?
```

When this subtree is completely fixed,

what should it tell its parent?

It only needs to say one thing:

```text
I have extra coins.

OR

I need coins.
```

Nothing else matters.

---

# The Important Observation

Every node must finally keep exactly

```text
1 coin
```

Therefore,

if a subtree has

```text
More than needed
```

it sends coins upward.

If it has

```text
Less than needed
```

it asks its parent for coins.

---

# Defining Balance

For every subtree,

compute

```text
Balance

=

Coins

-

Nodes
```

or equivalently

```text
Subtree Coins

-

Subtree Nodes
```

Interpretation

```text
Positive

↓

Extra coins to send upward.
```

```text
Negative

↓

Coins needed from parent.
```

---

# Example 1

Leaf

```text
3 coins
```

```text
Coins

=

3
```

```text
Nodes

=

1
```

Balance

```text
3 - 1

=

2
```

Meaning

```text
Keep 1 coin.

Send 2 coins upward.
```

---

Another leaf

```text
0 coins
```

Coins

```text
0
```

Nodes

```text
1
```

Balance

```text
0 - 1

=

-1
```

Meaning

```text
Need 1 coin from parent.
```

---

# What Should DFS Return?

The parent only needs one value.

Return

```text
Balance
```

which is

```python
balance = subtree_coins - subtree_nodes
```

or

```python
balance = node.val + left + right - 1
```

---

# Why Postorder DFS?

A parent cannot know its balance until it knows

- Left balance
- Right balance

Therefore

```text
Left

↓

Right

↓

Node
```

This is exactly

```text
Postorder DFS
```

---

# Example 1 Dry Run

Tree

```text
      3
     / \
    0   0
```

Expected Answer

```text
2
```

---

## Visit Left Leaf

Coins

```text
0
```

Nodes

```text
1
```

Balance

```text
-1
```

Meaning

```text
Need 1 coin.
```

Return

```text
-1
```

---

## Visit Right Leaf

Same calculation.

Return

```text
-1
```

---

## Visit Root

Current value

```text
3
```

Left Balance

```text
-1
```

Right Balance

```text
-1
```

Balance

```text
3

+

(-1)

+

(-1)

-

1

=

0
```

Meaning

```text
Root keeps exactly one coin.
```

Perfect.

---

# Where Do the Moves Come From?

Left child needed

```text
1 coin
```

That means

```text
1 coin crossed the edge.
```

Moves

```text
1
```

Right child also needed

```text
1 coin
```

Moves

```text
1
```

Total

```text
2
```

---

# Why Absolute Value?

Suppose

```text
Balance = +4
```

Meaning

```text
4 coins move upward.
```

Moves

```text
4
```

---

Suppose

```text
Balance = -4
```

Meaning

```text
Need 4 coins from parent.
```

Moves

```text
4
```

Direction doesn't matter.

Only the number of coins crossing the edge matters.

Therefore

```python
moves += abs(balance)
```

---

# Example 2 Dry Run

Tree

```text
      0
     / \
    3   0
```

---

## Left Child

Coins

```text
3
```

Nodes

```text
1
```

Balance

```text
2
```

Meaning

```text
Send 2 coins upward.
```

Moves

```text
2
```

---

## Right Child

Balance

```text
-1
```

Meaning

```text
Need 1 coin.
```

Moves

```text
1
```

---

## Root

Current

```text
0
```

Left

```text
2
```

Right

```text
-1
```

Balance

```text
0

+

2

+

(-1)

-

1

=

0
```

Everything is balanced.

Total moves

```text
2 + 1

=

3
```

Correct.

---

# Mathematical Formula

Suppose

```text
L = Balance from Left

R = Balance from Right
```

Current node has

```text
node.val
```

Then

```python
balance = node.val + L + R - 1
```

Why subtract **1**?

Because

```text
Every node must keep one coin for itself.
```

Anything extra goes upward.

Anything missing comes from parent.

---

# Python Code

```python
class Solution:
    def distributeCoins(self, root: Optional[TreeNode]) -> int:

        moves = 0

        def dfs(node):
            nonlocal moves

            if not node:
                return 0

            # Balance from left subtree
            left = dfs(node.left)

            # Balance from right subtree
            right = dfs(node.right)

            # Coins crossing the left and right edges
            moves += abs(left) + abs(right)

            # Return balance to parent
            return node.val + left + right - 1

        dfs(root)

        return moves
```

---

# Code Explanation

## Step 1

Visit children first.

```python
left = dfs(node.left)

right = dfs(node.right)
```

Each child returns

```text
Extra coins

or

Needed coins
```

---

## Step 2

Count moves.

```python
moves += abs(left) + abs(right)
```

Every coin crossing an edge counts as one move.

Positive balance

```text
Coins move upward.
```

Negative balance

```text
Coins move downward.
```

Both require

```text
abs(balance)
```

moves.

---

## Step 3

Return balance.

```python
return node.val + left + right - 1
```

Current node

- Receives extra coins from children.
- Gives coins to children if needed.
- Keeps exactly **1 coin**.
- Sends remaining balance to parent.

---

# Why Does This Work?

Imagine this subtree.

```text
        ?
       / \
      ?   ?
```

The parent doesn't care:

- How children arranged the coins.
- How many moves happened inside the subtree.

The parent only needs to know:

```text
How many extra coins do you have?

OR

How many coins do you still need?
```

That single number is the **balance**.

Once we realize this,

the problem becomes a simple Postorder DFS.

---

# Complexity Analysis

## Time Complexity

Every node is visited exactly once.

```text
O(n)
```

---

## Space Complexity

Recursion stack

```text
O(h)
```

where

- `n` = number of nodes
- `h` = height of the tree

Worst case

```text
O(n)
```

for a skewed tree.

---

# Pattern Recognition

This problem belongs to an important category:

```text
Subtree computes information

↓

Returns it to parent

↓

Parent combines results
```

Examples

| Problem | DFS Returns |
|----------|-------------|
| Average of Subtree | (Sum, Count) |
| Diameter of Binary Tree | Height |
| Maximum Path Sum | Best Downward Path |
| Distribute Coins | Balance |
| Binary Tree Cameras | State |
| Longest Univalue Path | Path Length |

---

# ⭐ Interview Trick

Whenever solving a tree problem, ask:

> **"What information does the parent need from its children?"**

For this problem, the answer is simply:

```text
How many extra coins do you have?

OR

How many coins do you need?
```

That quantity is called the **balance**.

```text
Balance

=

Coins

-

Nodes
```

Once you identify the correct value to return from DFS, the entire solution naturally becomes a **Postorder DFS**.

---

# 📝 Key Takeaways

- Do **not** simulate every coin movement.
- Think in terms of **subtree balance** instead of individual moves.
- Each subtree returns only one value: **balance**.
- Positive balance means **send coins upward**.
- Negative balance means **request coins from the parent**.
- The number of moves across an edge is always `abs(balance)`.
- The solution uses **Postorder DFS** because a parent must first know the balances of its children.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">1022. Sum of Root To Leaf Binary Numbers</span>

- **Difficulty:** Easy
- **Topics:** Binary Tree, DFS, Recursion

---

# 📖 Problem Statement

You are given the `root` of a binary tree where every node contains either **0** or **1**.

Each **root-to-leaf** path represents a **binary number**, where the root is the **most significant bit (MSB)**.

Return the **sum of all binary numbers** represented by every root-to-leaf path.

---

## Example 1

### Input

```text
        1
      /   \
     0     1
    / \   / \
   0   1 0   1
```

### Output

```text
22
```

### Explanation

Root-to-leaf paths are

```text
1 → 0 → 0 = 100₂ = 4

1 → 0 → 1 = 101₂ = 5

1 → 1 → 0 = 110₂ = 6

1 → 1 → 1 = 111₂ = 7
```

Total

```text
4 + 5 + 6 + 7 = 22
```

---

## Example 2

Input

```text
0
```

Output

```text
0
```

---

# 🤔 The Main Difficulty

Many people don't struggle with the code.

They struggle with questions like:

- How does recursion know the current binary number?
- Why don't we store the entire path?
- What should be passed into DFS?

The answer comes from asking **one important question**.

---

# 💡 The Most Important Question

Whenever you solve a tree problem, ask:

> **What information should travel from parent to child?**

or

> **What information should travel from child to parent?**

This problem belongs to

```text
Parent

↓

Child
```

information flow.

---

# Step 1: Understand What the Path Represents

Suppose the tree is

```text
    1
   /
  0
 /
1
```

The path is

```text
1 → 0 → 1
```

Binary

```text
101₂
```

Decimal

```text
5
```

As we move downward,

we are slowly building a binary number.

---

# Step 2: How Do We Build a Binary Number?

Suppose we already have

```text
1
```

Now we visit

```text
0
```

Binary becomes

```text
10
```

How did we get there?

Take the current number

```text
1
```

Shift left

```text
1 << 1

=

10₂
```

Then insert the current bit

```text
10 + 0

=

10₂
```

---

Another step

Current

```text
10₂
```

Visit

```text
1
```

Shift

```text
100₂
```

Add

```text
101₂
```

Finished.

---

# Formula

Every time we visit a node

```python
current = current * 2 + node.val
```

or equivalently

```python
current = (current << 1) | node.val
```

Both produce the same result.

---

# Why Does Multiplying by 2 Work?

Remember decimal numbers.

Appending a digit

```text
23

↓

Append 5

↓

235
```

Formula

```text
23 × 10 + 5
```

Binary works exactly the same way.

Appending one binary digit

```text
101

↓

Append 1

↓

1011
```

Formula

```text
101 × 2 + 1
```

That's why we write

```python
current = current * 2 + node.val
```

---

# Step 3: What Should DFS Carry?

Do we need to store the entire path?

Example

```text
1 → 0 → 1
```

Do we need

```python
[1,0,1]
```

No.

The only thing we need is

```text
The binary number built so far.
```

Therefore,

DFS becomes

```python
dfs(node, currentBinary)
```

---

# Step 4: When Do We Add to the Answer?

Only at a leaf.

Why?

Because only a

```text
Root

↓

Leaf
```

forms a complete binary number.

A partial path

```text
Root

↓

Internal Node
```

is incomplete.

So

```python
if leaf:
    return currentBinary
```

---

# Dry Run

Tree

```text
        1
      /   \
     0     1
    / \   / \
   0   1 0   1
```

---

## Start

```python
dfs(root, 0)
```

Current

```text
0
```

---

## Visit Root (1)

Formula

```text
0 × 2 + 1

=

1
```

Current Binary

```text
1
```

---

## Left Child (0)

Formula

```text
1 × 2 + 0

=

2
```

Binary

```text
10₂
```

---

## Left Child (0)

Formula

```text
2 × 2 + 0

=

4
```

Binary

```text
100₂
```

Leaf reached.

Return

```text
4
```

---

## Right Child (1)

Backtrack.

Formula

```text
2 × 2 + 1

=

5
```

Binary

```text
101₂
```

Return

```text
5
```

---

## Right Subtree

Current

```text
1
```

Visit

```text
1
```

Formula

```text
1 × 2 + 1

=

3
```

Binary

```text
11₂
```

---

Left Child

```text
3 × 2 + 0

=

6
```

Return

```text
6
```

---

Right Child

```text
3 × 2 + 1

=

7
```

Return

```text
7
```

---

Final Answer

```text
4 + 5 + 6 + 7

=

22
```

---

# Recursion Flow

```text
                 dfs(1,0)

                     │

            current = 1

          /                 \

dfs(0,1)                 dfs(1,1)

current=2               current=3

 /      \               /      \

4        5             6        7

Return   Return        Return   Return

      \      |          |      /

        4 + 5 + 6 + 7

               =

              22
```

Notice something important.

Each child receives

```text
The binary number built by its parent.
```

The child extends it by one more bit.

---

# Python Code

```python
class Solution:
    def sumRootToLeaf(self, root: Optional[TreeNode]) -> int:

        def dfs(node, current):

            if not node:
                return 0

            # Build the binary number
            current = current * 2 + node.val

            # If this is a leaf, return the completed number
            if not node.left and not node.right:
                return current

            # Sum from both subtrees
            return (
                dfs(node.left, current)
                + dfs(node.right, current)
            )

        return dfs(root, 0)
```

---

# Code Explanation

## Step 1

Start DFS

```python
dfs(root, 0)
```

Initially,

the binary number is

```text
0
```

---

## Step 2

Update the current binary number

```python
current = current * 2 + node.val
```

This appends the current bit.

---

## Step 3

If we reach a leaf

```python
return current
```

because one complete binary number has been formed.

---

## Step 4

Otherwise,

continue exploring

```python
dfs(node.left, current)

dfs(node.right, current)
```

Each child receives the updated binary number.

---

## Step 5

Add answers

```python
left + right
```

Every root-to-leaf path contributes one number.

---

# Why Does This Work?

Imagine the path

```text
1 → 0 → 1
```

The recursion carries

```text
1

↓

10

↓

101
```

Instead of storing

```python
[1,0,1]
```

we store

```text
101₂
```

which is enough.

Every child simply extends its parent's number.

---

# Complexity Analysis

## Time Complexity

Every node is visited once.

```text
O(n)
```

---

## Space Complexity

The recursion stack stores one root-to-leaf path.

```text
O(h)
```

where

- `n` = number of nodes
- `h` = height of the tree

Worst case

```text
O(n)
```

for a skewed tree.

---

# Recursion Pattern

A very useful way to classify tree problems is by asking

> **Where does the information flow?**

---

## Category 1: Parent → Child

The parent gives information to its children.

Pattern

```python
dfs(node, state)
```

Examples

| Problem | State Passed Down |
|----------|-------------------|
| Path Sum | Current Sum |
| Root to Leaf Numbers | Current Number |
| Sum of Binary Numbers | Current Binary |
| Maximum Ancestor Difference | Min & Max Values |
| Even-Valued Grandparent | Parent & Grandparent |

---

## Category 2: Child → Parent

Children compute information and return it upward.

Pattern

```python
left = dfs(node.left)

right = dfs(node.right)

return something
```

Examples

| Problem | Information Returned |
|----------|----------------------|
| Diameter of Binary Tree | Height |
| Average of Subtree | (Sum, Count) |
| Distribute Coins | Balance |
| Maximum Path Sum | Best Downward Path |
| Binary Tree Cameras | State |

---

# ⭐ Interview Trick

Whenever solving a tree problem, ask yourself:

> **"What does a child need from its parent?"**

For this problem,

the answer is

```text
The binary number built so far.
```

Therefore,

the recursion naturally becomes

```python
dfs(node, currentBinary)
```

Every child extends the binary number,

and every leaf returns one complete decimal value.

---

# 📝 Key Takeaways

- Do **not** store the entire root-to-leaf path.
- Carry only the **binary number built so far**.
- Update it using:

```python
current = current * 2 + node.val
```

- Only leaf nodes produce complete binary numbers.
- This is a classic **Parent → Child DFS** problem where information flows downward through recursion.

<br><br><br><br><br>