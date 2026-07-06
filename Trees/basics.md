# 🌳 Trees in Data Structures & Algorithms

> Trees are one of the most important data structures in DSA. Almost every interview contains at least one tree problem. Before solving tree questions, you should understand the basic terminology, different types of trees, and their properties.

---

# 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Why Trees?](#2-why-trees)
3. [Basic Structure of a Tree](#3-basic-structure-of-a-tree)
4. [Tree Terminology](#4-tree-terminology)
5. [Types of Trees](#5-types-of-trees)
6. [Binary Tree](#52-binary-tree)
7. [Binary Search Tree (BST)](#59-binary-search-tree-bst)
8. [Properties of Trees](#6-properties-of-trees)
9. [Binary Tree Representation](#7-binary-tree-representation)
10. [Summary](#8-summary)

---

# 1. Introduction

A **Tree** is a **non-linear hierarchical data structure** that consists of nodes connected by edges.

Unlike arrays and linked lists, data in a tree is organized in a **parent-child relationship**.

A tree starts with one node called the **Root**, and every other node is connected directly or indirectly to it.

Example:

```text
          A
        /   \
       B     C
      / \   / \
     D   E F   G
```

Here,

- A is the Root
- B and C are children of A
- D, E, F, and G are leaf nodes

---

# 2. Why Trees?

Trees are used whenever data naturally forms a hierarchy.

Some real-world examples are:

```
Computer
│
├── Documents
│   ├── Resume.pdf
│   └── Notes.docx
│
├── Downloads
│   ├── Image.png
│   └── Movie.mp4
│
└── Desktop
```

Other applications include:

- File Systems
- HTML DOM
- Organization Charts
- Database Indexing
- Search Engines
- AI Decision Trees
- Game Trees
- Routing Algorithms

---

# 3. Basic Structure of a Tree

Example:

```text
               A
             /   \
            B     C
           / \   / \
          D   E F   G
```

Every circle is called a **Node**.

Every connecting line is called an **Edge**.

---

# 4. Tree Terminology

## 4.1 Node

A single element in a tree.

```text
A
```

Every value stored in a tree is called a node.

---

## 4.2 Edge

An edge connects two nodes.

```text
A ----- B
```

One connection equals one edge.

---

## 4.3 Root

The topmost node of a tree.

```text
      A
     / \
    B   C
```

Root = **A**

Every tree has exactly one root.

---

## 4.4 Parent

A node that has one or more children.

```text
      A
     /
    B
```

Parent of B is A.

---

## 4.5 Child

A node connected below another node.

```text
      A
     /
    B
```

Child of A is B.

---

## 4.6 Siblings

Nodes having the same parent.

```text
      A
     / \
    B   C
```

B and C are siblings.

---

## 4.7 Leaf Node

A node having **no children**.

```text
        A
       / \
      B   C
         /
        D
```

Leaf Nodes:

- B
- D

---

## 4.8 Internal Node

Any node that has at least one child.

```text
      A
     /
    B
   /
  C
```

Internal Nodes:

- A
- B

---

## 4.9 Ancestors

All nodes above a given node.

```text
A
|
B
|
C
```

Ancestors of C:

- A
- B

---

## 4.10 Descendants

All nodes below a given node.

```text
A
|
B
|
C
```

Descendants of A:

- B
- C

---

## 4.11 Degree of a Node

The number of children a node has.

Example:

```text
        A
      / | \
     B  C  D
```

Degree(A) = 3

Degree(B) = 0

---

## 4.12 Degree of a Tree

The maximum degree of any node in the tree.

Example:

```text
        A
      / | \
     B  C  D
```

Maximum degree = 3

---

## 4.13 Path

A sequence of connected nodes.

```text
A → B → D
```

Path Length = Number of edges.

```
A → B → D

Length = 2
```

---

## 4.14 Depth of a Node

The number of edges from the root to the node.

Example:

```text
A
|
B
|
C
|
D
```

| Node | Depth |
|------|------|
| A | 0 |
| B | 1 |
| C | 2 |
| D | 3 |

---

## 4.15 Height of a Node

The number of edges from that node to its deepest leaf.

Example:

```text
        A
       /
      B
     /
    C
```

Height(C) = 0

Height(B) = 1

Height(A) = 2

---

## 4.16 Height of a Tree

The longest path from the root to any leaf.

Example:

```text
        A
      /   \
     B     C
    /
   D
```

Longest path:

```
A → B → D
```

Height = **2**

---

## 4.17 Level of a Node

Level = Depth + 1

Example:

```text
        A
      /   \
     B     C
    / \
   D   E
```

| Level | Nodes |
|-------|------|
| 1 | A |
| 2 | B C |
| 3 | D E |

---

# 5. Types of Trees

There are many types of trees.

---

## 5.1 General Tree

A node can have any number of children.

```text
            A
        /   |   \
       B    C    D
      / \
     E   F
```

Maximum children = Unlimited

---

## 5.2 Binary Tree

A node can have **at most two children**.

```text
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

Each node can have:

- 0 children
- 1 child
- 2 children

Never more than two.

---

## 5.3 Full Binary Tree

Every node has either:

- 0 children
- 2 children

Valid:

```text
        1
      /   \
     2     3
```

Also Valid:

```text
         1
       /   \
      2     3
     / \
    4   5
```

Not Full:

```text
      1
     /
    2
```

Because node 1 has only one child.

---

## 5.4 Complete Binary Tree

Every level is completely filled except possibly the last level.

The last level is filled from **left to right**.

Example:

```text
        1
      /   \
     2     3
    / \   /
   4   5 6
```

Valid Complete Binary Tree

Not Complete:

```text
        1
      /   \
     2     3
      \     \
       5     7
```

There are gaps before nodes.

---

## 5.5 Perfect Binary Tree

Every internal node has exactly two children.

All leaf nodes are on the same level.

```text
            1
         /     \
        2       3
      /  \    /  \
     4   5   6    7
```

All leaves are at Level 3.

---

## 5.6 Balanced Binary Tree

For every node,

```
Height Difference

| Left Height - Right Height | ≤ 1
```

Example:

```text
        1
      /   \
     2     3
    /
   4
```

Balanced.

Not Balanced:

```text
        1
       /
      2
     /
    3
   /
  4
```

The left subtree is much deeper.

---

## 5.7 Degenerate Tree

Every parent has only one child.

Looks exactly like a linked list.

```text
1
 \
 2
  \
   3
    \
     4
```

Searching becomes slow.

---

## 5.8 Skewed Tree

A special type of degenerate tree.

### Left Skewed

```text
      4
     /
    3
   /
  2
 /
1
```

### Right Skewed

```text
1
 \
 2
  \
   3
    \
     4
```

---

## 5.9 Binary Search Tree (BST)

A Binary Search Tree follows one rule:

```
Left Subtree < Root < Right Subtree
```

Example:

```text
          8
        /   \
       4     10
      / \      \
     2   6      15
```

Searching is much faster than a normal binary tree.

---

# 6. Properties of Trees

For a tree having **N nodes**

### Number of Edges

```
Edges = N - 1
```

Example

```
Nodes = 8

Edges = 7
```

---

### Maximum Nodes at Level L

```
2^L
```

(Level starts from 0)

Example

| Level | Maximum Nodes |
|--------|--------------|
| 0 | 1 |
| 1 | 2 |
| 2 | 4 |
| 3 | 8 |

---

### Maximum Nodes in Height h

```
2^(h+1) - 1
```

Example

Height = 3

```
2^(3+1)-1

16-1

15 Nodes
```

---

### Minimum Height

Balanced Tree

```
log₂(N)
```

---

# 7. Binary Tree Representation

## Node Representation

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

# 8. Summary

| Term | Meaning |
|------|---------|
| Root | Topmost node |
| Parent | Node having children |
| Child | Node below parent |
| Leaf Node | Node with no children |
| Internal Node | Node having children |
| Edge | Connection between two nodes |
| Degree | Number of children |
| Height | Longest path to a leaf |
| Depth | Distance from root |
| Level | Depth + 1 |
| Ancestors | Nodes above |
| Descendants | Nodes below |

---

# 📝 Quick Revision

```
General Tree
        ↓
Binary Tree
        ↓
Binary Search Tree
        ↓
Traversals
        ↓
DFS
├── Preorder
├── Inorder
└── Postorder

BFS
└── Level Order

Advanced Trees
├── AVL Tree
├── Red Black Tree
├── Segment Tree
├── Fenwick Tree
├── Trie
├── Heap
├── N-ary Tree
└── B Tree
```