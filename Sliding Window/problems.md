# Sliding Window Problems

Welcome to the sliding window problems section! Here you will find various data structure and algorithm problems related to Sliding Window, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [1004. Max Consecutive Ones III](#1004-max-consecutive-ones-iii)
- [424. Longest Repeating Character Replacement](#424-longest-repeating-character-replacement)
- [930. Binary Subarrays With Sum](#930-binary-subarrays-with-sum)
- [1423. Maximum Points You Can Obtain from Cards](#1423-maximum-points-you-can-obtain-from-cards)
- [992. Subarrays with K Different Integers](#992-subarrays-with-k-different-integers)
- [76. Minimum Window Substring](#76-minimum-window-substring)
- [1861. Rotating the Box](#1861-rotating-the-box)
- [647. Palindromic Substrings](#647-palindromic-substrings)
- [1850. Minimum Adjacent Swaps to Reach the Kth Smallest Number](#1850-minimum-adjacent-swaps-to-reach-the-kth-smallest-number)

<br><br><br><br><br>

---

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

# 424. Longest Repeating Character Replacement

- **Difficulty:** Medium
- **Topics:** Sliding Window, Hash Map, String

---

# 📖 Problem Statement

You are given a string `s` consisting of uppercase English letters and an integer `k`.

You can replace **at most `k` characters** with any uppercase letter.

Return the length of the **longest substring** that can be made of **only one repeating character** after performing at most `k` replacements.

---

## Example 1

### Input

```python
s = "ABAB"
k = 2
```

### Output

```python
4
```

### Explanation

Replace both `A`s with `B`s (or vice versa).

```text
BBBB
```

Length:

```text
4
```

---

## Example 2

### Input

```python
s = "AABABBA"
k = 1
```

### Output

```python
4
```

### Explanation

Replace one `A` with `B`.

```text
AABBBBA
```

Longest repeating substring:

```text
BBBB
```

Length:

```text
4
```

---

# Intuition

At first glance, it looks like we need to try replacing every character.

That would be very slow.

Instead, ask a different question:

> **If I already have a window, how many characters must I replace to make the entire window contain the same letter?**

---

## Example

Window:

```text
A A B A
```

Frequency:

```text
A → 3
B → 1
```

The best choice is to keep the majority character (`A`) and replace all others.

Required replacements:

```text
1
```

Instead of:

```text
Try converting to A
Try converting to B
Try converting to C
...
```

we simply keep the character that appears most frequently.

---

# The Key Observation

Suppose:

```text
Window Length = 7
```

Most frequent character appears

```text
5 times
```

Then:

```text
2 characters are different.
```

Those are exactly the characters we need to replace.

So,

```text
Replacements Needed

=

Window Size - Maximum Frequency
```

Formula:

```text
window_size - max_frequency
```

---

# Example

Window:

```text
A A B A B
```

Window Size:

```text
5
```

Frequency:

```text
A → 3
B → 2
```

Maximum Frequency:

```text
3
```

Replacements:

```text
5 - 3 = 2
```

If

```text
k >= 2
```

the window is valid.

---

# Sliding Window Idea

Maintain:

- Left pointer
- Right pointer
- Frequency of every character
- Maximum frequency inside the current window

Expand the window.

If

```text
window_size - max_frequency > k
```

the window becomes invalid.

Shrink it from the left until it becomes valid again.

---

# Dry Run

Input

```text
s = "AABABBA"
k = 1
```

---

### Initial

```text
Window = ""
```

---

### Right = 0

```text
A
```

Frequency

```text
A = 1
```

Window

```text
A
```

Valid.

Answer

```text
1
```

---

### Right = 1

```text
AA
```

Frequency

```text
A = 2
```

Window Size

```text
2
```

Replacements

```text
2 - 2 = 0
```

Valid.

Answer

```text
2
```

---

### Right = 2

```text
AAB
```

Frequency

```text
A = 2
B = 1
```

Window Size

```text
3
```

Replacements

```text
3 - 2 = 1
```

Still valid.

Answer

```text
3
```

---

### Right = 3

```text
AABA
```

Frequency

```text
A = 3
B = 1
```

Window Size

```text
4
```

Replacements

```text
4 - 3 = 1
```

Valid.

Answer

```text
4
```

---

### Right = 4

Window

```text
AABAB
```

Frequency

```text
A = 3
B = 2
```

Window Size

```text
5
```

Replacements

```text
5 - 3 = 2
```

Now

```text
2 > k
```

Window becomes invalid.

Shrink from the left until

```text
window_size - max_frequency <= k
```

Again continue expanding.

The maximum valid window length is

```text
4
```

---

# Optimized Solution

## Code

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:

        left = 0
        freq = [0] * 26
        max_freq = 0
        max_length = 0

        for right in range(len(s)):

            index = ord(s[right]) - ord('A')
            freq[index] += 1

            max_freq = max(max_freq, freq[index])

            while (right - left + 1) - max_freq > k:

                freq[ord(s[left]) - ord('A')] -= 1
                left += 1

            max_length = max(max_length, right - left + 1)

        return max_length
```

---

# Why Don't We Decrease `max_freq`?

When the window shrinks,

you may think

```python
max_freq
```

should also decrease.

Surprisingly, it is **not necessary**.

Example:

```text
Window

A A A B
```

Maximum Frequency

```text
3
```

After shrinking

```text
A A B
```

the stored value may still remain

```text
3
```

instead of

```text
2
```

This is okay.

A slightly larger `max_freq` only makes the window appear more valid temporarily.

As the window continues to move, it naturally corrects itself.

Keeping `max_freq` only increasing avoids repeatedly scanning the frequency array, giving an overall

```text
O(n)
```

solution.

---

# Complexity Analysis

## Time Complexity

Each character enters and leaves the window at most once.

```text
O(n)
```

---

## Space Complexity

Frequency array contains only 26 uppercase letters.

```text
O(26)

≈ O(1)
```

---

# Pattern Recognition

Whenever a problem says:

- Replace at most `k`
- Flip at most `k`
- Modify at most `k`
- Longest valid substring after changes

think:

```text
Sliding Window
```

Look for a formula like

```text
Window Size - Something Good <= k
```

Examples:

| Problem | Formula |
|----------|---------|
| 1004. Max Consecutive Ones III | `window_size - ones <= k` (equivalent to zeros ≤ k) |
| 424. Longest Repeating Character Replacement | `window_size - max_frequency <= k` |

---

# 📝 Key Takeaways

- Keep the **most frequent character** in the window.
- Replace every other character.
- Required replacements are

```text
window_size - max_frequency
```

- If replacements exceed `k`, shrink the window.
- Update the maximum valid window length throughout the traversal.
- This is one of the most important **Sliding Window** templates and appears frequently in interview problems.

<br/><br/><br/><br/><br/>

---

# 930. Binary Subarrays With Sum

- **Difficulty:** Medium
- **Topics:** Sliding Window, Prefix Sum, Hash Map

---

# 📖 Problem Statement

Given a binary array `nums` and an integer `goal`, return the number of **non-empty subarrays** whose sum is exactly equal to `goal`.

Since the array contains only `0`s and `1`s,

```text
Subarray Sum
=
Number of 1's in that subarray
```

---

## Example 1

### Input

```python
nums = [1,0,1,0,1]
goal = 2
```

### Output

```python
4
```

---

## Example 2

### Input

```python
nums = [0,0,0,0,0]
goal = 0
```

### Output

```python
15
```

---

# Intuition

The question asks:

```text
Count subarrays having sum exactly equal to goal.
```

Since the array is binary,

```text
Sum = Number of 1's
```

There are two common approaches.

---

# Approach 1: Prefix Sum + HashMap

## Idea

Let

```text
prefix_sum
```

be the sum from the beginning up to the current index.

If

```text
Current Prefix - Previous Prefix = goal
```

then

```text
Subarray Sum = goal
```

Rearranging,

```text
Previous Prefix = Current Prefix - goal
```

So for every prefix sum:

- Compute the current prefix sum.
- Check how many times `(prefix - goal)` has appeared.
- Add that count to the answer.

---

## Code

```python
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int):

        prefix = 0
        count = 0
        hashmap = {0: 1}

        for num in nums:

            prefix += num

            if prefix - goal in hashmap:
                count += hashmap[prefix - goal]

            hashmap[prefix] = hashmap.get(prefix, 0) + 1

        return count
```

---

## Why Initialize

```python
hashmap = {0:1}
```

If

```text
prefix == goal
```

then

```text
prefix - goal = 0
```

This means a valid subarray starts from index `0`.

---

## Complexity

### Time

```text
O(n)
```

### Space

```text
O(n)
```

---

# Approach 2: Sliding Window Trick (Binary Array Only)

## Key Observation

Instead of directly counting subarrays with sum exactly equal to `goal`,

count

```text
Subarrays with sum ≤ goal
```

and

```text
Subarrays with sum ≤ goal-1
```

Their difference gives

```text
Subarrays with sum exactly goal
```

Formula:

```text
exact(goal)

=

atMost(goal)

-

atMost(goal-1)
```

---

# Why Does This Work?

Suppose

```text
atMost(2)
```

counts

```text
Sum = 0

Sum = 1

Sum = 2
```

and

```text
atMost(1)
```

counts

```text
Sum = 0

Sum = 1
```

Subtracting them removes everything except

```text
Sum = 2
```

---

# Sliding Window

Maintain:

- Left pointer
- Right pointer
- Current window sum

Expand the window.

If

```text
sum > limit
```

shrink from the left until the window becomes valid.

Every valid window ending at `right` contributes

```text
right - left + 1
```

subarrays.

---

## Optimized Code

```python
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int):

        n = len(nums)

        def atMost(limit):

            if limit < 0:
                return 0

            left = 0
            window_sum = 0
            count = 0

            for right in range(n):

                window_sum += nums[right]

                while window_sum > limit:

                    window_sum -= nums[left]
                    left += 1

                count += right - left + 1

            return count

        return atMost(goal) - atMost(goal - 1)
```

---

# My Initial Attempt

```python
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int):

        n = len(nums)

        def countLessThan(num):

            if num < 0:
                return 0

            l = 0
            r = 0
            summ = 0
            count = 0

            while r < n:

                summ += nums[r]

                if summ <= goal:
                    count += r - l + 1
                    r += 1

                else:
                    l += 1

            return count

        return countLessThan(goal) - countLessThan(goal - 1)
```

---

# Problems in My Approach

## 1. Wrong Variable Used

The function receives

```python
num
```

but the condition checks

```python
summ <= goal
```

instead of

```python
summ <= num
```

So both calls

```python
countLessThan(goal)

countLessThan(goal - 1)
```

behave almost identically.

---

## 2. Window Never Shrinks Properly

When the window becomes invalid,

I only moved the left pointer.

```python
l += 1
```

But I forgot to remove the left element from the window sum.

Correct shrinking is

```python
summ -= nums[l]
l += 1
```

Without subtracting,

```text
summ
```

never decreases,

so the window remains invalid.

---

## 3. Missing While Loop

The window may still be invalid after removing one element.

Instead of

```python
if summ > num:
```

we should repeatedly shrink.

```python
while summ > num:
```

until the window becomes valid again.

---

# Correct Sliding Window

```python
def atMost(limit):

    if limit < 0:
        return 0

    left = 0
    window_sum = 0
    count = 0

    for right in range(len(nums)):

        window_sum += nums[right]

        while window_sum > limit:

            window_sum -= nums[left]
            left += 1

        count += right - left + 1

    return count
```

---

# Why

```python
count += right - left + 1
```

Suppose

```text
Window

[left ........ right]
```

is valid.

Then every subarray ending at

```text
right
```

and starting anywhere between

```text
left
```

and

```text
right
```

is also valid.

Number of such subarrays:

```text
right - left + 1
```

---

# Dry Run

Input

```python
nums = [1,0,1]
goal = 1
```

Compute

```text
atMost(1)
```

---

### Right = 0

Window

```text
[1]
```

Count

```text
1
```

---

### Right = 1

Window

```text
[1,0]
```

Still valid.

New subarrays

```text
[0]

[1,0]
```

Count increases by

```text
2
```

---

### Right = 2

Window

```text
[1,0,1]
```

Sum becomes

```text
2
```

Shrink.

Window becomes

```text
[0,1]
```

Valid again.

New valid subarrays ending at index 2

```text
[1]

[0,1]
```

Count increases by

```text
2
```

Continue similarly.

Finally,

```text
Answer

=

atMost(goal)

-

atMost(goal-1)
```

---

# Complexity Analysis

## Prefix Sum

Time

```text
O(n)
```

Space

```text
O(n)
```

---

## Sliding Window

Time

```text
O(n)
```

Space

```text
O(1)
```

---

# Pattern Recognition

When the problem asks

- Number of subarrays with exact sum
- Exact value
- Count subarrays

think

```text
Prefix Sum + HashMap
```

When the array contains only

- Binary values
- Non-negative numbers

you can also use

```text
exact(goal)

=

atMost(goal)

-

atMost(goal-1)
```

This is a powerful sliding window pattern used in many interview problems.

<br/><br/><br/><br/><br/>

---

# 1423. Maximum Points You Can Obtain from Cards

- **Difficulty:** Medium
- **Topics:** Array, Sliding Window, Prefix Sum

---

# 📖 Problem Statement

You are given an array `cardPoints`, where each element represents the points on a card.

You must take **exactly `k` cards**.

Each card can only be taken from either:

- the beginning of the array, or
- the end of the array.

Return the **maximum score** possible.

---

## Example 1

### Input

```python
cardPoints = [1,2,3,4,5,6,1]
k = 3
```

### Output

```python
12
```

### Explanation

One optimal choice is:

```text
Take

1 (left)
6 (right)
5 (right)
```

Score:

```text
1 + 6 + 5 = 12
```

---

## Example 2

### Input

```python
cardPoints = [2,2,2]
k = 2
```

### Output

```python
4
```

---

## Example 3

### Input

```python
cardPoints = [9,7,7,9,7,7,9]
k = 7
```

### Output

```python
55
```

---

# First Thought (Brute Force)

At every step, we have two choices:

- Take from the left.
- Take from the right.

This forms many possible combinations.

For example, when

```text
k = 3
```

Possible choices are

```text
LLL
LLR
LRL
LRR
RLL
RLR
RRL
RRR
```

The number of possibilities grows exponentially.

This approach is too slow.

---

# The Key Intuition

Instead of asking

```text
Which k cards should I take?
```

ask

```text
Which cards will remain?
```

This small change completely transforms the problem.

---

# The Mental Flip

Suppose

```text
Total cards = n
```

You take

```text
k cards
```

Then the remaining cards are

```text
n - k
```

Since you can only remove cards from the two ends,

the cards left behind must always form one continuous block.

Example

```text
[1,2,3,4,5,6,1]
```

Take

```text
1 card from left

2 cards from right
```

Remaining cards

```text
2 3 4 5
```

Notice they are contiguous.

---

# New Problem

Instead of maximizing the cards we take,

minimize the cards we leave.

So the problem becomes

```text
Find the minimum sum subarray of length (n-k).
```

Finally,

```text
Answer

=

Total Sum

-

Minimum Window Sum
```

---

# Why Does This Work?

The total sum of all cards never changes.

Suppose

```text
Total Sum = 22
```

If the remaining cards have sum

```text
10
```

then the taken cards have sum

```text
22 - 10 = 12
```

To maximize the taken sum,

we simply minimize the remaining sum.

---

# Example

```text
cardPoints = [1,2,3,4,5,6,1]

k = 3
```

Total cards

```text
7
```

Remaining window size

```text
7 - 3 = 4
```

Subarrays of length 4

```text
[1,2,3,4] = 10

[2,3,4,5] = 14

[3,4,5,6] = 18

[4,5,6,1] = 16
```

Minimum window

```text
10
```

Total sum

```text
22
```

Answer

```text
22 - 10 = 12
```

---

# Sliding Window Approach

Since the window size is fixed,

this becomes a **Fixed-Size Sliding Window** problem.

Steps:

1. Compute the total sum.
2. Compute the first window of size `n-k`.
3. Slide the window.
4. Keep track of the minimum window sum.
5. Return

```text
total_sum - minimum_window_sum
```

---

# Code

```python
class Solution:
    def maxScore(self, cardPoints: List[int], k: int):

        n = len(cardPoints)

        total_sum = sum(cardPoints)

        window_size = n - k

        if window_size == 0:
            return total_sum

        current_sum = sum(cardPoints[:window_size])
        min_sum = current_sum

        for i in range(window_size, n):

            current_sum += cardPoints[i]
            current_sum -= cardPoints[i - window_size]

            min_sum = min(min_sum, current_sum)

        return total_sum - min_sum
```

---

# Dry Run

Input

```text
cardPoints = [1,2,3,4,5,6,1]

k = 3
```

Window size

```text
4
```

---

### Initial Window

```text
[1,2,3,4]
```

Sum

```text
10
```

Minimum

```text
10
```

---

### Slide Window

Remove

```text
1
```

Add

```text
5
```

Window

```text
[2,3,4,5]
```

Sum

```text
14
```

Minimum

```text
10
```

---

### Slide Again

Window

```text
[3,4,5,6]
```

Sum

```text
18
```

Minimum

```text
10
```

---

### Slide Again

Window

```text
[4,5,6,1]
```

Sum

```text
16
```

Minimum

```text
10
```

---

Final Answer

```text
22 - 10 = 12
```

---

# Alternative Approach

Another way to think is:

Take

```text
0 cards from left

k cards from right
```

Then

```text
1 card from left

k-1 cards from right
```

Then

```text
2 cards from left

k-2 cards from right
```

Continue until

```text
k cards from left

0 cards from right
```

There are only

```text
k + 1
```

possible splits.

Compute the score for every split and keep the maximum.

---

## Code

```python
class Solution:
    def maxScore(self, cardPoints: List[int], k: int):

        left_sum = sum(cardPoints[:k])
        max_score = left_sum

        right_sum = 0

        for i in range(1, k + 1):

            left_sum -= cardPoints[k - i]
            right_sum += cardPoints[-i]

            max_score = max(max_score, left_sum + right_sum)

        return max_score
```

---

# Complexity Analysis

## Sliding Window

### Time

```text
O(n)
```

### Space

```text
O(1)
```

---

## Prefix Split Method

### Time

```text
O(k)
```

### Space

```text
O(1)
```

---

# Pattern Recognition

Whenever a problem says

- Pick elements from both ends
- Take exactly `k` elements
- Maximize score

don't think

```text
What should I take?
```

Instead think

```text
What will remain?
```

Usually,

the remaining elements form a contiguous middle window.

Then solve

```text
Minimum (or Maximum) Fixed-Size Window
```

---

# 📝 Key Takeaways

- Taking `k` cards from the ends means leaving exactly `n-k` cards in the middle.
- The remaining cards always form one contiguous subarray.
- Maximize the taken score by minimizing the remaining window sum.
- Use a **fixed-size sliding window** to find the minimum window efficiently.
- An alternative solution is to try every split between taking cards from the left and right.

<br/><br/><br/><br/><br/>

---

# 992. Subarrays with K Different Integers

- **Difficulty:** Hard
- **Topics:** Sliding Window, Hash Map

---

# 📖 Problem Statement

Given an integer array `nums` and an integer `k`, return the number of subarrays that contain **exactly `k` distinct integers**.

A subarray is a contiguous part of the array.

---

## Example 1

### Input

```python
nums = [1,2,1,2,3]
k = 2
```

### Output

```python
7
```

### Explanation

The valid subarrays are:

```text
[1,2]

[2,1]

[1,2]

[2,3]

[1,2,1]

[2,1,2]

[1,2,1,2]
```

---

## Example 2

### Input

```python
nums = [1,2,1,3,4]
k = 3
```

### Output

```python
3
```

---

# Intuition

The problem asks for

```text
Exactly k distinct integers.
```

Counting "exactly" is difficult using a normal sliding window.

Instead, convert it into an easier problem.

---

# The Key Observation

Count

```text
Subarrays with at most k distinct integers.
```

and

```text
Subarrays with at most (k-1) distinct integers.
```

Their difference gives

```text
Subarrays with exactly k distinct integers.
```

Formula:

```text
Exactly(k)

=

AtMost(k)

-

AtMost(k-1)
```

This is the same idea used in problems like:

- Binary Subarrays With Sum
- Count Number of Nice Subarrays

---

# Sliding Window for At Most K Distinct

Maintain:

- Left pointer
- Right pointer
- Frequency map

Expand the window.

If the number of distinct integers becomes greater than `k`,

shrink the window until it becomes valid again.

After the window becomes valid,

all subarrays ending at `right` are valid.

Count them using

```text
right - left + 1
```

---

# My Initial Approach

```python
class Solution:
    def subarraysWithKDistinct(self, nums: List[int], k: int):

        n = len(nums)

        def countLessThen(k):

            if k < 0:
                return 0

            left = 0
            right = 0
            count = 0
            diff_int = {}

            for right in range(n):

                if nums[right] not in diff_int:
                    diff_int[nums[right]] = 0
                else:
                    diff_int[nums[right]] += 1

                if len(diff_int.keys()) <= k:
                    count += right - left + 1

                while len(diff_int.keys()) > k:
                    diff_int[nums[left]] -= 1
                    left += 1

                return count

        return countLessThen(k) - countLessThen(k - 1)
```

---

# Problems in My Approach

## 1. Returning Inside the Loop

The statement

```python
return count
```

is inside the `for` loop.

So the function returns after processing only the first element.

It should be outside the loop.

---

## 2. Incorrect Frequency Counting

I wrote

```python
if nums[right] not in diff_int:
    diff_int[nums[right]] = 0
else:
    diff_int[nums[right]] += 1
```

For the first occurrence,

frequency becomes

```text
0
```

instead of

```text
1
```

Correct way:

```python
freq[nums[right]] = freq.get(nums[right], 0) + 1
```

---

## 3. Counting Before Shrinking

I counted the window before making it valid.

```python
if len(diff_int) <= k:
    count += right - left + 1
```

then

```python
while len(diff_int) > k:
```

This counts invalid windows.

Correct order:

```text
Expand

↓

Shrink until valid

↓

Count
```

---

## 4. Keys Were Never Removed

While shrinking,

I decreased the frequency.

```python
diff_int[nums[left]] -= 1
```

But when frequency becomes

```text
0
```

the key must be removed.

Otherwise,

```python
len(diff_int)
```

still counts it as a distinct element.

Correct:

```python
if freq[nums[left]] == 0:
    del freq[nums[left]]
```

---

# Optimized Solution

```python
class Solution:
    def subarraysWithKDistinct(self, nums: List[int], k: int):

        def atMost(limit):

            if limit < 0:
                return 0

            left = 0
            count = 0
            freq = {}

            for right in range(len(nums)):

                freq[nums[right]] = freq.get(nums[right], 0) + 1

                while len(freq) > limit:

                    freq[nums[left]] -= 1

                    if freq[nums[left]] == 0:
                        del freq[nums[left]]

                    left += 1

                count += right - left + 1

            return count

        return atMost(k) - atMost(k - 1)
```

---

# Why Don't We Need

```python
if len(freq) <= k
```

After the shrinking loop,

```python
while len(freq) > limit:
```

finishes,

the window is **guaranteed** to satisfy

```text
len(freq) <= limit
```

Therefore,

this condition is always true.

Instead of checking

```python
if len(freq) <= limit:
```

we directly count

```python
count += right - left + 1
```

The validity is enforced by the sliding window itself.

---

# Dry Run

Input

```python
nums = [1,2,1]
k = 2
```

Compute

```text
atMost(2)
```

---

### Right = 0

Window

```text
[1]
```

Distinct

```text
1
```

Valid.

Count

```text
1
```

---

### Right = 1

Window

```text
[1,2]
```

Distinct

```text
2
```

Valid.

New subarrays ending at index 1

```text
[2]

[1,2]
```

Count increases by

```text
2
```

---

### Right = 2

Window

```text
[1,2,1]
```

Distinct

```text
2
```

Still valid.

New subarrays ending at index 2

```text
[1]

[2,1]

[1,2,1]
```

Count increases by

```text
3
```

Continue similarly for

```text
atMost(k-1)
```

Then subtract.

---

# Complexity Analysis

## Time Complexity

Each element enters and leaves the window at most once.

```text
O(n)
```

---

## Space Complexity

The frequency map stores at most `k` distinct elements.

Worst case:

```text
O(n)
```

---

# Pattern Recognition

Whenever the problem asks

- Exactly `k` distinct elements
- Exactly `k` odd numbers
- Exactly `k` occurrences
- Exact count

think

```text
Exactly(k)

=

AtMost(k)

-

AtMost(k-1)
```

General sliding window template:

```python
Expand right

Update frequency

While window is invalid:
    Shrink from left

Count += right - left + 1
```

---

# 📝 Key Takeaways

- Counting **exactly** is often difficult directly.
- Convert it into

```text
AtMost(k)

-

AtMost(k-1)
```

- Always shrink the window **before** counting.
- Remove keys from the frequency map when their count becomes zero.
- After the shrinking loop, the window is guaranteed to be valid, so no extra `if` condition is needed before counting.

<br/><br/><br/><br/><br/>

---

# 76. Minimum Window Substring

- **Difficulty:** Hard
- **Topics:** Sliding Window, Hash Map

---

# 📖 Problem Statement

Given two strings `s` and `t`, return the **smallest substring** of `s` that contains **every character of `t`**, including duplicates.

If no such substring exists, return an empty string.

---

## Example 1

### Input

```python
s = "ADOBECODEBANC"
t = "ABC"
```

### Output

```python
"BANC"
```

---

## Example 2

### Input

```python
s = "a"
t = "a"
```

### Output

```python
"a"
```

---

## Example 3

### Input

```python
s = "a"
t = "aa"
```

### Output

```python
""
```

---

# Intuition

This problem is one of the most important Sliding Window problems.

The first thing to identify is **what the question is asking**.

We are **not** looking for:

- Longest substring
- Maximum length
- Exactly `k`

Instead, we are looking for:

```text
The smallest window that satisfies a requirement.
```

This immediately suggests a different sliding window pattern.

---

# Step 1: Understand the Requirement

Suppose

```text
t = "AABC"
```

The window must contain:

```text
A → 2
B → 1
C → 1
```

Notice:

- Order does **not** matter.
- Frequency **does** matter.

So we need to track frequencies.

---

# Step 2: Store Required Frequencies

Create a frequency map for `t`.

Example:

```text
t = "AABC"
```

Frequency map:

```python
{
    'A': 2,
    'B': 1,
    'C': 1
}
```

This tells us exactly what every valid window must contain.

---

# Step 3: Maintain the Current Window

As we move through `s`, maintain another frequency map.

Example:

Current window:

```text
"AADBC"
```

Window frequencies:

```python
{
    'A': 2,
    'D': 1,
    'B': 1,
    'C': 1
}
```

---

# Step 4: How Do We Know the Window is Valid?

Simply comparing two maps every time would be expensive.

Instead, introduce two variables.

## required

Number of unique characters in `t`.

Example:

```text
t = "AABC"
```

Unique characters:

```text
A
B
C
```

So

```python
required = 3
```

---

## formed

Tracks how many required characters currently satisfy their required frequency.

Example:

Need:

```text
A → 2
B → 1
C → 1
```

Window:

```text
A → 2
B → 1
C → 0
```

Then

```text
formed = 2
```

because:

- A is satisfied
- B is satisfied
- C is not

---

# Step 5: When is the Window Valid?

The window becomes valid only when

```text
formed == required
```

At this point,

every required character is present with the correct frequency.

---

# Step 6: Sliding Window Strategy

Expand the window by moving `right`.

Every time:

- Add the new character.
- Update the frequency.
- If a character's required frequency is satisfied, increase `formed`.

Once

```text
formed == required
```

the window is valid.

Now,

instead of expanding,

start shrinking from the left.

---

# Step 7: Why Shrink?

The current window is valid.

But we want the **minimum** valid window.

So while the window remains valid:

- Update the best answer.
- Remove characters from the left.
- Stop only when the window becomes invalid.

---

# Dry Run

```
s = ADOBECODEBANC
t = ABC
```

Need:

```text
A
B
C
```

---

Expand:

```text
ADOBEC
```

Window contains:

```text
A
B
C
```

Valid.

Now shrink.

Remove

```text
A
```

Window becomes invalid.

Continue expanding.

Eventually reach:

```text
BANC
```

Contains:

```text
A
B
C
```

Valid again.

Shrink further?

No.

Removing anything breaks validity.

Answer:

```text
BANC
```

---

# Sliding Window Flow

```text
Expand Right

↓

Window becomes valid

↓

Shrink Left

↓

Update minimum answer

↓

Window becomes invalid

↓

Expand again
```

---

# Optimized Solution

```python
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:

        if not s or not t:
            return ""

        need = Counter(t)
        window = {}

        required = len(need)
        formed = 0

        left = 0

        min_len = float("inf")
        answer = (0, 0)

        for right in range(len(s)):

            char = s[right]

            window[char] = window.get(char, 0) + 1

            if char in need and window[char] == need[char]:
                formed += 1

            while left <= right and formed == required:

                if right - left + 1 < min_len:
                    min_len = right - left + 1
                    answer = (left, right)

                left_char = s[left]
                window[left_char] -= 1

                if left_char in need and window[left_char] < need[left_char]:
                    formed -= 1

                left += 1

        if min_len == float("inf"):
            return ""

        l, r = answer
        return s[l:r+1]
```

---

# Complexity Analysis

## Time Complexity

Each character enters and leaves the window at most once.

```text
O(m + n)
```

where

- `m = len(s)`
- `n = len(t)`

---

## Space Complexity

Frequency maps store character counts.

```text
O(Alphabet Size)
```

For English letters, this is effectively constant.

---

# Pattern Recognition

Whenever a problem asks for

- Smallest substring
- Minimum window
- Cover all characters
- Contains every required element

immediately think:

```text
Frequency Map

+

Sliding Window

+

Shrink While Valid
```

---

# Universal Template for Minimum Window Problems

```python
for right:

    Add current element

    while window is valid:

        Update answer

        Remove left element

        left += 1
```

Notice the difference from longest-window problems.

### Longest Window

```text
Expand

↓

Window becomes invalid

↓

Shrink
```

---

### Minimum Window

```text
Expand

↓

Window becomes valid

↓

Shrink as much as possible
```

This inversion is the key idea behind minimum window problems.

---

# Connection to Other Sliding Window Problems

| Problem | Pattern |
|----------|---------|
| 1004. Max Consecutive Ones III | Longest valid window |
| 424. Longest Repeating Character Replacement | Longest valid window with frequency |
| 930. Binary Subarrays With Sum | Count exact using AtMost |
| 992. Subarrays with K Different Integers | Count exact using AtMost |
| 76. Minimum Window Substring | Minimum valid window |

---

# 📝 Key Takeaways

- This is a **minimum valid window** problem.
- Track required frequencies using a hash map.
- Use another map to maintain the current window.
- `formed` tells whether the window satisfies all requirements.
- Expand until the window becomes valid.
- Shrink while it remains valid to obtain the smallest answer.
- Remember the core pattern:

```text
Expand

↓

Valid

↓

Shrink

↓

Update Minimum
```

<br/><br/><br/><br/><br/>

---

# 1861. Rotating the Box

- **Difficulty:** Medium
- **Topics:** Matrix, Simulation

---

# 📖 Problem Statement

You are given an `m × n` matrix representing a side view of a box.

Each cell contains one of:

- `'#'` → Stone
- `'*'` → Obstacle
- `'.'` → Empty space

The box is rotated **90° clockwise**, after which gravity causes every stone to fall downward until it reaches:

- Another stone
- An obstacle
- The bottom of the box

Return the final rotated box.

---

## Example 1

### Input

```python
boxGrid = [["#",".","#"]]
```

### Output

```text
[
 [ "." ],
 [ "#" ],
 [ "#" ]
]
```

---

## Example 2

### Input

```python
boxGrid = [
    ["#",".","*","."],
    ["#","#","*","."]
]
```

### Output

```text
[
 ["#","."],
 ["#","#"],
 ["*","*"],
 [".","."]
]
```

---

# Intuition

At first glance, this problem looks complicated.

You might think:

```text
Rotate the matrix

↓

Simulate gravity downward
```

Although this works, simulating vertical gravity after rotation is more complicated.

Instead, use a simpler observation.

---

# Key Observation

After a **90° clockwise rotation**,

```text
Right

↓

Down
```

That means,

instead of rotating first,

we can:

```text
Simulate gravity toward the RIGHT

↓

Rotate the matrix
```

The final result will be exactly the same.

This simplifies the implementation significantly.

---

# Step 1: Simulate Gravity Horizontally

Consider one row.

Example:

```text
# . # * . #
```

The obstacle divides the row into independent segments.

Each segment behaves separately.

Within each segment,

stones slide as far right as possible.

---

# Step 2: Process One Row

Use an `empty` pointer.

Initially,

```text
empty = last column
```

Traverse the row from

```text
Right → Left
```

---

## Case 1: Obstacle

If the current cell is

```text
*
```

gravity cannot cross it.

So reset the pointer.

```python
empty = current_column - 1
```

---

## Case 2: Stone

If the current cell is

```text
#
```

move it to the current `empty` position.

Then move the pointer one step left.

---

## Case 3: Empty Cell

Simply continue.

---

# Example

Initial row

```text
# . # . *
```

Process from right to left.

Obstacle:

```text
*
```

Everything before it forms one segment.

After gravity:

```text
. # # . *
```

All stones move to the right.

---

# Step 3: Rotate the Matrix

After gravity simulation,

rotate the matrix.

For an original matrix of size

```text
m × n
```

the rotated matrix has size

```text
n × m
```

Position mapping:

```text
new[j][m - 1 - i] = boxGrid[i][j]
```

This is the standard 90° clockwise rotation formula.

---

# Dry Run

Original

```text
# . #
```

---

### Gravity

Row becomes

```text
. # #
```

---

### Rotation

Original:

```text
. # #
```

After rotation:

```text
.
#
#
```

Exactly as expected.

---

# Optimized Solution

```python
class Solution:
    def rotateTheBox(self, boxGrid: List[List[str]]) -> List[List[str]]:

        m = len(boxGrid)
        n = len(boxGrid[0])

        # Step 1: Simulate gravity towards the right
        for row in range(m):

            empty = n - 1

            for col in range(n - 1, -1, -1):

                if boxGrid[row][col] == '*':
                    empty = col - 1

                elif boxGrid[row][col] == '#':
                    boxGrid[row][col] = '.'
                    boxGrid[row][empty] = '#'
                    empty -= 1

        # Step 2: Rotate the matrix
        rotated = [[None] * m for _ in range(n)]

        for i in range(m):
            for j in range(n):
                rotated[j][m - 1 - i] = boxGrid[i][j]

        return rotated
```

---

# Why This Works

Before rotation,

gravity is simulated toward the

```text
Right
```

After rotating the matrix,

that direction naturally becomes

```text
Down
```

which matches the required behavior.

---

# Complexity Analysis

## Time Complexity

Gravity Simulation

```text
O(m × n)
```

Rotation

```text
O(m × n)
```

Total

```text
O(m × n)
```

---

## Space Complexity

The rotated matrix requires

```text
O(m × n)
```

extra space.

---

# Pattern Recognition

Whenever a problem contains

- Matrix rotation
- Gravity
- Falling objects
- Obstacles

ask yourself:

```text
Can I simulate gravity BEFORE rotating?
```

Sometimes changing the order of operations makes the implementation much simpler.

---

# Rotation Formula

For a matrix of size

```text
m × n
```

rotated clockwise,

every element moves as follows:

```text
Original:

(i, j)

↓

Rotated:

(j, m - 1 - i)
```

Remember this mapping for all 90° clockwise rotation problems.

---

# Mental Model

Think of this problem as two completely separate operations.

```text
Compress every row to the right
        +
Rotate the matrix
```

Instead of thinking about falling stones after rotation, simplify the problem into:

```text
Row Compression
        +
Matrix Rotation
```

This makes the solution much easier to derive and implement.

---

# 📝 Key Takeaways

- Gravity after a clockwise rotation is equivalent to moving stones **right before rotation**.
- Process each row independently.
- Obstacles split the row into separate segments.
- Use an `empty` pointer to place stones efficiently.
- Rotate using the standard mapping:

```text
new[j][m - 1 - i] = old[i][j]
```

- Overall complexity:

```text
Time  : O(m × n)

Space : O(m × n)
```

<br/><br/><br/><br/><br/>

---

# 647. Palindromic Substrings

- **Difficulty:** Medium
- **Topics:** String, Two Pointers, Expand Around Center

---

# 📖 Problem Statement

Given a string `s`, return the number of **palindromic substrings**.

A palindrome is a string that reads the same forward and backward.

A substring is a contiguous sequence of characters.

---

## Example 1

### Input

```python
s = "abc"
```

### Output

```python
3
```

### Explanation

The palindromic substrings are:

```text
"a"
"b"
"c"
```

---

## Example 2

### Input

```python
s = "aaa"
```

### Output

```python
6
```

### Explanation

The palindromic substrings are:

```text
"a"
"a"
"a"
"aa"
"aa"
"aaa"
```

---

# Intuition

The brute-force idea is straightforward.

Generate every possible substring.

Then check whether each substring is a palindrome.

---

# Brute Force

There are

```text
O(n²)
```

possible substrings.

Checking each substring takes

```text
O(n)
```

Therefore,

```text
Time = O(n³)
```

This is too slow.

---

# Key Observation

Instead of checking every substring,

think in the opposite direction.

Ask:

```text
How is a palindrome formed?
```

Every palindrome grows outward from a center.

This is the key idea.

---

# Two Types of Palindromes

## 1. Odd-Length Palindrome

The center is a character.

Example:

```text
aba
 ^
```

Expand outward.

---

## 2. Even-Length Palindrome

The center lies between two characters.

Example:

```text
abba
 ^^
```

Expand outward.

---

Therefore,

for every position in the string,

we must check both possibilities.

---

# Number of Centers

For a string of length

```text
n
```

there are

```text
n odd centers

+

n-1 even centers
```

Total:

```text
2n-1 centers
```

---

# Expand Around Center

For every center:

1. Start with left and right pointers.
2. Expand while:

```text
left >= 0

right < n

s[left] == s[right]
```

3. Every successful expansion gives one palindrome.

---

# Example

```
s = "aaa"
```

---

## Odd Centers

Center at index 0

```text
a
```

Count = 1

---

Center at index 1

```text
a

↓

aaa
```

Count = 2

---

Center at index 2

```text
a
```

Count = 1

---

Odd total

```text
4
```

---

## Even Centers

Between

```text
0 and 1
```

Palindrome

```text
aa
```

Count = 1

---

Between

```text
1 and 2
```

Palindrome

```text
aa
```

Count = 1

---

Total

```text
4 + 2 = 6
```

---

# Dry Run

```
s = "aba"
```

Center at

```text
b
```

Expand:

```text
b

↓

aba
```

Both are palindromes.

---

Center between

```text
a | b
```

Characters differ.

Stop.

---

# Optimized Solution

```python
class Solution:
    def countSubstrings(self, s: str) -> int:

        n = len(s)
        count = 0

        def expand(left, right):

            nonlocal count

            while (
                left >= 0
                and right < n
                and s[left] == s[right]
            ):
                count += 1
                left -= 1
                right += 1

        for i in range(n):

            # Odd-length palindrome
            expand(i, i)

            # Even-length palindrome
            expand(i, i + 1)

        return count
```

---

# Cleaner Version

Instead of using `nonlocal`, return the number of palindromes found from each center.

```python
class Solution:
    def countSubstrings(self, s: str) -> int:

        n = len(s)
        count = 0

        def expand(left, right):

            total = 0

            while (
                left >= 0
                and right < n
                and s[left] == s[right]
            ):
                total += 1
                left -= 1
                right += 1

            return total

        for i in range(n):

            count += expand(i, i)

            count += expand(i, i + 1)

        return count
```

---

# Why This Works

Every palindrome has exactly one center.

Instead of generating every substring,

we generate every possible center.

Each successful expansion corresponds to one valid palindrome.

---

# Complexity Analysis

## Time Complexity

There are

```text
2n-1
```

centers.

Each expansion may take

```text
O(n)
```

Therefore,

```text
O(n²)
```

---

## Space Complexity

Only two pointers are used.

```text
O(1)
```

---

# Alternative Approach

Dynamic Programming.

Define

```text
dp[i][j]
```

as

```text
True

if s[i...j] is a palindrome
```

This also runs in

```text
O(n²)
```

time,

but requires

```text
O(n²)
```

space.

The expand-around-center solution is cleaner and uses constant extra space.

---

# Pattern Recognition

Whenever a problem asks about

- Counting palindromic substrings
- Longest palindromic substring
- Expanding palindromes

immediately think:

```text
Expand Around Center
```

---

# Related Problems

| Problem | Technique |
|----------|-----------|
| 5. Longest Palindromic Substring | Expand Around Center |
| 647. Palindromic Substrings | Expand Around Center |
| 131. Palindrome Partitioning | Expand / DP |

---

# Mental Model

Instead of thinking

```text
Generate all substrings

↓

Check palindrome
```

think

```text
Choose a center

↓

Expand outward

↓

Every successful expansion is one palindrome
```

This change in perspective reduces the complexity from

```text
O(n³)

↓

O(n²)
```

and is the standard approach for palindrome substring problems.

---

# 📝 Key Takeaways

- Every palindrome grows from a center.
- There are two kinds of centers:
  - Odd-length (`i, i`)
  - Even-length (`i, i+1`)
- Expand while characters match.
- Every successful expansion contributes one palindrome.
- Time Complexity:

```text
O(n²)
```

- Space Complexity:

```text
O(1)
```

<br/><br/><br/><br/><br/>

---

# 1850. Minimum Adjacent Swaps to Reach the Kth Smallest Number

- **Difficulty:** Medium
- **Topics:** Greedy, String, Next Permutation

---

# 📖 Problem Statement

Given a string `num` representing a large integer and an integer `k`:

1. Find the **k-th smallest wonderful number**, where a wonderful number is the next lexicographically larger permutation of `num`.
2. Return the **minimum number of adjacent swaps** required to transform the original `num` into that k-th permutation.

---

## Example

### Input

```python
num = "5489355142"
k = 4
```

### Output

```python
2
```

### Explanation

The 4th next permutation is:

```text
5489355421
```

Transform:

```text
5489355142
↓

5489355412

↓

5489355421
```

Total adjacent swaps:

```text
2
```

---

# Problem Breakdown

The problem consists of **two completely independent phases**.

## Phase 1

Generate the **k-th next permutation**.

## Phase 2

Find the **minimum adjacent swaps** to convert the original number into that target permutation.

---

# Phase 1: Finding the k-th Smallest Wonderful Number

If you already know the **Next Permutation** algorithm, this part becomes simple.

Instead of finding only the next permutation once,

repeat it exactly `k` times.

```text
target = num

Repeat k times:

target = next_permutation(target)
```

---

## Why This Works

Constraints:

```text
n ≤ 1000
k ≤ 1000
```

Each next permutation costs

```text
O(n)
```

Therefore,

```text
Total = O(k × n)
```

Maximum operations:

```text
1000 × 1000 = 10⁶
```

This easily fits within the limits.

No combinatorics or factorial mathematics are needed.

---

# Next Permutation Reminder

For an array:

```text
a
```

### Step 1

Find the **dip** from the right.

Find the first index

```text
a[i] < a[i+1]
```

---

### Step 2

Find the smallest element greater than `a[i]` on its right.

---

### Step 3

Swap them.

---

### Step 4

Reverse the suffix.

---

# Phase 2: Minimum Adjacent Swaps

Now suppose we have:

```text
Original

5489355142
```

Target

```text
5489355421
```

We need the minimum adjacent swaps to convert one into the other.

---

# Key Observation

Adjacent swaps behave exactly like bubble sort.

Whenever the current digit isn't correct,

bring the required digit left one position at a time.

---

# Greedy Strategy

Treat the original number as a mutable list.

For every position `i`:

If

```text
original[i] == target[i]
```

move to the next index.

Otherwise:

1. Search to the right for the required digit.
2. Bubble that digit left until it reaches position `i`.
3. Count every adjacent swap.

This guarantees the minimum number of adjacent swaps.

---

# Example

Original:

```text
1 1 1 1 2
```

Target:

```text
2 1 1 1 1
```

At index

```text
0
```

Need:

```text
2
```

Found at:

```text
index = 4
```

Bubble left:

```text
11112

↓

11121

↓

11211

↓

12111

↓

21111
```

Total swaps:

```text
4
```

---

# Complete Algorithm

### Step 1

Convert the string into a list.

---

### Step 2

Generate the k-th permutation.

```text
Repeat next permutation k times.
```

---

### Step 3

Compare the original list with the target.

Whenever characters differ:

- Find the matching character.
- Bubble it left.
- Count swaps.

---

# Optimized Solution

```python
class Solution:
    def getMinSwaps(self, num: str, k: int) -> int:

        # ---------- Step 1: Generate kth permutation ----------

        def next_permutation(arr):

            n = len(arr)

            # Find dip
            i = n - 2
            while i >= 0 and arr[i] >= arr[i + 1]:
                i -= 1

            # Find just larger element
            j = n - 1
            while arr[j] <= arr[i]:
                j -= 1

            arr[i], arr[j] = arr[j], arr[i]

            # Reverse suffix
            arr[i + 1:] = reversed(arr[i + 1:])

        target = list(num)

        for _ in range(k):
            next_permutation(target)

        # ---------- Step 2: Count adjacent swaps ----------

        original = list(num)
        swaps = 0

        for i in range(len(original)):

            if original[i] == target[i]:
                continue

            j = i

            while original[j] != target[i]:
                j += 1

            while j > i:
                original[j], original[j - 1] = original[j - 1], original[j]
                swaps += 1
                j -= 1

        return swaps
```

---

# Why the Greedy Swap Works

Suppose we need

```text
target[i]
```

at position `i`.

The nearest occurrence of that digit on the right is exactly the one we should move.

Each adjacent swap decreases its distance by one.

No extra swaps are introduced.

Therefore, this greedy approach always gives the minimum number of adjacent swaps.

---

# Dry Run

Original:

```text
11112
```

Target:

```text
21111
```

Need `2` at index `0`.

Find it at index `4`.

Bubble left:

```text
11112

↓

11121

↓

11211

↓

12111

↓

21111
```

Swaps:

```text
4
```

---

# Complexity Analysis

## Next Permutation

Repeated `k` times.

```text
O(k × n)
```

---

## Adjacent Swap Counting

Worst case:

```text
O(n²)
```

---

## Total

```text
O(k × n + n²)
```

With

```text
n ≤ 1000
```

this is acceptable.

---

# Pattern Recognition

This problem combines two well-known algorithms.

## Pattern 1

Generate a target state.

```text
Next Permutation
```

---

## Pattern 2

Transform one state into another.

```text
Greedy Adjacent Bubble Swaps
```

Many interview problems follow this structure:

```text
Generate Target

↓

Compute Minimum Operations
```

---

# Mental Model

Instead of thinking:

```text
Find kth permutation

and

Find swaps

at the same time
```

split the problem into two separate tasks:

```text
1. Generate the target permutation.

↓

2. Transform the original into the target using the minimum adjacent swaps.
```

Treat them independently, then combine the answers.

---

# 📝 Key Takeaways

- The problem has **two independent phases**:
  1. Generate the **k-th next permutation**.
  2. Compute the **minimum adjacent swaps**.
- Generate the target by applying **Next Permutation** exactly `k` times.
- Use a greedy bubble strategy to move each required digit into its correct position.
- Every adjacent swap reduces the distance by one, ensuring the minimum number of swaps.
- Time Complexity:

```text
O(k × n + n²)
```

- Space Complexity:

```text
O(n)
```