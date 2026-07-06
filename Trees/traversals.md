# 🌳 Binary Tree Traversals

> Traversal means **visiting every node of a tree exactly once** in a specific order.

Traversals are the foundation of almost every Binary Tree problem. Before solving problems like Height, Diameter, Maximum Path Sum, or Lowest Common Ancestor (LCA), you must understand how traversals work.

---

# 📚 Table of Contents

1. [What is Tree Traversal?](#1-what-is-tree-traversal)
2. [Types of Traversals](#2-types-of-traversals)
3. [Depth First Search (DFS)](#3-depth-first-search-dfs)
4. [Breadth First Search (BFS)](#7-breadth-first-search-bfs)
5. [Preorder Traversal](#4-preorder-traversal)
6. [Inorder Traversal](#5-inorder-traversal)
7. [Postorder Traversal](#6-postorder-traversal)
8. [Level Order Traversal](#7-breadth-first-search-bfs)
9. [DFS vs BFS](#8-dfs-vs-bfs)
10. [Recursive vs Iterative Traversal](#9-recursive-vs-iterative)
11. [Which Traversal Should I Use?](#11-which-traversal-should-i-use)
12. [Time & Space Complexity](#10-time-complexity)
13. [Interview Tips](#12-interview-pattern)
14. [Practice Problems](#13-common-leetcode-problems)

---

# 1. What is Tree Traversal?

Traversal means **visiting every node exactly once**.

Suppose we have the following tree:

```text
            1
          /   \
         2     3
        / \   / \
       4   5 6   7
```

Although every traversal visits all **7 nodes**, the **order** in which they are visited is different.

---

# 2. Types of Traversals

Tree Traversals are divided into two major categories.

```text
Tree Traversal
│
├── Depth First Search (DFS)
│     ├── Preorder
│     ├── Inorder
│     └── Postorder
│
└── Breadth First Search (BFS)
      └── Level Order
```

---

# 3. Depth First Search (DFS)

DFS explores one branch completely before moving to another branch.

Imagine exploring a maze.

Instead of checking nearby paths first, you go as deep as possible.

```text
            1
          /   \
         2     3
        / \
       4   5
```

DFS visits nodes by moving downward first.

DFS has three different orders.

- Preorder
- Inorder
- Postorder

---

# Memory Trick

```text
Preorder

Root
 ↓
Left
 ↓
Right
```

```text
Inorder

Left
 ↓
Root
 ↓
Right
```

```text
Postorder

Left
 ↓
Right
 ↓
Root
```

Remember

```
Pre = Root First

In = Root Middle

Post = Root Last
```

---

# 4. Preorder Traversal

## Order

```text
Root → Left → Right
```

Example

```text
            1
          /   \
         2     3
        / \   / \
       4   5 6   7
```

### Step-by-Step

Visit Root

```
1
```

Go Left

```
2
```

Go Left Again

```
4
```

Backtrack

Visit Right

```
5
```

Go back

Visit Right Subtree

```
3
```

Visit Left

```
6
```

Visit Right

```
7
```

Final Traversal

```text
1 2 4 5 3 6 7
```

---

## Recursive Code

```python
def preorder(root):

    if root is None:
        return

    print(root.val)

    preorder(root.left)

    preorder(root.right)
```

---

## Dry Run

```text
preorder(1)

↓

print(1)

↓

preorder(2)

↓

print(2)

↓

preorder(4)

↓

print(4)

↓

None

↓

None

↓

preorder(5)

↓

print(5)

↓

...

Result

1 2 4 5 3 6 7
```

---

## When is Preorder Used?

- Copy Tree
- Serialize Tree
- Prefix Expression
- Construct Tree
- Printing Tree Structure

---

# 5. Inorder Traversal

## Order

```text
Left → Root → Right
```

Example

```text
            1
          /   \
         2     3
        / \   / \
       4   5 6   7
```

Traversal

```text
4 2 5 1 6 3 7
```

---

## Recursive Code

```python
def inorder(root):

    if root is None:
        return

    inorder(root.left)

    print(root.val)

    inorder(root.right)
```

---

## Why is Inorder Special?

If the tree is a **Binary Search Tree**, Inorder Traversal gives the values in **sorted order**.

Example BST

```text
          8
        /   \
       4     10
      / \      \
     2   6      15
```

Inorder

```text
2 4 6 8 10 15
```

Sorted!

---

## Applications

- BST Problems
- Validate BST
- Kth Smallest Element
- Convert BST

---

# 6. Postorder Traversal

## Order

```text
Left → Right → Root
```

Traversal

```text
4 5 2 6 7 3 1
```

---

## Recursive Code

```python
def postorder(root):

    if root is None:
        return

    postorder(root.left)

    postorder(root.right)

    print(root.val)
```

---

## Why is Postorder Important?

Parent is processed only after both children are processed.

This is useful whenever the answer depends on child nodes.

Examples

- Height
- Diameter
- Maximum Path Sum
- Balanced Tree
- Delete Tree
- Tree Dynamic Programming

---

# 7. Breadth First Search (BFS)

Unlike DFS,

BFS explores nodes **level by level**.

It uses a **Queue**.

```text
            1
          /   \
         2     3
        / \   / \
       4   5 6   7
```

Traversal

```text
1 2 3 4 5 6 7
```

---

## Algorithm

```
Put Root into Queue

Repeat until Queue becomes empty

Remove Front Node

Visit It

Insert Left Child

Insert Right Child
```

---

## Visualization

Queue

```
[1]
```

Visit

```
1
```

Queue

```
[2 3]
```

Visit

```
2
```

Queue

```
[3 4 5]
```

Visit

```
3
```

Queue

```
[4 5 6 7]
```

Continue...

---

## Python Code

```python
from collections import deque

def levelOrder(root):

    if root is None:
        return

    q = deque([root])

    while q:

        node = q.popleft()

        print(node.val)

        if node.left:
            q.append(node.left)

        if node.right:
            q.append(node.right)
```

---

## Applications

- Level Order Traversal
- Zigzag Traversal
- Right Side View
- Left Side View
- Minimum Depth
- Average of Levels
- Cousins in Binary Tree

---

# 8. DFS vs BFS

| Feature | DFS | BFS |
|----------|-----|-----|
| Full Form | Depth First Search | Breadth First Search |
| Data Structure | Stack / Recursion | Queue |
| Visits | Deep First | Level First |
| Memory | Less | More |
| Good For | Recursive Problems | Level Problems |

---

# 9. Recursive vs Iterative

## Recursive

Uses function calls.

```python
preorder(root.left)
```

Pros

- Short
- Easy

Cons

- Stack Overflow on deep trees

---

## Iterative

Uses Stack or Queue.

Pros

- Faster in interviews
- No recursion limit

Cons

- Slightly harder to write

---

# 10. Time Complexity

Suppose there are **N nodes**.

Every traversal visits every node exactly once.

| Traversal | Time | Space |
|------------|------|-------|
| Preorder | O(N) | O(H) |
| Inorder | O(N) | O(H) |
| Postorder | O(N) | O(H) |
| Level Order | O(N) | O(W) |

Where

```
H = Height of Tree

W = Maximum Width
```

Worst Case

```
O(N)
```

---

# 11. Which Traversal Should I Use?

| Situation | Traversal |
|-----------|-----------|
| Visit Parent First | Preorder |
| BST Problems | Inorder |
| Child Before Parent | Postorder |
| Level-wise Problems | BFS |

---

# 12. Interview Pattern

Whenever you see a tree problem, ask these questions.

### Question 1

Do I need to visit nodes level by level?

→ BFS

---

### Question 2

Does parent depend on children?

→ Postorder

---

### Question 3

Do I need sorted values?

→ Inorder

---

### Question 4

Do I need to process root first?

→ Preorder

---

# 13. Common LeetCode Problems

## Preorder

- Binary Tree Preorder Traversal
- Flatten Binary Tree
- Construct String from Binary Tree

---

## Inorder

- Binary Tree Inorder Traversal
- Validate BST
- Kth Smallest Element in BST

---

## Postorder

- Binary Tree Postorder Traversal
- Diameter of Binary Tree
- Maximum Path Sum
- Balanced Binary Tree
- Binary Tree Tilt

---

## BFS

- Binary Tree Level Order Traversal
- Binary Tree Right Side View
- Average of Levels
- Binary Tree Zigzag Level Order Traversal
- Find Bottom Left Tree Value

---

# 14. Cheat Sheet

```text
PREORDER

Root
↓

Left
↓

Right
```

```text
INORDER

Left
↓

Root
↓

Right
```

```text
POSTORDER

Left
↓

Right
↓

Root
```

```text
LEVEL ORDER

Level 1

↓

Level 2

↓

Level 3
```

---

# Quick Revision

```
Traversal
│
├── DFS
│   ├── Preorder
│   ├── Inorder
│   └── Postorder
│
└── BFS
    └── Level Order
```

Remember:

```
Root First  → Preorder

Root Middle → Inorder

Root Last   → Postorder

Level by Level → BFS
```