# Sliding Window Operations

*A Complete Step-by-Step Guide to Building and Moving a Sliding Window*

> **Prerequisites:** Before reading this document, you should understand the basics of the Sliding Window technique, including what a window is, the difference between fixed and variable windows, and where Sliding Window is applicable.

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Understanding Window Operations](#2-understanding-window-operations)
* [3. Operation 1 – Window Creation](#3-operation-1--window-creation)
* [4. Operation 2 – Window Expansion](#4-operation-2--window-expansion)
* [5. Operation 3 – Add an Element](#5-operation-3--add-an-element)
* [6. Operation 4 – Validate the Window](#6-operation-4--validate-the-window)
* [7. Operation 5 – Window Shrinking](#7-operation-5--window-shrinking)
* [8. Operation 6 – Remove an Element](#8-operation-6--remove-an-element)
* [9. Operation 7 – Update the Answer](#9-operation-7--update-the-answer)
* [10. Window Movement Rules](#10-window-movement-rules)
* [11. Universal Sliding Window Flow](#11-universal-sliding-window-flow)
* [12. Fixed Window Operations](#12-fixed-window-operations)
* [13. Variable Window Operations](#13-variable-window-operations)
* [14. Complete Dry Run](#14-complete-dry-run)
* [15. Common Mistakes](#15-common-mistakes)
* [16. Sliding Window Operation Cheat Sheet](#16-sliding-window-operation-cheat-sheet)

---

# 1. Introduction

Sliding Window is not just an algorithm—it is a sequence of operations.

Whenever you solve a Sliding Window problem, you repeatedly perform the same set of operations:

1. Create a window
2. Expand the window
3. Add the new element
4. Check whether the window is valid
5. Shrink the window if necessary
6. Remove the old element
7. Update the answer
8. Repeat

Once you master these operations, almost every Sliding Window problem becomes much easier.

---

# 2. Understanding Window Operations

Suppose we have the following array:

```
Index

0 1 2 3 4 5 6

Array

2 5 1 8 3 9 4
```

Initially there is **no window**.

```
2 5 1 8 3 9 4
```

Our goal is to gradually build a window.

Eventually it may become

```
[2 5 1]
```

Then

```
[2 5 1 8]
```

Then

```
[5 1 8]
```

Notice that the window keeps moving.

This movement is called **Sliding**.

---

# 3. Operation 1 – Window Creation

## Purpose

Create the very first window.

Every Sliding Window algorithm starts by placing two pointers at the beginning.

```
left = 0
right = 0
```

Visual Representation

```
Array

2 5 1 8 3

^

L,R
```

Both pointers point to the first element.

The window currently contains

```
[2]
```

At this stage:

* Left Boundary = 0
* Right Boundary = 0
* Window Size = 1

This is called **Window Initialization**.

---

# 4. Operation 2 – Window Expansion

## Purpose

Increase the size of the window.

Whenever we move the **right pointer**, the window grows.

```
right += 1
```

Example

Before

```
[2]
```

After expanding

```
[2 5]
```

Expand again

```
[2 5 1]
```

Expand again

```
[2 5 1 8]
```

Notice that only the **right pointer** moves.

Expansion always increases the number of elements inside the window.

---

# 5. Operation 3 – Add an Element

Whenever the window expands, a new element enters the window.

That element must now contribute to whatever information we are maintaining.

Depending on the problem, we may update:

### Running Sum

```
Current Sum

10

↓

Add new element

+8

↓

18
```

### Frequency Map

```
Before

a : 2

b : 1

New Character

a

After

a : 3

b : 1
```

### Count

```
Odd Count

2

↓

New Odd Number

↓

3
```

### Maximum

```
Current Maximum

9

↓

New Value

12

↓

Maximum becomes 12
```

Whenever **right moves**, remember:

> **Add the contribution of the new element.**

---

# 6. Operation 4 – Validate the Window

Now check whether the current window satisfies the problem's condition.

Different problems have different conditions.

Examples:

```
Window Size == K
```

```
Sum >= Target
```

```
Distinct Characters <= K
```

```
Zeros <= 2
```

```
Duplicate Characters == 0
```

A window that satisfies the condition is called a **Valid Window**.

Otherwise it is an **Invalid Window**.

Visual Example

Valid

```
[2 5 1]
```

Invalid

```
[2 5 1 8 9]
```

If the window becomes invalid, we cannot continue expanding until it becomes valid again.

---

# 7. Operation 5 – Window Shrinking

If the window is invalid, reduce its size.

Move the left pointer.

```
left += 1
```

Before

```
[2 5 1 8]
```

Shrink

```
[5 1 8]
```

Shrink Again

```
[1 8]
```

Notice:

Only the **left pointer** moves.

The right pointer never moves backward.

Shrinking makes the window smaller until it becomes valid again.

---

# 8. Operation 6 – Remove an Element

Before moving the left pointer, the element leaving the window must be removed from our calculations.

Suppose the current sum is

```
25
```

Left element

```
5
```

Remove it

```
25 - 5 = 20
```

For a frequency map

Before

```
a : 3
```

Remove one

```
a : 2
```

Always remember:

**Remove first.**

**Move the left pointer second.**

Removing after moving the pointer often causes incorrect results.

---

# 9. Operation 7 – Update the Answer

Whenever the window satisfies the condition, record the answer.

Depending on the problem, the answer may represent:

* Maximum length
* Minimum length
* Maximum sum
* Minimum sum
* Number of valid windows

Example

Current Best

```
12
```

Current Window

```
18
```

Update

```
Best = 18
```

Another Example

Current Best Length

```
8
```

Current Window Length

```
5
```

Minimum becomes

```
5
```

Always update the answer **only when the window is valid**.

---

# 10. Window Movement Rules

These rules never change.

## Rule 1

The right pointer only moves forward.

```
→ → →
```

Never

```
←
```

---

## Rule 2

The left pointer only moves forward.

```
→ → →
```

Never

```
←
```

---

## Rule 3

The window never jumps.

Correct

```
[2]

↓

[2 5]

↓

[2 5 1]
```

Incorrect

```
[2]

↓

[2 5 1 8]
```

Sliding Window grows or shrinks one step at a time.

---

## Rule 4

Every element enters the window exactly once.

```
↓

Enter

↓

Leave
```

---

## Rule 5

Every element leaves the window at most once.

Because of this, the overall complexity remains **O(N)**.

---

# 11. Universal Sliding Window Flow

Every Sliding Window algorithm follows this sequence.

```
Start

↓

Create Window

↓

Expand Right

↓

Add Element

↓

Validate Window

↓

Window Valid ?

      |
  +---+---+
  |       |
 Yes      No
  |       |
Update   Remove Left Element
Answer       ↓
  |      Shrink Window
  |          ↓
  +----------+
        |
     Repeat
        |
        ↓
      Finish
```

This flow is the backbone of almost every Sliding Window solution.

---

# 12. Fixed Window Operations

In a **Fixed Window**, the window size never changes.

Example

Window Size = 3

```
[2 5 1]

↓

[5 1 8]

↓

[1 8 3]
```

Flow

```
Create Window

↓

Expand

↓

Window Size == K ?

↓

Yes

↓

Update Answer

↓

Remove Left Element

↓

Move Left

↓

Continue
```

The window grows until it reaches size **K**. After that, every new element added requires one old element to be removed.

---

# 13. Variable Window Operations

In a **Variable Window**, the window size changes based on a condition.

Example

```
[2]

↓

[2 5]

↓

[2 5 1]

↓

[2 5 1 8]

↓

Condition Breaks

↓

Shrink

↓

[5 1 8]
```

Flow

```
Expand Window

↓

Add Element

↓

Condition Valid ?

      |
  +---+---+
  |       |
 Yes      No
  |       |
Update   Remove
Answer   Left
  |       |
  +-------+
      |
Expand Again
```

Unlike a Fixed Window, the size is not constant.

---

# 14. Complete Dry Run

Suppose we have

```
Array

2 4 6 8 10
```

Initial State

```
L,R

↓

2 4 6 8 10

Window

[2]
```

Expand

```
[2 4]
```

Expand

```
[2 4 6]
```

Condition satisfied.

Update answer.

Expand again.

```
[2 4 6 8]
```

Condition becomes invalid.

Remove left element.

```
[4 6 8]
```

Update answer.

Expand again.

```
[4 6 8 10]
```

Continue until the right pointer reaches the end of the array.

This same pattern appears in almost every Sliding Window problem.

---

# 15. Common Mistakes

### ❌ Forgetting to remove the left element

Removing the pointer without updating the maintained information leads to incorrect results.

---

### ❌ Updating the answer before validating the window

Always validate the window first.

---

### ❌ Shrinking too early

Do not shrink unless the condition requires it.

---

### ❌ Using the wrong window size

Correct formula:

```
Window Size = right - left + 1
```

---

### ❌ Moving pointers backward

Sliding Window pointers only move forward.

---

### ❌ Forgetting to update auxiliary data

If you maintain a frequency map, running sum, count, or maximum, update it whenever an element enters or leaves the window.

---

# 16. Sliding Window Operation Cheat Sheet

```
1. Create Window

left = 0
right = 0

↓

2. Expand Window

Move right pointer

↓

3. Add Element

Include nums[right]

↓

4. Validate Window

Check the problem condition

↓

5. Invalid?

Yes

↓

6. Remove Left Element

↓

7. Shrink Window

Move left pointer

↓

8. Window Becomes Valid

↓

9. Update Answer

↓

10. Repeat Until Right Reaches End
```

---

# Summary

Every Sliding Window algorithm is built from the same small set of operations:

* Create the window.
* Expand by moving the right pointer.
* Add the new element.
* Validate the window.
* Shrink by moving the left pointer when necessary.
* Remove the outgoing element.
* Update the answer.
* Repeat until the array or string has been completely processed.

Once these operations become second nature, solving Fixed Window, Variable Window, and advanced Sliding Window problems becomes much easier because the underlying process never changes—only the condition being checked does.
