# 1008. Construct Binary Search Tree from Preorder Traversal Medium

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
