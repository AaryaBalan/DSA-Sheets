# Tree Problems

Welcome to the tree problems section! Here you will find various data structure and algorithm problems related to Trees, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [1008. Construct Binary Search Tree from Preorder Traversal](#1008-construct-binary-search-tree-from-preorder-traversal)
- [102. Binary Tree Level Order Traversal](#102-binary-tree-level-order-traversal)
- [2265. Count Nodes Equal to Average of Subtree](#2265-count-nodes-equal-to-average-of-subtree)

<br><br><br><br><br>

---

# 1008. Construct Binary Search Tree from Preorder Traversal

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

# 102. Binary Tree Level Order Traversal

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

# 2265. Count Nodes Equal to Average of Subtree

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