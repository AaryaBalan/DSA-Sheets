# 🌳 Tree Operations

> After learning Tree Basics and Traversals, the next important topic is **Tree Operations**. These operations form the building blocks of almost every Binary Tree interview question.

---

# 📚 Table of Contents

1. Introduction
2. TreeNode Structure
3. Creating a Tree
4. Searching in a Binary Tree
5. Inserting into a Binary Tree
6. Deleting a Node in a Binary Tree
7. Calculating Height
8. Counting Nodes
9. Counting Leaf Nodes
10. Counting Internal Nodes
11. Finding Maximum Value
12. Finding Minimum Value
13. Mirroring (Invert Tree)
14. Copying a Tree
15. Comparing Two Trees
16. Time Complexity
17. Common Interview Questions
18. Summary

---

# 1. Introduction

A Binary Tree supports several fundamental operations.

These operations help solve more advanced problems like:

- Diameter of Binary Tree
- Balanced Binary Tree
- Lowest Common Ancestor
- Maximum Path Sum
- Binary Tree Cameras
- Serialize & Deserialize Tree

---

# 2. TreeNode Structure

```python
class TreeNode:

    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
```

Each node stores:

- Value
- Left Child
- Right Child

---

# Example Tree

Throughout this chapter we'll use

```text
            1
          /   \
         2     3
        / \   /
       4   5 6
```

---

# 3. Creating a Tree

```python
root = TreeNode(1)

root.left = TreeNode(2)
root.right = TreeNode(3)

root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

root.right.left = TreeNode(6)
```

Result

```text
            1
          /   \
         2     3
        / \   /
       4   5 6
```

---

# 4. Searching in a Binary Tree

Unlike a BST,

A Binary Tree has **no ordering**.

Therefore we may need to visit every node.

The easiest approach is DFS.

---

## Algorithm

```
If root is None

Return False

If root value matches

Return True

Search Left

Search Right
```

---

## Python Code

```python
def search(root, target):

    if root is None:
        return False

    if root.val == target:
        return True

    return search(root.left, target) or search(root.right, target)
```

---

Example

Search 5

```text
1

↓

2

↓

4

↓

Back

↓

5

Found
```

---

Time Complexity

```
O(N)
```

---

# 5. Inserting into a Binary Tree

Unlike BST,

There is no fixed position.

The most common interview approach is:

Insert at the first available position using **Level Order Traversal**.

---

Example

Insert 7

Before

```text
            1
          /   \
         2     3
        / \   /
       4   5 6
```

After

```text
            1
          /   \
         2     3
        / \   / \
       4   5 6   7
```

---

Algorithm

```
Queue ← Root

While Queue not Empty

Remove Front Node

If Left Empty

Insert

Else

Push Left

If Right Empty

Insert

Else

Push Right
```

---

Python

```python
from collections import deque

def insert(root, value):

    newNode = TreeNode(value)

    if root is None:
        return newNode

    q = deque([root])

    while q:

        node = q.popleft()

        if node.left is None:
            node.left = newNode
            return root

        q.append(node.left)

        if node.right is None:
            node.right = newNode
            return root

        q.append(node.right)
```

---

Time Complexity

```
O(N)
```

---

# 6. Deleting a Node

Deleting from a Binary Tree is different from deleting from a BST.

General Steps

```
Find Node to Delete

↓

Find Deepest Node

↓

Replace Deleted Node

↓

Delete Deepest Node
```

Example

Delete 2

```text
Before

            1
          /   \
         2     3
        / \   /
       4   5 6
```

Deepest Node = 6

Replace

```text
            1
          /   \
         6     3
        / \
       4   5
```

---

Time Complexity

```
O(N)
```

---

# 7. Finding Height

Height = Longest path from Root to Leaf

Example

```text
            1
          /   \
         2     3
        /
       4
```

Longest path

```
1

↓

2

↓

4
```

Height

```
2
```

---

Recursive Formula

```
Height(root)

=

1 +

max(

Left Height,

Right Height

)
```

---

Python

```python
def height(root):

    if root is None:
        return -1

    return 1 + max(height(root.left), height(root.right))
```

---

Time Complexity

```
O(N)
```

---

# 8. Counting Total Nodes

Recursive Formula

```
Nodes

=

1

+

Left Nodes

+

Right Nodes
```

Python

```python
def countNodes(root):

    if root is None:
        return 0

    return 1 + countNodes(root.left) + countNodes(root.right)
```

Example

```text
            1
          /   \
         2     3
        / \   /
       4   5 6
```

Answer

```
6 Nodes
```

---

# 9. Counting Leaf Nodes

Leaf Node

```
No Left Child

AND

No Right Child
```

Python

```python
def leafNodes(root):

    if root is None:
        return 0

    if root.left is None and root.right is None:
        return 1

    return leafNodes(root.left) + leafNodes(root.right)
```

Example

Leaves

```
4

5

6
```

Answer

```
3
```

---

# 10. Counting Internal Nodes

Internal Node

```
Has at least one child
```

Python

```python
def internalNodes(root):

    if root is None:
        return 0

    if root.left is None and root.right is None:
        return 0

    return 1 + internalNodes(root.left) + internalNodes(root.right)
```

Example

Internal Nodes

```
1

2

3
```

Answer

```
3
```

---

# 11. Finding Maximum Value

Python

```python
def maximum(root):

    if root is None:
        return float("-inf")

    return max(

        root.val,

        maximum(root.left),

        maximum(root.right)

    )
```

Example

```
Maximum

=

6
```

---

# 12. Finding Minimum Value

Python

```python
def minimum(root):

    if root is None:
        return float("inf")

    return min(

        root.val,

        minimum(root.left),

        minimum(root.right)

    )
```

Example

```
Minimum

=

1
```

---

# 13. Mirror (Invert Tree)

Swap every Left and Right child.

Before

```text
            1
          /   \
         2     3
        / \
       4   5
```

After

```text
            1
          /   \
         3     2
              / \
             5   4
```

---

Python

```python
def invert(root):

    if root is None:
        return None

    root.left, root.right = root.right, root.left

    invert(root.left)

    invert(root.right)

    return root
```

---

Time Complexity

```
O(N)
```

---

# 14. Copying a Tree

Create an entirely new tree with the same structure.

Python

```python
def copyTree(root):

    if root is None:
        return None

    node = TreeNode(root.val)

    node.left = copyTree(root.left)

    node.right = copyTree(root.right)

    return node
```

---

# 15. Comparing Two Trees

Two trees are identical if

- Structure is same
- Values are same

Python

```python
def isSame(root1, root2):

    if root1 is None and root2 is None:
        return True

    if root1 is None or root2 is None:
        return False

    return (

        root1.val == root2.val

        and

        isSame(root1.left, root2.left)

        and

        isSame(root1.right, root2.right)

    )
```

---

# 16. Time Complexity

| Operation | Complexity |
|------------|------------|
| Search | O(N) |
| Insert | O(N) |
| Delete | O(N) |
| Height | O(N) |
| Count Nodes | O(N) |
| Count Leaves | O(N) |
| Count Internal Nodes | O(N) |
| Maximum | O(N) |
| Minimum | O(N) |
| Mirror | O(N) |
| Copy Tree | O(N) |
| Compare Trees | O(N) |

---

# 17. Common Interview Questions

## Search

- Search in Binary Tree

---

## Height

- Maximum Depth
- Minimum Depth

---

## Counting

- Count Complete Tree Nodes
- Count Good Nodes
- Count Leaves

---

## Mirror

- Invert Binary Tree

---

## Comparison

- Same Tree
- Symmetric Tree
- Subtree of Another Tree

---

## Others

- Diameter of Binary Tree
- Balanced Binary Tree
- Maximum Path Sum
- Binary Tree Tilt
- Sum of Left Leaves
- Find Bottom Left Tree Value

---

# 18. Summary

| Operation | Purpose |
|-----------|---------|
| Create | Build a tree |
| Search | Find a value |
| Insert | Add a node |
| Delete | Remove a node |
| Height | Longest root-to-leaf path |
| Count Nodes | Total nodes |
| Count Leaves | Nodes with no children |
| Count Internal | Nodes with children |
| Maximum | Largest value |
| Minimum | Smallest value |
| Mirror | Swap left and right |
| Copy | Duplicate tree |
| Compare | Check if two trees are identical |

---

# 📝 Quick Revision

```text
Tree Operations
│
├── Create Tree
├── Search
├── Insert
├── Delete
├── Height
├── Count Nodes
├── Count Leaves
├── Count Internal Nodes
├── Maximum
├── Minimum
├── Mirror (Invert)
├── Copy Tree
└── Compare Trees
```

---

# 💡 Key Takeaways

- Almost every tree operation uses **DFS recursion**.
- **Height, Count, Mirror, Copy, Compare** all follow the same recursive pattern:
  1. Handle the base case (`root is None`).
  2. Recursively solve the left subtree.
  3. Recursively solve the right subtree.
  4. Combine the results.
- For **Binary Trees**, search, insert, and delete generally take **O(N)** because there is no ordering.
- These operations are the foundation for advanced tree problems like **Diameter**, **Balanced Tree**, **LCA**, and **Maximum Path Sum**.