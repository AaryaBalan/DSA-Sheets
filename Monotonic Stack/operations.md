# 📖 Common Monotonic Stack Operations

These four operations are the **building blocks** of almost every monotonic stack problem.

```
                Monotonic Stack
                      │
        ┌─────────────┴─────────────┐
        │                           │
    Smaller                     Greater
        │                           │
   ┌────┴────┐                ┌─────┴─────┐
   │         │                │           │
Previous   Next          Previous      Next
Smaller    Smaller       Greater       Greater
(PSE)      (NSE)         (PGE)         (NGE)
```

---

# 📚 Table of Contents

1. [What are these operations?](#what-are-these-operations)
2. [1️⃣ Previous Smaller Element (PSE)](#1️⃣-previous-smaller-element-pse)
3. [2️⃣ Next Smaller Element (NSE)](#2️⃣-next-smaller-element-nse)
4. [3️⃣ Previous Greater Element (PGE)](#3️⃣-previous-greater-element-pge)
5. [4️⃣ Next Greater Element (NGE)](#4️⃣-next-greater-element-nge)
6. [🎯 Why These Four?](#🎯-why-these-four)
7. [🧠 How to Remember the Traversal Direction](#🧠-how-to-remember-the-traversal-direction)
8. [🧠 How to Remember the Pop Conditions](#🧠-how-to-remember-the-pop-conditions)
9. [🔥 The Universal Mental Process](#🔥-the-universal-mental-process)

---

# What are these operations?

Suppose

```
nums = [3, 1, 2, 4]
```

```
Index

0 1 2 3
```

---

# 1️⃣ Previous Smaller Element (PSE)

## Definition

For every element,

> Find the **nearest smaller element on the LEFT**.

```
nums

3 1 2 4
```

Let's find PSE.

---

### For 3

Left side

```
Nothing
```

Answer

```
-1
```

---

### For 1

Left

```
3
```

Is 3 smaller?

No.

Answer

```
-1
```

---

### For 2

Left

```
3 1
```

Nearest smaller

```
1
```

---

### For 4

Left

```
3 1 2
```

Nearest smaller

```
2
```

---

Result

```
Value

3 1 2 4

PSE

-1 -1 1 2
```

---

## Python Code

```python
def previous_smaller(nums):
    stack = []
    pse = [-1] * len(nums)

    for i in range(len(nums)):

        while stack and nums[stack[-1]] > nums[i]:
            stack.pop()

        if stack:
            pse[i] = stack[-1]

        stack.append(i)

    return pse
```

---

## Dry Run

```
nums

3 1 2 4
```

### i = 0

Stack

```
[]
```

No previous smaller

```
pse[0] = -1
```

Push

```
[0]
```

---

### i = 1

Current

```
1
```

Top

```
3
```

3 > 1

Pop

```
[]
```

No previous smaller

```
pse[1] = -1
```

Push

```
[1]
```

---

### i = 2

Current

```
2
```

Top

```
1
```

1 < 2

Answer

```
1
```

Push

```
[1,2]
```

---

### i = 3

Current

```
4
```

Top

```
2
```

Answer

```
2
```

Done.

---

# 2️⃣ Next Smaller Element (NSE)

## Definition

Find the **nearest smaller element on the RIGHT**.

```
nums

3 1 2 4
```

---

For

```
3
```

Right

```
1 2 4
```

Nearest smaller

```
1
```

---

For

```
1
```

Nothing smaller

```
-1
```

---

For

```
2
```

Nothing smaller

```
-1
```

---

For

```
4
```

Nothing smaller

```
-1
```

---

Result

```
1 -1 -1 -1
```

---

## Python Code

```python
def next_smaller(nums):
    n = len(nums)

    stack = []

    nse = [-1] * n

    for i in range(n - 1, -1, -1):

        while stack and nums[stack[-1]] >= nums[i]:
            stack.pop()

        if stack:
            nse[i] = stack[-1]

        stack.append(i)

    return nse
```

---

## Why Right → Left?

Because

we need information from the RIGHT.

Those elements haven't been processed yet if we move left → right.

So we traverse backwards.

---

# 3️⃣ Previous Greater Element (PGE)

Definition

Nearest greater element on LEFT.

Example

```
nums

3 1 2 4
```

Answers

```
-1

3

3

-1
```

---

## Python

```python
def previous_greater(nums):

    stack=[]

    pge=[-1]*len(nums)

    for i in range(len(nums)):

        while stack and nums[stack[-1]] < nums[i]:
            stack.pop()

        if stack:
            pge[i]=stack[-1]

        stack.append(i)

    return pge
```

---

# Dry Run

Current

```
2
```

Left

```
3 1
```

Nearest greater

```
3
```

---

# 4️⃣ Next Greater Element (NGE)

Definition

Nearest greater element on RIGHT.

Example

```
nums

3 1 2 4
```

Answers

```
4

2

4

-1
```

---

## Python

```python
def next_greater(nums):

    n=len(nums)

    stack=[]

    nge=[-1]*n

    for i in range(n-1,-1,-1):

        while stack and nums[stack[-1]]<=nums[i]:
            stack.pop()

        if stack:
            nge[i]=stack[-1]

        stack.append(i)

    return nge
```

---

# 🎯 Why These Four?

Every monotonic stack problem is built from one or more of these operations.

```
                Monotonic Stack

                       │

        ┌──────────────┴──────────────┐

        │                             │

     Smaller                      Greater

        │                             │

   ┌────┴─────┐                 ┌─────┴─────┐

   │          │                 │           │

 Previous    Next           Previous      Next

 Smaller     Smaller        Greater       Greater

 (PSE)       (NSE)          (PGE)         (NGE)
```

---

# 🧠 How to Remember the Traversal Direction

| Operation        | Traverse     |
| ---------------- | ------------ |
| Previous Smaller | Left → Right |
| Previous Greater | Left → Right |
| Next Smaller     | Right → Left |
| Next Greater     | Right → Left |

**Reason:**

* **Previous** means the answer must come from elements you've already visited, so traverse **left → right**.
* **Next** means the answer is to the right, so traverse **right → left** to have that information available.

---

# 🧠 How to Remember the Pop Conditions

| Operation              | Pop While |
| ---------------------- | --------- |
| Previous Smaller (PSE) | `>`       |
| Next Smaller (NSE)     | `>=`      |
| Previous Greater (PGE) | `<`       |
| Next Greater (NGE)     | `<=`      |

The strict/non-strict difference (`>` vs `>=`, `<` vs `<=`) is mainly important when **duplicate values** exist. It ensures each duplicate is handled consistently and avoids double counting in contribution-based problems.

---

# 🔥 The Universal Mental Process

Whenever you're asked for any of these four operations, ask yourself these questions in order:

1. **Do I need Previous or Next?**

   * Previous → Traverse **Left → Right**
   * Next → Traverse **Right → Left**

2. **Do I need Smaller or Greater?**

   * Smaller → Maintain an **Increasing Stack**
   * Greater → Maintain a **Decreasing Stack**

3. **What breaks my stack's order?**

   * Pop elements that violate the required monotonic property.

If you can answer those three questions, you can derive the correct monotonic stack template instead of memorizing four separate pieces of code. This is the mindset interviewers are looking for.
