# 🪟 Sliding Window in Data Structures & Algorithms

> Sliding Window is an essential optimization technique in DSA, mainly used to reduce the time complexity of problems involving contiguous subarrays or substrings. Before solving Sliding Window questions, you should be comfortable with arrays, strings, and basic two-pointer concepts.

---

# 📚 Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Introduction to Sliding Window](#2-introduction-to-sliding-window)
3. [Why Sliding Window?](#3-why-sliding-window)
4. [How to Identify Sliding Window Problems](#4-how-to-identify-sliding-window-problems)
5. [Types of Sliding Window](#5-types-of-sliding-window)
6. [Fixed Size Window](#51-fixed-size-sliding-window)
7. [Variable Size Window](#52-variable-size-sliding-window)
8. [Time and Space Complexity](#6-time-and-space-complexity)
9. [Summary](#7-summary)

---

# 1. Prerequisites

Before diving into the Sliding Window technique, ensure you have a strong understanding of the following foundational concepts:

## 1.1 Arrays and Strings
- **Iteration:** Traversing elements using `for` or `while` loops.
- **Indexing:** Accessing elements efficiently using zero-based indexing.
- **Contiguous Nature:** Understanding that subarrays and substrings require elements to be adjacent.

## 1.2 Two Pointers Technique
- The Sliding Window is essentially an extension of the Two Pointers technique.
- Understanding how to use a `left` and `right` pointer to represent a range or bounds `[left, right]`.

## 1.3 Hashing and Frequency Maps
- **Hash Maps (Dictionaries):** Storing frequencies of elements or characters (e.g., `count[char] = frequency`).
- **Sets:** Keeping track of unique elements in a window.

## 1.4 Time Complexity (Big O Notation)
- Understanding the difference between `O(N^2)` (nested loops for generating all subarrays) and `O(N)` (linear time).

---

# 2. Introduction to Sliding Window

The **Sliding Window** technique is a computational technique that aims to reduce the use of nested loops and replace it with a single loop, thereby reducing the time complexity.

Imagine a "window" of a certain size moving from the beginning to the end of an array or string.

Example:
Given an array: `[1, 2, 3, 4, 5]` and a window size of `3`.

```text
Window 1: [1, 2, 3] 4, 5
Window 2: 1, [2, 3, 4] 5
Window 3: 1, 2, [3, 4, 5]
```

Instead of recalculating the sum of the elements in the window every time from scratch, we can simply subtract the element that is left behind and add the new element that comes into the window.

---

# 3. Why Sliding Window?

The primary reason to use the Sliding Window technique is **Optimization**.

### Without Sliding Window (Brute Force)
To find the maximum sum of a subarray of size `K`, you would typically use two nested loops:
- Outer loop to pick the starting point.
- Inner loop to traverse the next `K` elements.
- **Time Complexity:** `O(N * K)`

### With Sliding Window
You maintain a window of size `K` and slide it across the array.
- Calculate the sum of the first `K` elements.
- Move the window by adding the next element and removing the first element of the previous window.
- **Time Complexity:** `O(N)`

---

# 4. How to Identify Sliding Window Problems

Not all array/string problems can be solved using this technique. Look for these keywords and constraints in the problem statement:

1. **Input Data Structure:** The problem involves arrays, strings, or linked lists.
2. **Sub-structures:** You are asked to find a **Subarray** or **Substring** (must be contiguous).
3. **Objective:** You need to find the:
   - Maximum / Minimum (e.g., max sum, min length).
   - Longest / Shortest (e.g., longest substring without repeating characters).
   - Target Value (e.g., subarray with a given sum).
   - Count (e.g., number of anagrams).

---

# 5. Types of Sliding Window

There are two main variations of the Sliding Window technique:

---

## 5.1 Fixed Size Sliding Window

In this type, the size of the window (`K`) is explicitly given and remains constant throughout the process.

**Problem Example:** Find the maximum sum of any contiguous subarray of size `K`.

**Generic Template:**

```python
def fixed_window(arr, k):
    left = 0
    window_sum = 0
    max_sum = float('-inf')
    
    for right in range(len(arr)):
        # 1. Add current element to the window
        window_sum += arr[right]
        
        # 2. Check if the window size has been reached
        if right - left + 1 == k:
            # 3. Process the window (e.g., update max)
            max_sum = max(max_sum, window_sum)
            
            # 4. Remove the element going out of the window
            window_sum -= arr[left]
            
            # 5. Slide the window
            left += 1
            
    return max_sum
```

---

## 5.2 Variable Size Sliding Window

In this type, the size of the window is NOT fixed. It expands and shrinks dynamically based on a given condition (e.g., sum < target).

**Problem Example:** Find the length of the longest substring with at most `K` distinct characters.

**Generic Template:**

```python
def variable_window(arr, condition):
    left = 0
    max_len = 0
    # Data structure to keep track of window state (e.g., sum, hash map)
    state = 0 
    
    for right in range(len(arr)):
        # 1. Add current element to the state
        # state += arr[right] or state[arr[right]] += 1
        
        # 2. Shrink the window from the left if the condition is violated
        while not valid(state):
            # Remove arr[left] from state
            # left += 1
            pass
            
        # 3. Update the result for the valid window
        max_len = max(max_len, right - left + 1)
        
    return max_len
```

---

# 6. Time and Space Complexity

### Time Complexity
- The `right` pointer moves from `0` to `N-1`.
- The `left` pointer also moves from `0` to `N-1`.
- Each element is added to the window at most once and removed at most once.
- Overall Time Complexity is **`O(N)`** (Linear Time), avoiding the `O(N^2)` of nested loops.

### Space Complexity
- If you only use variables to store sums or lengths, the space complexity is **`O(1)`**.
- If you use a Hash Map or Set to store frequencies (common in string problems), the space complexity is **`O(K)`** where `K` is the number of distinct characters or elements (e.g., `O(26)` for lowercase English letters).

---

# 7. Summary

| Aspect | Fixed Size Window | Variable Size Window |
|--------|-------------------|----------------------|
| **Window Size** | Constant (`K`) | Dynamic (expands/shrinks) |
| **Expansion** | Expand until size `K` is reached | Expand until condition is violated |
| **Contraction** | Slide by removing 1, adding 1 | Shrink until condition is valid again |
| **Use Cases** | Max sum subarray of size K | Longest substring with K distinct chars |
| **Time Complexity**| `O(N)` | `O(N)` |

---

# 📝 Quick Revision

```text
Brute Force O(N^2) → Nested Loops
        ↓
Optimization → Sliding Window O(N)
        ↓
Check Problem Statement
├── Contiguous Array/String?
├── Max / Min / Longest / Shortest?
        ↓
Identify Type
├── Size given? → Fixed Window
└── Condition given? → Variable Window
```
