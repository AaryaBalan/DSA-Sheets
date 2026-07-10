# 1004. Max Consecutive Ones III

- **Difficulty:** Medium
- **Topics:** Array, Sliding Window, Two Pointers

---

# 📖 Problem Statement

Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1`s if you can flip **at most `k` zeros** into `1`s.

---

## Example 1

### Input

```python
nums = [1,1,1,0,0,0,1,1,1,1,0]
k = 2
```

### Output

```python
6
```

### Explanation

Flip two zeros:

```text
1 1 1 0 0 1 1 1 1 1 0
      ↑ ↑
```

Longest consecutive ones:

```text
1 1 1 0 0 1 1 1 1 1
```

Length:

```text
6
```

---

## Example 2

### Input

```python
nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1]
k = 3
```

### Output

```python
10
```

---

# Intuition

Instead of thinking

```text
Flip k zeros
```

think

```text
Find the longest subarray containing at most k zeros.
```

If a window has at most `k` zeros, we can flip those zeros into `1`s.

So the problem becomes:

```text
Longest subarray with at most k zeros.
```

This is a classic **Sliding Window** problem.

---

# My Initial Approach

## Idea

I tried to:

- Expand the window using `j`.
- Use `count` to track remaining flips.
- Remember the first flipped zero using `zero`.
- Once all flips were used, restart the window from the next position after the first flipped zero.

---

## Code

```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        n = len(nums)
        i = 0
        j = 0
        maxCount = 0
        count = k
        zero = n

        while j < n:

            print(j)

            if nums[j] == 0 and count > 0:
                count -= 1
                j += 1
                zero = min(zero, j)

            if nums[j] == 0 and count == 0:
                maxCount = max(j - i, maxCount)
                i = zero + 1
                j = i
                count = k
                zero = n

            if nums[j] == 1:
                j += 1

        return maxCount
```

---

# Problems in My Approach

### 1. Index Out of Range

Inside the first condition:

```python
j += 1
```

Immediately afterwards:

```python
if nums[j] == 0 and count == 0:
```

If `j` becomes equal to `n`, then

```python
nums[j]
```

tries to access

```python
nums[n]
```

which is outside the array and causes an

```text
IndexError: list index out of range
```

---

### 2. Restarting the Window

When all flips are used:

```python
i = zero + 1
j = i
count = k
```

the entire window is restarted.

A sliding window should **shrink gradually**, not restart from scratch.

Restarting causes unnecessary repeated work.

---

### 3. Missing Final Window

The answer is updated only when

```python
count == 0
```

If the longest valid window is at the end of the array, it may never be considered.

---

# Optimized Approach

## Idea

Maintain a sliding window containing **at most `k` zeros**.

Rules:

- Expand the window using the right pointer.
- Count the number of zeros.
- If zeros exceed `k`, shrink the window from the left until it becomes valid again.
- Update the maximum window size.

---

## Code

```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:

        left = 0
        zero_count = 0
        max_length = 0

        for right in range(len(nums)):

            if nums[right] == 0:
                zero_count += 1

            while zero_count > k:

                if nums[left] == 0:
                    zero_count -= 1

                left += 1

            max_length = max(max_length, right - left + 1)

        return max_length
```

---

# Dry Run

Input

```python
nums = [1,1,1,0,0,1,1]
k = 1
```

---

### Initial

```text
left = 0
right = 0
zero_count = 0
max_length = 0
```

---

### Right = 0

```text
Window = [1]
```

Zeros:

```text
0
```

Length:

```text
1
```

---

### Right = 1

```text
Window = [1,1]
```

Length:

```text
2
```

---

### Right = 2

```text
Window = [1,1,1]
```

Length:

```text
3
```

---

### Right = 3

Element:

```text
0
```

Zeros:

```text
1
```

Valid window.

Length:

```text
4
```

---

### Right = 4

Element:

```text
0
```

Zeros:

```text
2
```

Invalid because

```text
2 > k
```

Shrink from left.

Remove

```text
1
```

Still

```text
2 zeros
```

Remove

```text
1
```

Still

```text
2 zeros
```

Remove

```text
1
```

Still

```text
2 zeros
```

Remove first zero.

Now

```text
1 zero
```

Window becomes valid again.

---

Continue the same process until the end.

The largest valid window is the answer.

---

# Complexity Analysis

## Time Complexity

Each element enters and leaves the window at most once.

```text
O(n)
```

---

## Space Complexity

Only a few variables are used.

```text
O(1)
```

---

# Pattern Recognition

Whenever a problem asks for:

- Longest subarray
- At most `k` changes
- At most `k` zeros
- At most `k` distinct elements

immediately think:

```text
Sliding Window
```

General template:

```python
Expand right

Update window state

While window is invalid:
    Shrink left

Update answer
```

This pattern appears repeatedly in sliding window problems.

<br/><br/><br/><br/><br/>

---

