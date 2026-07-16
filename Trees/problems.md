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
- [2641. Cousins in Binary Tree II](#2641-cousins-in-binary-tree-ii)
- [1448. Count Good Nodes in Binary Tree](#1448-count-good-nodes-in-binary-tree)
- [513. Find Bottom Left Tree Value](#513-find-bottom-left-tree-value)
- [1110. Delete Nodes And Return Forest](#1110-delete-nodes-and-return-forest)
- [114. Flatten Binary Tree to Linked List](#114-flatten-binary-tree-to-linked-list)
- [872. Leaf-Similar Trees](#872-leaf-similar-trees)
- [129. Sum Root to Leaf Numbers](#129-sum-root-to-leaf-numbers)
- [257. Binary Tree Paths](#257-binary-tree-paths)
- [107. Binary Tree Level Order Traversal II](#107-binary-tree-level-order-traversal-ii)
- [437. Path Sum III](#437-path-sum-iii)
- [95. Unique Binary Search Trees II](#95-unique-binary-search-trees-ii)

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

<br/><br/><br/><br/><br/>

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

---

# <span style="color: limegreen;">2641. Cousins in Binary Tree II</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, BFS, Level-wise Processing

---

# 📖 Problem Statement

You are given the root of a binary tree.

Replace the value of every node with the **sum of all its cousins' values**.

Two nodes are cousins if:

- They are at the **same depth**
- They have **different parents**

Return the modified tree.

---

## Example 1

### Input

```text
          5
        /   \
       4     9
      / \     \
     1  10     7
```

### Output

```text
          0
        /   \
       0     0
      / \     \
     7   7    11
```

---

## Example 2

### Input

```text
      3
     / \
    1   2
```

### Output

```text
      0
     / \
    0   0
```

---

# 🤔 Why Is This Problem Difficult?

Most people read

```text
Find cousins
```

and immediately think

```text
For every node,

↓

Find all cousins

↓

Add them
```

This becomes

```text
O(n²)
```

and is extremely complicated.

The trick is to **change the way you think**.

---

# Step 1: Understand What Cousins Are

Tree

```text
          5
        /   \
       4     9
      / \     \
     1  10     7
```

Depths

```text
Depth 0

5
```

```text
Depth 1

4

9
```

```text
Depth 2

1

10

7
```

At depth 2

```text
1 and 10
```

share the same parent

```text
4
```

So they are

```text
Siblings
```

NOT cousins.

---

Node

```text
1
```

Cousins

```text
7
```

because

```text
Same depth

Different parent
```

Therefore

```text
New Value of 1

=

7
```

---

Node

```text
10
```

Cousins

```text
7
```

New value

```text
7
```

---

Node

```text
7
```

Cousins

```text
1 + 10

=

11
```

---

# Step 2: Stop Thinking About Cousins

This is the most important step.

Instead of asking

```text
How do I find cousins?
```

ask

```text
What information does this node actually need?
```

Take node

```text
1
```

It needs

```text
All nodes at the same level

minus

Nodes having the same parent
```

Interesting...

---

# Step 3: Discover the Formula

Look at depth 2

```text
1

10

7
```

Total Level Sum

```text
1 + 10 + 7

=

18
```

Sibling group of node 1

```text
1 + 10

=

11
```

Therefore

```text
Cousin Sum

=

18 - 11

=

7
```

Exactly the answer.

---

Node

```text
7
```

Level Sum

```text
18
```

Sibling Sum

```text
7
```

Answer

```text
18 - 7

=

11
```

Again correct.

---

# ⭐ The Big Observation

We never actually need to find cousins.

We only need

```text
Level Sum

-

Sibling Sum
```

This is the key insight.

---

# Step 4: What Information Should We Precompute?

For every depth,

store

```text
Depth

↓

Sum of all nodes at that depth
```

Example

```text
Depth 0

5
```

```text
Depth 1

4 + 9

=

13
```

```text
Depth 2

1 + 10 + 7

=

18
```

Store

```python
levelSum = {

0 : 5,

1 : 13,

2 : 18

}
```

---

# Step 5: First DFS

Purpose

```text
Compute

Depth

↓

Level Sum
```

DFS

```python
dfs(node, depth)
```

---

Visit

```text
5
```

```python
levelSum[0] += 5
```

---

Visit

```text
4
```

```python
levelSum[1] += 4
```

---

Visit

```text
9
```

```python
levelSum[1] += 9
```

Continue...

Finally

```python
{

0 : 5,

1 : 13,

2 : 18

}
```

---

# Step 6: Second DFS

Now we already know

```text
Every level's total sum.
```

For each parent,

compute

```text
Sum of its children
```

Example

```text
      4
     / \
    1  10
```

Children Sum

```text
11
```

Level Sum

```text
18
```

New values

```text
1

↓

18 - 11

=

7
```

```text
10

↓

18 - 11

=

7
```

---

Another parent

```text
9
 \
 7
```

Children Sum

```text
7
```

Level Sum

```text
18
```

New value

```text
18 - 7

=

11
```

Done.

---

# Complete Dry Run

Original Tree

```text
          5
        /   \
       4     9
      / \     \
     1  10     7
```

---

## First DFS

Compute

```python
{

0 : 5,

1 : 13,

2 : 18

}
```

---

## Root

Root has no cousins.

Set

```text
0
```

Tree

```text
          0
        /   \
       4     9
      / \     \
     1  10     7
```

---

## Children of Root

Children Sum

```text
4 + 9

=

13
```

Level Sum

```text
13
```

New values

```text
4

↓

13 - 13

=

0
```

```text
9

↓

13 - 13

=

0
```

Tree

```text
          0
        /   \
       0     0
      / \     \
     1  10     7
```

---

## Children of Node 4

Children Sum

```text
1 + 10

=

11
```

Level Sum

```text
18
```

New values

```text
1

↓

18 - 11

=

7
```

```text
10

↓

18 - 11

=

7
```

---

## Children of Node 9

Children Sum

```text
7
```

Level Sum

```text
18
```

New value

```text
18 - 7

=

11
```

Final Tree

```text
          0
        /   \
       0     0
      / \     \
     7   7    11
```

Correct.

---

# Python Code

```python
from collections import defaultdict

class Solution:

    def replaceValueInTree(self, root):

        levelSum = defaultdict(int)

        # First DFS
        # Compute sum of every level
        def dfs1(node, depth):

            if not node:
                return

            levelSum[depth] += node.val

            dfs1(node.left, depth + 1)
            dfs1(node.right, depth + 1)

        dfs1(root, 0)

        # Root has no cousins
        root.val = 0

        # Second DFS
        def dfs2(node, depth):

            if not node:
                return

            childSum = 0

            if node.left:
                childSum += node.left.val

            if node.right:
                childSum += node.right.val

            nextLevelSum = levelSum[depth + 1]

            if node.left:
                node.left.val = nextLevelSum - childSum

            if node.right:
                node.right.val = nextLevelSum - childSum

            dfs2(node.left, depth + 1)
            dfs2(node.right, depth + 1)

        dfs2(root, 0)

        return root
```

---

# Code Explanation

## First DFS

Collect

```text
Depth

↓

Sum of nodes at that depth
```

Store in

```python
levelSum
```

---

## Second DFS

For every parent

Compute

```text
Children Sum
```

Then

```python
New Child Value

=

Level Sum

-

Children Sum
```

This removes siblings

and keeps only cousins.

---

# Why Does This Work?

Suppose

```text
Depth 2

↓

1

10

7
```

Total

```text
18
```

Node

```text
1
```

cannot count

```text
1

10
```

because they have the same parent.

Subtract

```text
11
```

Remaining

```text
7
```

Exactly the cousin sum.

---

# Complexity Analysis

## Time Complexity

First DFS

```text
O(n)
```

Second DFS

```text
O(n)
```

Total

```text
O(n)
```

---

## Space Complexity

HashMap

```text
O(h)
```

Recursion Stack

```text
O(h)
```

Overall

```text
O(n)
```

in the worst case.

---

# 🧠 How to Think Like This

Whenever a tree problem mentions

- Same Level
- Same Depth
- Cousins
- Nodes in the same row

Ask yourself

```text
Can I precompute something level-wise?
```

Usually

```text
YES
```

Instead of searching for cousins,

search for

```text
Level Total
```

Then remove

```text
Nodes that should not be counted.
```

---

# General Mental Model

Instead of

```text
Find cousins
```

Transform it into

```text
All nodes at my level

↓

Remove my sibling group

↓

Remaining nodes are cousins.
```

This simple mathematical transformation turns a difficult graph-like problem into an easy DFS problem.

---

# ⭐ Interview Trick

Whenever you see

```text
Same Depth
```

think

```text
Level Sum
```

Whenever you see

```text
Exclude siblings
```

think

```text
Subtract Children Sum
```

So the final formula becomes

```text
Cousin Sum

=

Level Sum

-

Sibling Sum
```

Once you discover this equation, the entire solution naturally becomes a **two-pass DFS**.

---

# 📝 Key Takeaways

- Never try to explicitly find cousins.
- First compute the **sum of every level**.
- For each parent, compute the **sum of its children**.
- Each child's new value is:

```text
New Value

=

Level Sum

-

Sibling Sum
```

- The solution requires **two DFS traversals**:
  1. Compute `levelSum`.
  2. Update node values using `levelSum - childSum`.
- The hardest part of the problem is recognizing the formula:

```text
Cousin Sum = Level Sum - Sibling Sum
```

Once you identify this relationship, the implementation becomes straightforward.

<br><br><br><br><br>

---

# <span style="color: limegreen;">1448. Count Good Nodes in Binary Tree</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, Recursion

---

# 📖 Problem Statement

Given the `root` of a binary tree, a node **X** is called **good** if, in the path from the root to **X**, there is **no node with a value greater than X**.

Return the total number of **good nodes** in the tree.

---

## Example 1

### Input

```text
        3
       / \
      1   4
     /   / \
    3   1   5
```

### Output

```text
4
```

### Explanation

Good nodes are:

```text
3 (Root)

4

5

3 (Left Leaf)
```

---

## Example 2

### Input

```text
        3
       /
      3
     / \
    4   2
```

### Output

```text
3
```

---

## Example 3

### Input

```text
1
```

### Output

```text
1
```

---

# 💡 Key Observation

For every node, we only need to know:

```text
What is the maximum value seen from the root to this node?
```

If the current node's value is

```text
Greater than or equal to

Maximum seen so far
```

then the node is **good**.

---

# Parent → Child Information Flow

This is a **Parent → Child** DFS problem.

The parent passes one piece of information to its children:

```text
Maximum value seen so far
```

Each child updates it if necessary and passes it further.

---

# My Initial Solution

## Idea

Carry the maximum value (`maxi`) from the root to the current node.

If the current node's value is greater than or equal to `maxi`:

- Count it as a good node.
- Update `maxi`.

Then continue DFS.

---

## Code

```python
class Solution:
    def goodNodes(self, root: TreeNode) -> int:

        maxi = root.val
        ans = 0

        def dfs(root, maxi):
            nonlocal ans

            if not root:
                return

            if maxi <= root.val:
                maxi = root.val
                ans += 1

            dfs(root.left, maxi)
            dfs(root.right, maxi)

        dfs(root, maxi)

        return ans
```

---

# Dry Run

Tree

```text
        3
       / \
      1   4
     /   / \
    3   1   5
```

---

## Start

```python
dfs(3,3)
```

Current Maximum

```text
3
```

Current Node

```text
3
```

Since

```text
3 >= 3
```

Good Node ✅

Answer

```text
1
```

Pass

```text
max = 3
```

to children.

---

## Visit Node 1

Maximum

```text
3
```

Current

```text
1
```

```text
1 < 3
```

Not Good.

Pass

```text
3
```

---

## Visit Node 3

Maximum

```text
3
```

Current

```text
3
```

```text
3 >= 3
```

Good Node ✅

Answer

```text
2
```

---

## Visit Node 4

Maximum

```text
3
```

Current

```text
4
```

```text
4 >= 3
```

Good Node ✅

Update Maximum

```text
4
```

Answer

```text
3
```

---

## Visit Node 1

Maximum

```text
4
```

Current

```text
1
```

Not Good.

---

## Visit Node 5

Maximum

```text
4
```

Current

```text
5
```

```text
5 >= 4
```

Good Node ✅

Answer

```text
4
```

---

Final Answer

```text
4
```

---

# Optimized Version

The previous solution is already optimal in terms of time and space complexity.

A slightly cleaner implementation is to update the maximum using Python's `max()` function and start DFS directly with `root.val`.

## Code

```python
class Solution:
    def goodNodes(self, root: TreeNode) -> int:

        ans = 0

        def dfs(node, maxi):
            nonlocal ans

            if not node:
                return

            if node.val >= maxi:
                ans += 1

            maxi = max(maxi, node.val)

            dfs(node.left, maxi)
            dfs(node.right, maxi)

        dfs(root, root.val)

        return ans
```

---

# Why Does This Work?

At every node, we only need one piece of information:

```text
Largest value seen from the root to the current node.
```

We do **not** need:

- The complete path
- An array of ancestors
- Any extra data structure

The current node simply compares itself with the maximum value received from its parent.

If it is greater than or equal to that maximum:

```text
Current Node is Good
```

Then update the maximum and continue DFS.

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

This problem belongs to the **Parent → Child Information Flow** category.

The parent passes:

```text
Maximum value seen so far
```

The child:

1. Compares itself with that maximum.
2. Updates the maximum if needed.
3. Passes the updated value to its children.

Examples of similar problems:

| Problem | Information Passed Down |
|----------|-------------------------|
| Path Sum | Current Sum |
| Root to Leaf Binary Numbers | Current Binary Number |
| Maximum Difference Between Node and Ancestor | Minimum & Maximum Values |
| Count Good Nodes | Maximum Value |
| Sum of Nodes with Even-Valued Grandparent | Parent & Grandparent |

---

# 📝 Key Takeaways

- We only need to keep track of the **maximum value** along the current root-to-node path.
- If the current node's value is greater than or equal to this maximum, it is a **good node**.
- This is a classic **Parent → Child DFS** problem where the state (`maxi`) is passed downward.
- No additional arrays or ancestor lists are required.
- The solution runs in **O(n)** time with **O(h)** recursion stack space.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">513. Find Bottom Left Tree Value</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, BFS

---

# 📖 Problem Statement

Given the `root` of a binary tree, return the **leftmost value in the last row** of the tree.

---

## Example 1

### Input

```text
    2
   / \
  1   3
```

### Output

```text
1
```

---

## Example 2

### Input

```text
          1
        /   \
       2     3
      /     / \
     4     5   6
          /
         7
```

### Output

```text
7
```

---

# 💡 Key Observation

We need to find

1. The **deepest level** in the tree.
2. The **leftmost node** at that deepest level.

Since DFS visits the **left subtree before the right subtree**, the **first node** encountered at a new maximum depth will always be the leftmost node at that depth.

---

# My Initial Solution

## Idea

Instead of processing actual nodes, update the answer when recursion reaches a `None` child.

When recursion reaches beyond a leaf, use the parent node to determine the deepest leaf encountered.

---

## Code

```python
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:

        maxi = 0
        ans = root.val

        def dfs(root, parent, level):
            nonlocal maxi, ans

            if not root:

                if maxi < level - 1:
                    maxi = level - 1
                    ans = parent.val

                return

            dfs(root.left, root, level + 1)
            dfs(root.right, root, level + 1)

        dfs(root, 0, 0)

        return ans
```

---

# How This Solution Works

Tree

```text
        1
       / \
      2   3
     /
    4
```

DFS Order

```text
1

↓

2

↓

4

↓

None
```

When recursion reaches

```text
None
```

its parent is

```text
4
```

Since

```text
Depth = 2
```

is the deepest seen so far,

update

```text
Answer = 4
```

Because DFS always explores the left subtree first, the first deepest node encountered is automatically the leftmost one.

---

# Dry Run

Tree

```text
        1
       / \
      2   3
     /
    4
```

---

Visit

```text
1
```

Continue left.

---

Visit

```text
2
```

Continue left.

---

Visit

```text
4
```

Continue left.

---

Reach

```text
None
```

Parent

```text
4
```

Depth

```text
2
```

Update

```text
Answer = 4
```

Backtrack.

The remaining nodes are not deeper, so the answer remains

```text
4
```

---

# Optimized DFS Solution

A cleaner approach is to process the **current node itself** instead of waiting until reaching `None`.

Whenever we visit a node,

check if it is the deepest node seen so far.

If yes,

update the answer immediately.

---

## Code

```python
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:

        ans = root.val
        maxDepth = -1

        def dfs(node, depth):
            nonlocal ans, maxDepth

            if not node:
                return

            # First node visited at a new depth
            if depth > maxDepth:
                maxDepth = depth
                ans = node.val

            dfs(node.left, depth + 1)
            dfs(node.right, depth + 1)

        dfs(root, 0)

        return ans
```

---

# Why Does This Work?

Since DFS always visits

```text
Left

↓

Right
```

the first node encountered at every new depth is always the **leftmost node** of that level.

Example

```text
        1
       / \
      2   3
     /     \
    4       5
```

DFS visits

```text
1

↓

2

↓

4

↓

3

↓

5
```

When depth `2` is reached for the first time,

the node is

```text
4
```

Later,

node `5` is also at depth `2`, but

```python
depth == maxDepth
```

not

```python
depth > maxDepth
```

so the answer is not updated.

---

# BFS Solution

This problem can also be solved using **Level Order Traversal (BFS)**.

At every level,

the **first node** in the queue is the leftmost node of that level.

The first node of the **last level** is the required answer.

---

## Code

```python
from collections import deque

class Solution:
    def findBottomLeftValue(self, root):

        q = deque([root])

        while q:

            size = len(q)

            # First node of current level
            ans = q[0].val

            for _ in range(size):

                node = q.popleft()

                if node.left:
                    q.append(node.left)

                if node.right:
                    q.append(node.right)

        return ans
```

---

# Comparing the Approaches

| Approach | Idea |
|----------|------|
| Initial DFS | Update answer when recursion reaches `None` using the parent node |
| Optimized DFS | Update answer immediately when visiting a deeper node |
| BFS | Track the first node of every level and return the first node of the last level |

---

# Complexity Analysis

## DFS

### Time Complexity

```text
O(n)
```

Every node is visited once.

### Space Complexity

```text
O(h)
```

where `h` is the height of the tree.

---

## BFS

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(w)
```

where `w` is the maximum width of the tree.

---

# Pattern Recognition

This problem is based on **Depth Tracking**.

The DFS state carries

```text
Current Depth
```

At every node,

compare the current depth with the maximum depth seen so far.

If a deeper level is found,

update the answer.

The BFS solution is based on **Level Order Traversal**, where the first node of the final level is the required answer.

---

# 📝 Key Takeaways

- The goal is to find the **leftmost node at the deepest level**.
- DFS naturally works because it explores the **left subtree before the right subtree**.
- The initial solution updates the answer after reaching a `None` child using the parent node.
- The optimized DFS updates the answer directly while visiting nodes, making the recursion cleaner.
- BFS provides another intuitive solution by processing the tree level by level and recording the first node of each level.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">1110. Delete Nodes And Return Forest</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, Postorder Traversal, Tree Modification

---

# 📖 Problem Statement

You are given the root of a binary tree and an array `to_delete`.

Delete every node whose value appears in `to_delete`.

After deleting those nodes, the remaining disconnected trees form a **forest**.

Return the **roots** of all trees in the forest.

---

## Example 1

### Input

```text
Tree

          1
        /   \
       2     3
      / \   / \
     4   5 6   7

to_delete = [3,5]
```

### Output

```text
        1
       /
      2
     /
    4

6

7
```

Forest

```text
Tree 1

    1
   /
  2
 /
4

Tree 2

6

Tree 3

7
```

---

## Example 2

### Input

```text
      1
     / \
    2   4
     \
      3
```

```text
to_delete = [3]
```

### Output

```text
      1
     / \
    2   4
```

---

# 🤔 The Wrong Way to Think

Most people immediately think

```text
Delete node

Reconnect children

Maintain tree
```

This quickly becomes confusing.

Instead,

change the question.

---

# 💡 The Right Question

Instead of asking

> **"How do I delete this node?"**

ask

> **"After processing my children, what subtree should I return to my parent?"**

This small change makes the problem much easier.

---

# Step 1: What Should DFS Return?

Suppose you're at

```text
      2
     / \
    4   5
```

After processing both children,

what should be returned to the parent?

Only two possibilities exist.

Return

```text
Current Node
```

or

Return

```text
None
```

Nothing else.

---

# Step 2: Why Postorder DFS?

Consider

```text
      3
     / \
    6   7
```

Suppose

```text
3
```

must be deleted.

Before deleting it,

we need to know

- What happened to `6`
- What happened to `7`

Therefore,

children must be processed first.

Traversal order becomes

```text
Left

↓

Right

↓

Current Node
```

which is exactly

```text
Postorder DFS
```

---

# Step 3: The Important Decision

Suppose DFS has already processed

```text
6

7
```

Now we are standing at

```text
3
```

Question

```text
Should I delete node 3?
```

---

### Case 1

Delete

```text
3
```

Then

```text
6

7
```

become new tree roots.

Store them.

Return

```python
None
```

because the parent should disconnect node `3`.

---

### Case 2

Do not delete

```text
3
```

Simply return

```python
node
```

so the parent keeps the connection.

---

# Step 4: Dry Run

Tree

```text
          1
        /   \
       2     3
      / \   / \
     4   5 6   7
```

Delete

```text
3

5
```

---

## Visit Node 4

Leaf

Not deleted.

Return

```text
4
```

---

## Visit Node 5

Leaf

Delete.

Return

```text
None
```

Node `2` now becomes

```text
      2
     /
    4
```

---

## Visit Node 6

Leaf

Return

```text
6
```

---

## Visit Node 7

Leaf

Return

```text
7
```

---

## Visit Node 3

Children

```text
6

7
```

Need to delete?

Yes.

Add

```text
6

7
```

to the answer.

Return

```python
None
```

Parent disconnects node `3`.

---

## Visit Root 1

Tree becomes

```text
      1
     /
    2
   /
  4
```

Root survives.

Return

```text
1
```

Since the root is not deleted,

add it to the answer.

---

Final Forest

```text
Tree 1

    1
   /
  2
 /
4

Tree 2

6

Tree 3

7
```

---

# The Core Logic

The recursion performs exactly **two jobs**.

---

## Job 1

Process the children.

```python
node.left = dfs(node.left)

node.right = dfs(node.right)
```

Notice this pattern.

The child may return

- A modified subtree
- Or `None`

The parent reconnects accordingly.

---

## Job 2

Decide whether the current node survives.

If the node must be deleted

```python
return None
```

Otherwise

```python
return node
```

---

# Python Code

```python
class Solution:

    def delNodes(self, root, to_delete):

        delete = set(to_delete)
        ans = []

        def dfs(node):

            if not node:
                return None

            # Process children first
            node.left = dfs(node.left)
            node.right = dfs(node.right)

            # Delete current node
            if node.val in delete:

                if node.left:
                    ans.append(node.left)

                if node.right:
                    ans.append(node.right)

                return None

            return node

        root = dfs(root)

        # Root survives
        if root:
            ans.append(root)

        return ans
```

---

# Code Explanation

## Step 1

Convert

```python
to_delete
```

into a set.

```python
delete = set(to_delete)
```

This allows

```python
node.val in delete
```

to run in

```text
O(1)
```

---

## Step 2

Recursively process both children.

```python
node.left = dfs(node.left)

node.right = dfs(node.right)
```

Each child returns

- Updated subtree
- Or `None`

---

## Step 3

Check whether the current node must be deleted.

If yes,

its surviving children become new roots.

```python
if node.left:
    ans.append(node.left)

if node.right:
    ans.append(node.right)
```

Return

```python
None
```

to disconnect this node.

---

## Step 4

If the node is not deleted,

simply return

```python
node
```

---

## Step 5

After DFS,

if the original root still exists,

add it to the forest.

```python
if root:
    ans.append(root)
```

---

# Why Does This Work?

Imagine every node asks itself

```text
Should I stay?

OR

Should I disappear?
```

If it disappears,

its surviving children become independent trees.

If it stays,

it simply returns itself to its parent.

The parent reconnects using

```python
node.left = dfs(node.left)

node.right = dfs(node.right)
```

---

# Complexity Analysis

## Time Complexity

Every node is visited exactly once.

```text
O(n)
```

---

## Space Complexity

HashSet

```text
O(k)
```

where

```text
k = len(to_delete)
```

Recursion Stack

```text
O(h)
```

Overall

```text
O(n)
```

in the worst case.

---

# Pattern Recognition

This problem belongs to the **Tree Modification** category.

Whenever a problem asks to

- Delete Nodes
- Remove Subtrees
- Trim Trees
- Prune Trees
- Filter Trees

think

```python
node.left = dfs(node.left)

node.right = dfs(node.right)
```

The DFS returns

```text
The new subtree root
```

which allows the parent to reconnect the tree correctly.

---

# General Tree Modification Pattern

```python
def dfs(node):

    if not node:
        return None

    node.left = dfs(node.left)
    node.right = dfs(node.right)

    if current node should be removed:
        return None

    return node
```

This pattern appears in many tree problems such as

- Delete Node in BST
- Trim a Binary Search Tree
- Binary Tree Pruning
- Remove Leaf Nodes
- Delete Nodes and Return Forest

---

# 📝 Key Takeaways

- Don't think about **deleting nodes** directly.
- Think about **what subtree should be returned to the parent**.
- Every DFS call returns either:
  - The current node (keep it)
  - `None` (remove it)
- Always process children first using **Postorder DFS**.
- The parent reconnects its children using:

```python
node.left = dfs(node.left)

node.right = dfs(node.right)
```

- If a deleted node has surviving children, they become **new roots** in the resulting forest.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">114. Flatten Binary Tree to Linked List</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS, Tree Modification, Morris Traversal

---

# 📖 Problem Statement

Given the root of a binary tree, flatten the tree into a **linked list**.

The linked list should satisfy:

- Use the **right pointer** as the next pointer.
- Every **left pointer must be `None`**.
- The order of nodes must be the same as the **Preorder Traversal**.

Preorder Traversal means:

```text
Root

↓

Left

↓

Right
```

---

## Example 1

### Input

```text
        1
       / \
      2   5
     / \   \
    3   4   6
```

### Output

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
         \
          6
```

---

## Example 2

Input

```text
[]
```

Output

```text
[]
```

---

## Example 3

Input

```text
0
```

Output

```text
0
```

---

# 🤔 Step 1: Understand What "Flatten" Means

The tree

```text
        1
       / \
      2   5
     / \   \
    3   4   6
```

has the preorder traversal

```text
1

↓

2

↓

3

↓

4

↓

5

↓

6
```

The final tree should simply connect the nodes in this order.

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
         \
          6
```

Notice

```text
Every left pointer becomes NULL.
```

---

# Approach 1: Store the Preorder Traversal

The easiest solution is:

1. Perform preorder traversal.
2. Store all nodes in a list.
3. Reconnect them.

---

## Step 1

Preorder Traversal

```text
1

2

3

4

5

6
```

Store

```python
[1,2,3,4,5,6]
```

---

## Step 2

Reconnect

```text
1 → 2

2 → 3

3 → 4

4 → 5

5 → 6
```

Every

```text
left = None
```

---

## Code

```python
class Solution:

    def flatten(self, root):

        preorder = []

        def dfs(node):

            if not node:
                return

            preorder.append(node)

            dfs(node.left)
            dfs(node.right)

        dfs(root)

        for i in range(len(preorder)-1):

            preorder[i].left = None
            preorder[i].right = preorder[i+1]

        if preorder:
            preorder[-1].left = None
            preorder[-1].right = None
```

---

## Complexity

Time

```text
O(n)
```

Space

```text
O(n)
```

because of the preorder list.

---

# Approach 2: Recursive In-Place Solution

The follow-up asks

```text
O(1) Extra Space
```

So we cannot store the preorder list.

Instead,

we modify the tree while traversing.

---

# Step 1: Focus on One Node

Forget the whole tree.

Stand only at

```text
        1
       / \
      2   5
```

Eventually

```text
1
 \
 2
```

because preorder visits

```text
1

↓

2

↓

5
```

So

```text
Left subtree must become the right subtree.
```

---

# Step 2: The Main Observation

Original

```text
        1
       / \
      2   5
     / \
    3   4
```

Move the left subtree to the right.

```text
1
 \
 2
```

But now,

what happens to the old right subtree?

It must be attached after the **end of the left subtree**.

---

# Step 3: Find the Tail

Flatten the left subtree.

It becomes

```text
2
 \
 3
  \
   4
```

Tail

```text
4
```

Attach

```text
4.right = 5
```

Now move

```text
1.right = 2

1.left = None
```

Tree becomes

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

Exactly what we need.

---

# Why Postorder?

The parent cannot rearrange its children until both subtrees are already flattened.

Therefore

```text
Left

↓

Right

↓

Current
```

This is

```text
Postorder DFS
```

---

# What Should DFS Return?

The parent needs to know

```text
Where does my flattened subtree end?
```

Return

```text
Tail Node
```

of the flattened subtree.

Example

```text
    2
   / \
  3   4
```

After flattening

```text
2
 \
 3
  \
   4
```

Return

```text
4
```

because the parent needs to attach its old right subtree after node `4`.

---

# Recursive Code

```python
class Solution:

    def flatten(self, root):

        def dfs(node):

            if not node:
                return None

            leftTail = dfs(node.left)
            rightTail = dfs(node.right)

            if leftTail:

                leftTail.right = node.right
                node.right = node.left
                node.left = None

            if rightTail:
                return rightTail

            if leftTail:
                return leftTail

            return node

        dfs(root)
```

---

# Dry Run

Tree

```text
        1
       / \
      2   5
     / \   \
    3   4   6
```

---

### Visit 3

Already flat.

Tail

```text
3
```

---

### Visit 4

Tail

```text
4
```

---

### Visit 2

Left Tail

```text
3
```

Right Tail

```text
4
```

Connect

```text
3.right = 4
```

Move

```text
2.right = 3

2.left = None
```

Flattened

```text
2
 \
 3
  \
   4
```

Tail

```text
4
```

---

### Visit 6

Tail

```text
6
```

---

### Visit 5

Tail

```text
6
```

---

### Visit 1

Left Tail

```text
4
```

Right Tail

```text
6
```

Attach

```text
4.right = 5
```

Move

```text
1.right = 2

1.left = None
```

Final

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
        \
         6
```

---

# Approach 3: Morris Traversal (O(1) Extra Space)

Instead of recursion,

modify pointers iteratively.

For every node

If a left child exists

1. Find the **rightmost node** of the left subtree.
2. Attach the original right subtree.
3. Move the left subtree to the right.
4. Set the left pointer to `None`.

Repeat until the tree is flattened.

---

## Code

```python
class Solution:

    def flatten(self, root):

        curr = root

        while curr:

            if curr.left:

                prev = curr.left

                while prev.right:
                    prev = prev.right

                prev.right = curr.right
                curr.right = curr.left
                curr.left = None

            curr = curr.right
```

---

# Complexity Comparison

| Approach | Time | Extra Space |
|----------|------|-------------|
| Store Preorder List | O(n) | O(n) |
| Recursive (Tail Returned) | O(n) | O(h) |
| Morris Traversal | O(n) | O(1) |

---

# Pattern Recognition

This is a **Tree Modification** problem.

Ask yourself

> **What does my parent need after I'm done?**

The answer is

```text
The Tail of the Flattened Subtree
```

The parent uses that tail to attach its original right subtree.

---

# General Tree Modification Pattern

Whenever a problem asks you to

- Modify a Tree
- Flatten a Tree
- Rearrange Nodes
- Convert Tree Structure

think

```text
Process Children First

↓

Modify Current Node

↓

Return Information Needed by Parent
```

---

# 📝 Key Takeaways

- The final linked list follows the **Preorder Traversal**.
- Every node's **left pointer becomes `None`**.
- The recursive solution returns the **tail node** of the flattened subtree.
- The parent uses this tail to reconnect its original right subtree.
- The most optimized solution is **Morris Traversal**, which performs the flattening in **O(1)** extra space.
- This problem is a classic example of **modifying a tree while returning useful information (the tail) to the parent**.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">872. Leaf-Similar Trees</span>

- **Difficulty:** Easy
- **Topics:** Binary Tree, DFS

---

# 📖 Problem Statement

For every binary tree, consider all its **leaf nodes** from **left to right**.

The values of these leaves form the **leaf value sequence**.

Two trees are **leaf-similar** if their leaf sequences are exactly the same.

Return

```text
True
```

if both trees have identical leaf sequences,

otherwise return

```text
False
```

---

## Example 1

### Tree 1

```text
        3
       / \
      5   1
     / \ / \
    6  2 9  8
      / \
     7   4
```

Leaf sequence

```text
[6, 7, 4, 9, 8]
```

---

### Tree 2

```text
        3
       / \
      5   1
     / \ / \
    6  7 4  2
            / \
           9   8
```

Leaf sequence

```text
[6, 7, 4, 9, 8]
```

Output

```text
True
```

---

## Example 2

Tree 1

```text
    1
   / \
  2   3
```

Leaf sequence

```text
[2,3]
```

Tree 2

```text
    1
   / \
  3   2
```

Leaf sequence

```text
[3,2]
```

Output

```text
False
```

---

# 💡 Key Observation

We don't need to compare the trees themselves.

We only need to compare

```text
Leaf Sequence of Tree 1

AND

Leaf Sequence of Tree 2
```

If both sequences are identical,

the trees are leaf-similar.

---

# My Initial Solution

```python
class Solution:
    def leafSimilar(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:

        ans = []

        def getLeaves(root):
            nonlocal ans

            if not root:
                return None

            left = getLeaves(root.left)
            right = getLeaves(root.right)

            if not left and not right:
                ans.append(root.val)

            return ans

        l1 = getLeaves(root1)
        l2 = getLeaves(root2)

        print(l1, l2)

        return l1 == l2
```

---

# Why This Solution Does Not Work

The problem is that

```python
ans
```

is shared between **both trees**.

Execution

```python
l1 = getLeaves(root1)
```

Suppose Tree 1 has

```text
[6,7,4,9,8]
```

Now

```python
ans
```

contains

```python
[6,7,4,9,8]
```

---

Next

```python
l2 = getLeaves(root2)
```

Instead of creating a new list,

the same list is used again.

Now

```python
ans
```

becomes

```python
[6,7,4,9,8,6,7,4,9,8]
```

Both

```python
l1
```

and

```python
l2
```

refer to the **same list object**.

So comparing

```python
l1 == l2
```

does not compare two independent leaf sequences.

---

# Memory Visualization

Initially

```text
ans
 │
 ▼
[]
```

After processing Tree 1

```text
ans
 │
 ▼
[6,7,4,9,8]
 ▲
 │
l1
```

After processing Tree 2

```text
ans
 │
 ▼
[6,7,4,9,8,6,7,4,9,8]
 ▲              ▲
 │              │
l1             l2
```

Both variables point to the **same list**.

Appending values changes the same object.

---

# Optimized Approach

Instead of using one shared list,

create a separate list for each tree.

Then compare the two lists.

---

## Code

```python
class Solution:

    def leafSimilar(self, root1, root2):

        def getLeaves(root, leaves):

            if not root:
                return

            if not root.left and not root.right:
                leaves.append(root.val)
                return

            getLeaves(root.left, leaves)
            getLeaves(root.right, leaves)

        l1 = []
        l2 = []

        getLeaves(root1, l1)
        getLeaves(root2, l2)

        return l1 == l2
```

---

# Dry Run

Tree

```text
        3
       / \
      5   1
     / \ / \
    6  2 9  8
      / \
     7   4
```

---

Visit

```text
6
```

Leaf

Append

```text
[6]
```

---

Visit

```text
7
```

Append

```text
[6,7]
```

---

Visit

```text
4
```

Append

```text
[6,7,4]
```

---

Visit

```text
9
```

Append

```text
[6,7,4,9]
```

---

Visit

```text
8
```

Append

```text
[6,7,4,9,8]
```

The same process is repeated for the second tree.

Finally compare

```python
l1 == l2
```

If equal

```text
Return True
```

Otherwise

```text
Return False
```

---

# Why Does This Work?

DFS naturally visits leaves from

```text
Left

↓

Right
```

which is exactly the order required by the problem.

Each tree independently builds its own leaf sequence.

The final comparison becomes a simple list comparison.

---

# Complexity Analysis

Let

```text
n = Number of Nodes
```

## Time Complexity

Each node is visited once.

```text
O(n)
```

---

## Space Complexity

Leaf sequences

```text
O(L)
```

where

```text
L = Number of Leaf Nodes
```

Recursion stack

```text
O(h)
```

where

```text
h = Height of Tree
```

Overall

```text
O(n)
```

---

# Pattern Recognition

This is a **Tree Traversal + Collection** problem.

The goal is not to modify the tree.

Instead,

DFS is used to **collect information**.

General pattern

```text
Traverse Tree

↓

Collect Required Nodes

↓

Compare the Collected Result
```

---

# 📝 Key Takeaways

- The problem only requires comparing the **leaf sequences**, not the tree structures.
- DFS naturally visits leaves from **left to right**, matching the required order.
- Using one shared list for both trees causes both variables to reference the **same list object**.
- Create **separate lists** for each tree to collect leaf values independently.
- Finally, compare the two leaf sequences using

```python
l1 == l2
```

to determine whether the trees are leaf-similar.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">129. Sum Root to Leaf Numbers</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, DFS

---

# 📖 Problem Statement

You are given the root of a binary tree where every node contains a digit from **0 to 9**.

Each **root-to-leaf path** forms a number.

Example

```text
1 → 2 → 3
```

represents

```text
123
```

Return the **sum of all root-to-leaf numbers**.

---

## Example 1

### Input

```text
    1
   / \
  2   3
```

Root-to-leaf numbers

```text
1 → 2 = 12

1 → 3 = 13
```

Output

```text
12 + 13 = 25
```

---

## Example 2

### Input

```text
        4
       / \
      9   0
     / \
    5   1
```

Root-to-leaf numbers

```text
4 → 9 → 5 = 495

4 → 9 → 1 = 491

4 → 0 = 40
```

Output

```text
495 + 491 + 40 = 1026
```

---

# 💡 Key Observation

Every root-to-leaf path forms one number.

While moving from parent to child,

we should keep building the current number.

This is a **Parent → Child Information Flow** problem.

The parent passes

```text
Current Number Built So Far
```

to its children.

---

# My Initial Solution

## Idea

Carry the current number as a **string**.

Whenever a leaf is reached,

convert the string into an integer and add it to the answer.

---

## Code

```python
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:

        ans = 0

        def dfs(node, val):
            nonlocal ans

            if not node:
                return

            if node.left is None and node.right is None:
                ans += int(val + str(node.val))
                return

            dfs(node.left, val + str(node.val))
            dfs(node.right, val + str(node.val))

        dfs(root, "")

        return ans
```

---

# How This Solution Works

Tree

```text
      4
     / \
    9   0
   / \
  5   1
```

Start

```text
""
```

Visit

```text
4
```

Current String

```text
"4"
```

---

Visit

```text
9
```

Current String

```text
"49"
```

---

Visit

```text
5
```

Leaf

Current String

```text
"495"
```

Convert

```python
int("495")
```

Answer

```text
495
```

---

Visit

```text
1
```

Current String

```text
"491"
```

Answer

```text
495 + 491
```

---

Visit

```text
0
```

Current String

```text
"40"
```

Answer

```text
495 + 491 + 40 = 1026
```

---

# Drawback of This Approach

The solution is correct,

but every recursive call

- Creates a new string
- Copies the previous string
- Converts the final string into an integer

Since the problem is actually building a **number**, using strings is unnecessary.

---

# Optimized Approach

Instead of carrying a string,

carry the current number as an integer.

Suppose

```text
Current Number = 49
```

Visit

```text
5
```

New Number

```text
49 × 10 + 5

=

495
```

General Formula

```python
curr = curr * 10 + node.val
```

No string conversion is needed.

---

# Optimized Code

```python
class Solution:

    def sumNumbers(self, root: Optional[TreeNode]) -> int:

        def dfs(node, curr):

            if not node:
                return 0

            curr = curr * 10 + node.val

            if not node.left and not node.right:
                return curr

            return dfs(node.left, curr) + dfs(node.right, curr)

        return dfs(root, 0)
```

---

# Dry Run

Tree

```text
        4
       / \
      9   0
     / \
    5   1
```

---

### Start

Current Number

```text
0
```

---

### Visit 4

```text
0 × 10 + 4

=

4
```

---

### Visit 9

```text
4 × 10 + 9

=

49
```

---

### Visit 5

```text
49 × 10 + 5

=

495
```

Leaf

Return

```text
495
```

---

### Visit 1

```text
49 × 10 + 1

=

491
```

Leaf

Return

```text
491
```

---

### Visit 0

```text
4 × 10 + 0

=

40
```

Leaf

Return

```text
40
```

---

Final Answer

```text
495 + 491 + 40

=

1026
```

---

# Why Does This Work?

At every node,

the parent has already built part of the number.

The child simply extends it by adding one more digit.

Example

Current Number

```text
49
```

Visit

```text
5
```

New Number

```text
49 × 10 + 5

=

495
```

Every root-to-leaf path independently builds its own number.

When a leaf is reached,

that complete number is returned.

---

# Complexity Analysis

## Initial Solution

### Time Complexity

```text
O(n)
```

Additional string creation and conversion occur at every path.

---

### Space Complexity

```text
O(h)
```

where

```text
h = Height of Tree
```

---

## Optimized Solution

### Time Complexity

```text
O(n)
```

Every node is visited exactly once.

---

### Space Complexity

```text
O(h)
```

Only the recursion stack is used.

---

# Pattern Recognition

This problem belongs to the **Parent → Child Information Flow** category.

The parent passes

```text
Current Number Built So Far
```

Each child updates it using

```python
curr = curr * 10 + node.val
```

and passes the updated value to its children.

Similar problems include

- Sum of Root to Leaf Binary Numbers
- Path Sum
- Count Good Nodes
- Maximum Difference Between Node and Ancestor

---

# 📝 Key Takeaways

- Every root-to-leaf path represents one number.
- The current number is passed from parent to child.
- The initial solution builds the number using **strings** and converts it to an integer at the leaf.
- The optimized solution builds the number mathematically using

```python
curr = curr * 10 + node.val
```

- Returning the sum directly from DFS avoids shared state (`nonlocal`) and results in cleaner recursion.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">257. Binary Tree Paths</span>

- **Difficulty:** Easy
- **Topics:** Binary Tree, DFS, Backtracking

---

# 📖 Problem Statement

Given the root of a binary tree, return **all root-to-leaf paths**.

A **leaf node** is a node that has **no left child and no right child**.

Each path should be represented as

```text
"1->2->5"
```

---

## Example 1

### Input

```text
        1
       / \
      2   3
       \
        5
```

### Output

```python
["1->2->5", "1->3"]
```

---

## Example 2

### Input

```text
1
```

### Output

```python
["1"]
```

---

# 🤔 Step 1: Understand the Problem

We need every path that starts from the **root** and ends at a **leaf**.

Example

```text
        1
       / \
      2   3
       \
        5
```

Possible paths

```text
1 → 2 → 5

1 → 3
```

Convert them into strings

```text
"1->2->5"

"1->3"
```

Store them in a list.

---

# 💡 Step 2: How to Think

Whenever you see

```text
Root → Leaf
```

your first thought should be

```text
Carry the current path from parent to child.
```

This is a **Parent → Child Information Flow** problem.

The DFS state becomes

```python
dfs(node, path)
```

where

```text
path = Current Root-to-Current-Node Path
```

---

# Step 3: What Should `path` Contain?

Suppose we are here

```text
        1
       /
      2
```

Current path

```text
1->2
```

Move to

```text
5
```

New path

```text
1->2->5
```

If this is a leaf,

store it.

---

# Dry Run

Tree

```text
        1
       / \
      2   3
       \
        5
```

---

### Start

```python
dfs(1, "")
```

---

### Visit 1

Current Path

```text
1
```

Go left.

---

### Visit 2

Current Path

```text
1->2
```

Go right.

---

### Visit 5

Current Path

```text
1->2->5
```

Leaf reached.

Store

```python
["1->2->5"]
```

Return.

---

Backtrack.

Visit right child.

---

### Visit 3

Current Path

```text
1->3
```

Leaf reached.

Store

```python
["1->2->5", "1->3"]
```

Done.

---

# Solution 1 (Easy to Understand)

```python
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:

        ans = []

        def dfs(node, path):

            if not node:
                return

            if path == "":
                path = str(node.val)
            else:
                path = path + "->" + str(node.val)

            if not node.left and not node.right:
                ans.append(path)
                return

            dfs(node.left, path)
            dfs(node.right, path)

        dfs(root, "")

        return ans
```

---

# Code Explanation

Every recursive call receives

```text
Current Path
```

The current node is added to that path.

If the node is a leaf,

store the completed path.

Otherwise,

continue exploring the children.

---

# Solution 2 (Cleaner)

Instead of checking whether the path is empty,

start DFS with the root value.

```python
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:

        ans = []

        def dfs(node, path):

            if not node.left and not node.right:
                ans.append(path)
                return

            if node.left:
                dfs(node.left, path + "->" + str(node.left.val))

            if node.right:
                dfs(node.right, path + "->" + str(node.right.val))

        dfs(root, str(root.val))

        return ans
```

This version avoids handling the empty string separately.

---

# Solution 3 (Backtracking)

Instead of creating a new string at every recursive call,

carry the current path as a list.

Only convert it into a string when a leaf is reached.

```python
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:

        ans = []

        def dfs(node, path):

            if not node:
                return

            path.append(str(node.val))

            if not node.left and not node.right:
                ans.append("->".join(path))
            else:
                dfs(node.left, path)
                dfs(node.right, path)

            path.pop()

        dfs(root, [])

        return ans
```

---

# Why Backtracking Works

Suppose

```text
        1
       / \
      2   3
```

Visit

```text
1
```

Path

```text
["1"]
```

Visit

```text
2
```

Path

```text
["1","2"]
```

Leaf

Store

```text
"1->2"
```

Backtrack

```python
path.pop()
```

Path becomes

```text
["1"]
```

Now visit

```text
3
```

Path

```text
["1","3"]
```

Leaf

Store

```text
"1->3"
```

Backtracking ensures the same list can be reused for every path.

---

# Complexity Analysis

Let

- `N` = Number of Nodes
- `L` = Total Length of All Output Strings

## Time Complexity

```text
O(N + L)
```

Every node is visited once,

and every character in the output must be created.

---

## Space Complexity

Recursion Stack

```text
O(H)
```

where

```text
H = Height of Tree
```

Ignoring the output list,

the DFS uses only the recursion stack.

---

# Pattern Recognition

This problem belongs to the **Parent → Child Information Flow** category.

The parent passes

```text
Current Path
```

to its children.

Each child extends the path.

When a leaf is reached,

the path is stored.

Similar problems include

- Path Sum
- Sum Root to Leaf Numbers
- Sum Root to Leaf Binary Numbers
- Good Nodes
- Maximum Difference Between Node and Ancestor

---

# 📝 Key Takeaways

- Root-to-leaf problems usually require carrying information **downward**.
- Here, the DFS state is the **current path**.
- When a leaf is reached, store the completed path.
- The simplest solution builds the path as a string.
- A backtracking solution uses a list and converts it into a string only at the leaf, avoiding repeated string creation.
- The general DFS template is

```python
dfs(node, state)
```

where `state` represents information accumulated from the root to the current node.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">107. Binary Tree Level Order Traversal II</span>

- **Difficulty:** Medium
- **Topics:** Binary Tree, Breadth-First Search (BFS), Queue

---

# 📖 Problem Statement

Given the root of a binary tree, return the **bottom-up level order traversal** of its nodes' values.

Unlike the normal level order traversal, the result should start from the **last level** and end at the **root**.

Traversal inside each level is still

```text
Left → Right
```

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

### Output

```python
[[15,7],[9,20],[3]]
```

---

## Example 2

### Input

```text
1
```

### Output

```python
[[1]]
```

---

## Example 3

### Input

```text
[]
```

### Output

```python
[]
```

---

# 💡 Key Observation

The problem says

```text
Level Order Traversal
```

which immediately suggests

```text
BFS + Queue
```

The only difference from **LeetCode 102** is that the final answer should be

```text
Bottom → Top
```

instead of

```text
Top → Bottom
```

---

# My Solution

## Idea

Perform a normal BFS.

Instead of appending each level at the end,

insert it at the **front** of the answer using

```python
appendleft()
```

This automatically builds the answer in bottom-up order.

---

## Code

```python
from collections import deque

class Solution:
    def levelOrderBottom(self, root: Optional[TreeNode]) -> List[List[int]]:

        if not root:
            return []

        q = deque([root])
        ans = deque([])

        while q:

            qSize = len(q)
            level = []

            for i in range(qSize):

                node = q.popleft()

                level.append(node.val)

                if node.left:
                    q.append(node.left)

                if node.right:
                    q.append(node.right)

            ans.appendleft(level)

        return list(ans)
```

---

# Dry Run

Tree

```text
        3
       / \
      9   20
         /  \
        15   7
```

---

### Initial

Queue

```text
[3]
```

Answer

```text
[]
```

---

### Level 0

Process

```text
3
```

Level

```text
[3]
```

Insert at front

```text
[[3]]
```

Queue

```text
[9,20]
```

---

### Level 1

Process

```text
9

20
```

Level

```text
[9,20]
```

Insert at front

```text
[[9,20],[3]]
```

Queue

```text
[15,7]
```

---

### Level 2

Process

```text
15

7
```

Level

```text
[15,7]
```

Insert at front

```text
[[15,7],[9,20],[3]]
```

Queue

```text
[]
```

Traversal completed.

---

# Why Does `appendleft()` Work?

Normally,

Level Order Traversal produces

```text
[
 [3],
 [9,20],
 [15,7]
]
```

Instead of reversing the answer later,

every completed level is inserted at the front.

After processing

Level 0

```text
[[3]]
```

After processing

Level 1

```text
[[9,20],[3]]
```

After processing

Level 2

```text
[[15,7],[9,20],[3]]
```

The answer is already in the required order.

---

# Optimized Version

A small improvement is simply initializing the deque without passing an empty list.

```python
from collections import deque

class Solution:
    def levelOrderBottom(self, root: Optional[TreeNode]) -> List[List[int]]:

        if not root:
            return []

        q = deque([root])
        ans = deque()

        while q:

            level = []

            for _ in range(len(q)):

                node = q.popleft()

                level.append(node.val)

                if node.left:
                    q.append(node.left)

                if node.right:
                    q.append(node.right)

            ans.appendleft(level)

        return list(ans)
```

The logic remains exactly the same.

---

# Alternative Solution

Another common approach is

1. Perform normal level order traversal.
2. Reverse the final answer.

```python
from collections import deque

class Solution:
    def levelOrderBottom(self, root: Optional[TreeNode]) -> List[List[int]]:

        if not root:
            return []

        q = deque([root])
        ans = []

        while q:

            level = []

            for _ in range(len(q)):

                node = q.popleft()

                level.append(node.val)

                if node.left:
                    q.append(node.left)

                if node.right:
                    q.append(node.right)

            ans.append(level)

        return ans[::-1]
```

Both approaches have the same time complexity.

---

# Complexity Analysis

## Time Complexity

Every node is visited exactly once.

```text
O(n)
```

---

## Space Complexity

The queue stores at most one level of the tree.

```text
O(n)
```

in the worst case.

---

# Pattern Recognition

Whenever a tree problem contains words like

- Level Order
- Level by Level
- Bottom-Up Level
- Zigzag Level
- Average of Levels

immediately think

```text
Queue

↓

BFS
```

The only difference between these problems is

**what you do after processing each level.**

---

# 📝 Key Takeaways

- The phrase **"Level Order Traversal"** immediately indicates **BFS using a Queue**.
- Process one complete level at a time using the queue size.
- Instead of reversing the answer at the end, use

```python
appendleft(level)
```

to build the result directly in bottom-up order.
- Both the `appendleft()` approach and the `reverse()` approach have the same overall time complexity.
- This problem is a simple variation of the standard **Level Order Traversal** with only the output order changed.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">437. Path Sum III</span>

**Difficulty:** Medium

**Topics**
- Binary Tree
- DFS
- Prefix Sum
- HashMap
- Backtracking

---

# Problem Statement

Given the root of a binary tree and an integer `targetSum`, return the number of paths whose sum of node values equals `targetSum`.

### Rules

- The path **does NOT have to start from the root.**
- The path **does NOT have to end at a leaf.**
- The path **must always move downward** (parent → child).

---

# Understanding the Problem

Most tree problems ask:

- Root → Leaf
- Root → Any Node
- Leaf → Leaf

This problem is different.

It asks:

> Count every downward path whose sum equals `targetSum`.

Example:

```
        10
      /    \
     5     -3
    / \      \
   3   2      11
  / \   \
 3  -2   1
```

Target = **8**

Valid Paths

```
5 → 3

5 → 2 → 1

-3 → 11
```

Notice:

- Doesn't start from root.
- Doesn't necessarily end at a leaf.
- Only moves downward.

---

# First Observation

Suppose we try every node as a starting point.

```
Node 10

Check every path

Node 5

Check every path

Node 3

Check every path

...
```

This works.

But many paths are repeated.

Time Complexity becomes

```
O(N²)
```

We need something better.

---

# Thinking Like an Interviewer

Instead of asking

> "Where does this path start?"

Ask

> "Where does this path end?"

Suppose we are currently standing at a node.

Can we determine how many valid paths end here?

If yes,

then every node contributes independently.

---

# Prefix Sum Idea

Suppose current root-to-node path is

```
10 → 5 → 3
```

Running Sum

```
10

15

18
```

Suppose target is

```
8
```

Question:

Can some earlier prefix be removed so that the remaining path equals 8?

Current Prefix

```
18
```

Need

```
18 - X = 8
```

Therefore

```
X = 10
```

Did we previously see prefix sum

```
10
```

Yes.

Then

```
18 - 10 = 8
```

Meaning

```
5 → 3
```

is a valid path.

---

# Mathematical Formula

Suppose

```
Current Prefix = P

Earlier Prefix = X
```

Path Sum

```
P - X
```

Need

```
P - X = target
```

Rearrange

```
X = P - target
```

So at every node we ask

> Have we already seen a prefix sum equal to

```
runningSum - target
```

If yes,

then every occurrence represents one valid path.

---

# Why a HashMap?

Suppose

```
Prefix Sums

10

15

10

18
```

Current Prefix

```
18
```

Target

```
8
```

Need

```
10
```

But

```
10
```

appears twice.

That means

```
Two different paths
```

So we need

```
Prefix Sum → Frequency
```

Example

```
{
0 : 1,
10 : 2,
15 : 1
}
```

If

```
running-target = 10
```

Answer increases by

```
2
```

not

```
1
```

This is why a HashMap is necessary.

---

# Why Your Code Fails

Your code

```python
track = [0]

...

if pathSum-targetSum in track:
    ans += 1
```

Problems:

### Problem 1

You only check

```
Exists?
```

But don't count

```
How many times?
```

Suppose

```
track

[0,5,5]
```

Need

```
5
```

Correct Answer

```
2 paths
```

Your code counts

```
1
```

---

### Problem 2

No Backtracking

Suppose tree

```
      1
     / \
   -2  -3
```

Target

```
-1
```

Your DFS

After visiting left

```
track

[0,1,-1]
```

Then DFS goes right.

But

```
-1
```

belongs only to the left subtree.

Right subtree should never see it.

Your list keeps growing forever.

This mixes prefix sums from different branches.

---

# Why Backtracking Is Needed

The prefix sums should represent

ONLY

```
Root

↓

Current Node
```

Nothing else.

Correct DFS

```
Root

track

[0]

↓

Left

track

[0,1]

↓

Return

track

[0]

↓

Right

track

[0,1]
```

Wrong DFS

```
Root

↓

Left

[0,1,-1]

↓

Return

[0,1,-1]

↓

Right

Still contains -1

Wrong!
```

Always remove the current prefix before returning.

---

# Correct Algorithm

For every node

Step 1

Update running sum

```
running += node.val
```

Step 2

Need

```
running-target
```

Step 3

If found

```
answer += frequency
```

Step 4

Insert current prefix

```
prefix[running] += 1
```

Step 5

DFS Left

Step 6

DFS Right

Step 7

Backtrack

```
prefix[running] -= 1
```

---

# Dry Run

Example

```
        10
       /
      5
     /
    3

Target = 8
```

Initially

```
Prefix Map

{
0 : 1
}
```

Answer

```
0
```

---

## Visit 10

Running

```
10
```

Need

```
2
```

Not found

Insert

```
{
0:1,
10:1
}
```

---

## Visit 5

Running

```
15
```

Need

```
7
```

Not found

Insert

```
{
0:1,
10:1,
15:1
}
```

---

## Visit 3

Running

```
18
```

Need

```
10
```

Found

```
Frequency = 1
```

Answer

```
1
```

The valid path is

```
5 → 3
```

---

Backtrack

Remove

```
18
```

Return

Backtrack

Remove

```
15
```

Return

Backtrack

Remove

```
10
```

Finished.

---

# Visualization

Current Path

```
10

↓

5

↓

3
```

Running Sum

```
18
```

Need

```
18-8

=

10
```

Since

```
10
```

exists

Everything after

```
10
```

forms

```
8
```

Exactly

```
5 → 3
```

---

# Brute Force Solution

Idea

Start DFS from every node.

For each node

Explore every downward path.

Count sums.

Time Complexity

```
O(N²)
```

Space

```
O(H)
```

where

```
H
```

is tree height.

---

# Optimized Solution

Use

- DFS
- Prefix Sum
- HashMap
- Backtracking

Time Complexity

```
O(N)
```

Every node visited once.

Space Complexity

```
O(H)
```

Average

Worst Case

```
O(N)
```

for skewed tree.

---

# Correct Python Solution

```python
from collections import defaultdict

class Solution:
    def pathSum(self, root, targetSum):

        prefix = defaultdict(int)
        prefix[0] = 1

        ans = 0

        def dfs(node, running):

            nonlocal ans

            if not node:
                return

            running += node.val

            ans += prefix[running - targetSum]

            prefix[running] += 1

            dfs(node.left, running)
            dfs(node.right, running)

            # Backtracking
            prefix[running] -= 1

        dfs(root, 0)

        return ans
```

---

# Why Does This Work?

At every node

```
Current Prefix = P
```

Need

```
Earlier Prefix = P - target
```

If it exists

Then

```
Current Prefix

-

Earlier Prefix

=

Target
```

Every occurrence corresponds to a different valid path ending at the current node.

---

# Pattern Recognition

Whenever a problem contains

- Path Sum
- Continuous Sum
- Subarray Sum
- Root to Current Path
- Running Total

Think

```
Prefix Sum
```

If the problem asks

```
How many?
```

Think

```
Prefix Sum + HashMap
```

If it involves DFS

Think

```
Backtracking
```

---

# Interview Thinking Process

Instead of memorizing

```
Use Prefix Sum
```

Ask yourself

### Question 1

Am I dealing with sums?

↓

Yes

Running Sum

---

### Question 2

Can the path start anywhere?

↓

Yes

Need to subtract an earlier prefix.

---

### Question 3

What should I subtract?

```
Current Prefix - Target
```

---

### Question 4

How do I know if that prefix existed?

↓

HashMap

---

### Question 5

Can sibling branches share prefix sums?

↓

No

Need Backtracking

---

Final Thought Process

```
Tree

↓

Need Path Sum

↓

Running Prefix Sum

↓

Need Earlier Prefix

↓

HashMap

↓

DFS

↓

Backtracking

↓

O(N)
```

---

# Common Mistakes

❌ Using a list instead of a HashMap

❌ Forgetting duplicate prefix sums

❌ Forgetting backtracking

❌ Sharing prefix sums between sibling subtrees

❌ Thinking every path must start from the root

---

# Key Takeaways

✅ Prefix Sum converts path-sum problems into subtraction problems.

✅ A HashMap stores the frequency of prefix sums.

✅ Every node asks:
```
Have I already seen (runningSum - target)?
```

✅ Backtracking ensures the prefix map only contains the current root-to-node path.

✅ This reduces the time complexity from **O(N²)** to **O(N)**.

<br/><br/><br/><br/><br/>

---

# <span style="color: limegreen;">95. Unique Binary Search Trees II</span>
**Difficulty:** Medium

---

# ⭐ First Impression

When beginners see this problem, they usually think:

> "How can I generate every possible BST?"

That is actually the **wrong first question**.

The correct first question is:

> **What makes one BST different from another?**

Once you answer that, the whole problem becomes much easier.

---

# Step 1: Understand the Problem

Suppose

```
n = 3
```

The numbers are

```
1 2 3
```

We need to construct **every possible BST**.

Remember:

```
BST Property

Left subtree  < Root

Right subtree > Root
```

---

For example,

This is valid

```
    2
   / \
  1   3
```

because

```
1 < 2 < 3
```

---

This is also valid

```
1
 \
  2
   \
    3
```

because

```
1 < 2 < 3
```

---

This is also valid

```
    3
   /
  2
 /
1
```

Still valid.

---

The answer should contain **every unique structure**.

For n=3

There are **5** trees.

---

# Step 2: The Biggest Observation

Instead of thinking

```
How do I build all trees?
```

Think

```
How do I choose the ROOT?
```

This changes everything.

---

Suppose numbers are

```
1 2 3
```

Possible roots are

```
1

2

3
```

Only three possibilities.

Let's see them.

---

# Case 1

Choose

```
Root = 1
```

Then BST property forces

```
Left

Nothing
```

Right

```
2 3
```

Tree becomes

```
    1
     \
    ?
```

Now the question becomes

```
Generate every BST using

2 3
```

Notice...

This is exactly the SAME problem again.

---

# Case 2

Choose

```
Root = 2
```

Left numbers

```
1
```

Right numbers

```
3
```

Easy.

Only one possibility.

```
    2
   / \
  1   3
```

---

# Case 3

Choose

```
Root = 3
```

Left numbers

```
1 2
```

Right

Nothing

Again,

Need to generate

every BST using

1 2

Exactly the same problem again.

---

Notice the recursion?

---

# Step 3

Generalize

Suppose we want to generate

```
4 5 6 7
```

Possible roots

```
4

5

6

7
```

If root is

```
6
```

Left subtree must be

```
4 5
```

Right subtree

```
7
```

Again

Generate all left BSTs

Generate all right BSTs

Combine them.

That's the entire algorithm.

---

# Step 4

Let's define recursion.

```
generate(start,end)
```

means

```
Generate every BST

using numbers

start...

end
```

Example

```
generate(2,4)
```

returns every BST made using

```
2 3 4
```

---

# Step 5

Base Case

Suppose

```
generate(4,3)
```

Meaning

```
No numbers left.
```

Then

```
Return

[None]
```

NOT

```
[]
```

Why?

Because

Suppose

```
Root=2

Left=None

Right=3
```

The parent still needs one possibility.

Returning

```
[]
```

would produce zero combinations.

Returning

```
[None]
```

means

"There is one empty subtree."

This is extremely important.

---

# Step 6

General Case

Suppose

```
generate(1,3)
```

Loop

```
for root in 1..3
```

For each root

Generate

```
leftTrees

rightTrees
```

Then

Combine every left with every right.

---

# Why Combine?

Suppose

Left subtree has

```
2 possibilities
```

Right subtree has

```
3 possibilities
```

Total trees

```
2 × 3

=

6
```

Every left can pair with every right.

This is Cartesian Product.

---

# Dry Run (n=3)

Start

```
generate(1,3)
```

---

## Root = 1

Left

```
generate(1,0)

↓

[None]
```

Right

```
generate(2,3)
```

Let's solve it.

---

### generate(2,3)

Possible roots

```
2

3
```

---

#### Root =2

Left

```
None
```

Right

```
3
```

Tree

```
2
 \
  3
```

---

#### Root =3

Left

```
2
```

Right

```
None
```

Tree

```
    3
   /
  2
```

Return

```
[
2->3,

3<-2
]
```

Now attach them to root=1

Tree 1

```
1
 \
 2
  \
   3
```

Tree 2

```
1
 \
  3
 /
2
```

Two trees.

---

## Root=2

Left

```
1
```

Right

```
3
```

Only one possibility.

```
    2
   / \
  1   3
```

---

## Root=3

Symmetric.

Produces

```
    3
   /
  1
   \
    2
```

and

```
    3
   /
  2
 /
1
```

Total

```
5
```

Exactly the answer.

---

# Why This Works

Every BST has

```
Exactly one root.
```

Once the root is fixed,

BST property automatically fixes

```
Left numbers

Right numbers.
```

There is no choice.

The only remaining work is

Generate left BSTs

Generate right BSTs.

Recursion solves both.

---

# Python Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def generateTrees(self, n: int):

        def generate(start, end):

            if start > end:
                return [None]

            trees = []

            for rootVal in range(start, end + 1):

                leftTrees = generate(start, rootVal - 1)
                rightTrees = generate(rootVal + 1, end)

                for left in leftTrees:
                    for right in rightTrees:

                        root = TreeNode(rootVal)

                        root.left = left
                        root.right = right

                        trees.append(root)

            return trees

        return generate(1, n)
```

---

# Complexity

This is not

```
O(n²)
```

or

```
O(n³)
```

The number of BSTs itself grows.

It follows the

**Catalan Numbers**

```
n=1

1

n=2

2

n=3

5

n=4

14

n=5

42

n=6

132
```

The total time is proportional to the number of trees generated:

```
O(Cn × n)
```

where `Cn` is the nth Catalan number.

---

# Pattern Recognition

This problem belongs to

```
Generate All Possibilities
```

Whenever you see

```
Generate all

Return all

Every possible

All combinations
```

your brain should think

```
Backtracking

or

Divide & Conquer
```

For trees,

this usually becomes

```
Choose Root

↓

Generate Left

↓

Generate Right

↓

Combine
```

---

# Mental Checklist

Whenever you see a similar problem, ask:

1. **Can I choose one element as the root/decision?**
2. **Does that split the remaining problem into independent subproblems?**
3. **Can I recursively generate all possibilities for each side?**
4. **Do I need to combine every left result with every right result?**

If the answer to all four is "yes", you're looking at the same pattern used in:

- Unique Binary Search Trees II
- All Possible Full Binary Trees
- Different Ways to Add Parentheses
- Generate Parentheses
- Expression Tree generation

---

# Interview Thinking

An interviewer is not expecting you to memorize this solution. They are looking for whether you can make the key observation:

> **Choosing the root completely determines the left and right value ranges.**

Once you discover that, the recursion follows naturally:

```
Choose Root
      ↓
Generate Left Trees
      ↓
Generate Right Trees
      ↓
Combine Every Pair
```

That is the intuition behind the entire problem.