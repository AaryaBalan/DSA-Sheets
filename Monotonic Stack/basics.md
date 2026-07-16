# 📚 Monotonic Stack in Data Structures & Algorithms

> Monotonic Stack is one of the most powerful optimization techniques in Data Structures & Algorithms. It is mainly used to solve problems involving the nearest greater/smaller elements, boundaries, and contribution-based calculations. Instead of repeatedly scanning an array, a Monotonic Stack maintains only the useful elements, reducing many O(N²) solutions to O(N).

---

# 📚 Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Introduction to Monotonic Stack](#2-introduction-to-monotonic-stack)
3. [Why Monotonic Stack?](#3-why-monotonic-stack)
4. [What is a Monotonic Stack?](#4-what-is-a-monotonic-stack)
5. [How Does a Monotonic Stack Work?](#5-how-does-a-monotonic-stack-work)
6. [Why Do We Pop Elements?](#6-why-do-we-pop-elements)
7. [Types of Monotonic Stack](#7-types-of-monotonic-stack)
8. [Monotonic Increasing Stack](#71-monotonic-increasing-stack)
9. [Monotonic Decreasing Stack](#72-monotonic-decreasing-stack)
10. [Traversal Direction](#8-traversal-direction)
11. [Why Store Indices Instead of Values?](#9-why-store-indices-instead-of-values)
12. [Comparison Operators](#10-comparison-operators)
13. [Time and Space Complexity](#11-time-and-space-complexity)
14. [When Should You Think of Monotonic Stack?](#12-when-should-you-think-of-monotonic-stack)
15. [Common Beginner Mistakes](#13-common-beginner-mistakes)
16. [Summary](#14-summary)

---

# 1. Prerequisites

Before learning the Monotonic Stack technique, make sure you understand the following concepts.

## 1.1 Arrays

- Traversing arrays using loops
- Indexing
- Accessing elements
- Understanding neighboring elements

---

## 1.2 Stack

You should know:

- Push
- Pop
- Peek / Top
- Empty Stack
- LIFO Principle

---

## 1.3 Time Complexity

Understand the difference between

```
O(N²)

vs

O(N)
```

because the entire purpose of Monotonic Stack is optimization.

---

## 1.4 Basic Python Stack

```python
stack = []

stack.append(10)

stack.pop()

stack[-1]
```

---

# 2. Introduction to Monotonic Stack

A **Monotonic Stack** is **not a new data structure**.

It is simply a **normal Stack** that follows an additional rule.

Instead of storing elements in random order, it always maintains them in either

- Increasing order
- Decreasing order

while processing an array.

The word **Monotonic** means

```
Always moving in one direction.
```

For example,

Increasing

```
1 3 5 8 10
```

Decreasing

```
10 8 7 5 2
```

The stack continuously removes elements that break this ordering.

---

# 3. Why Monotonic Stack?

Consider an array.

```
5 2 8 4 9
```

Suppose we want to repeatedly know information about neighboring elements.

Without optimization,

we may repeatedly scan left or right.

```
Element

↓

Scan

↓

Scan

↓

Scan
```

This leads to

```
O(N²)
```

Instead,

Monotonic Stack keeps only useful elements.

Every useless element is immediately removed.

Therefore,

every element is processed only once.

Overall complexity becomes

```
O(N)
```

---

# 4. What is a Monotonic Stack?

Definition

A Monotonic Stack is a stack whose elements are always maintained in sorted order while traversing the array.

The stack never violates its ordering.

Whenever a new element breaks the ordering,

elements are removed until the ordering becomes valid again.

---

# 5. How Does a Monotonic Stack Work?

Every Monotonic Stack algorithm follows three simple steps.

```
Current Element

↓

Compare with Stack Top

↓

Pop Invalid Elements

↓

Push Current Element
```

These three steps are repeated for every element.

---

# 6. Why Do We Pop Elements?

This is the most important concept.

When a new element arrives,

some older elements become permanently useless.

Those elements can never help answer future queries.

Instead of keeping them,

we immediately remove them.

This keeps the stack clean and efficient.

Remember:

```
Pop

≠

Wrong Element

Pop

=

No Longer Useful Element
```

---

# 7. Types of Monotonic Stack

There are two types.

---

## 7.1 Monotonic Increasing Stack

The stack maintains increasing order.

```
Bottom

1

3

5

8

Top
```

If a smaller element arrives,

```
Incoming

↓

2
```

then

```
8 removed

5 removed

3 stays

2 inserted
```

The stack remains increasing.

---

## 7.2 Monotonic Decreasing Stack

The stack maintains decreasing order.

```
Bottom

9

7

5

2

Top
```

If a larger element arrives,

```
Incoming

↓

8
```

then

```
2 removed

5 removed

7 removed

8 inserted
```

Again,

the ordering is preserved.

---

# 8. Traversal Direction

Traversal depends on what information you need.

Generally,

```
Previous Information

↓

Left → Right
```

```
Next Information

↓

Right → Left
```

Choosing the correct direction allows previously processed elements to provide the required information.

---

# 9. Why Store Indices Instead of Values?

Interview solutions almost always store indices.

Example

Instead of

```python
stack.append(nums[i])
```

we store

```python
stack.append(i)
```

Why?

Indices allow us to

- Access values
- Calculate distances
- Find widths
- Handle duplicates
- Locate boundaries

One index gives both position and value.

---

# 10. Comparison Operators

Different problems require different comparisons.

You will commonly see

```
>

>=

<

<=
```

The choice mainly depends on

- duplicate values
- maintaining a valid ordering
- preventing double counting

Choosing the correct comparison is essential.

---

# 11. Time and Space Complexity

## Time Complexity

Although we often have

```
for

↓

while
```

the complexity is still

```
O(N)
```

because

every element

- enters the stack once
- leaves the stack once

Maximum operations

```
Push

N

+

Pop

N

=

2N
```

Therefore,

```
O(N)
```

---

## Space Complexity

Worst case

```
O(N)
```

because every element may remain inside the stack.

---

# 12. When Should You Think of Monotonic Stack?

Common clues include:

- Nearest element
- Previous element
- Next element
- First greater
- First smaller
- Boundary
- Span
- Dominating element

These often indicate that repeatedly scanning the array is inefficient and a Monotonic Stack may help.

---

# 13. Common Beginner Mistakes

- Thinking Monotonic Stack is a new data structure.
- Forgetting that it is still just a normal stack.
- Storing values when indices are needed.
- Forgetting why elements are popped.
- Assuming nested loops automatically mean O(N²).
- Mixing increasing and decreasing stack logic.
- Using the wrong traversal direction.

---

# 14. Summary

| Concept | Description |
|----------|-------------|
| Data Structure | Normal Stack |
| Technique | Maintain sorted order |
| Types | Increasing, Decreasing |
| Main Purpose | Remove useless elements |
| Time Complexity | O(N) |
| Space Complexity | O(N) |
| Stores | Usually Indices |
| Used For | Nearest / Previous / Next / Boundary Problems |

---

# 📝 Quick Revision

```text
Normal Stack
        ↓
Stores everything
        ↓
Repeated Searching
        ↓
O(N²)

────────────────────────

Monotonic Stack
        ↓
Keep only useful elements
        ↓
Remove useless elements immediately
        ↓
Maintain Increasing / Decreasing Order
        ↓
Each element
├── Push once
└── Pop once
        ↓
O(N)
```