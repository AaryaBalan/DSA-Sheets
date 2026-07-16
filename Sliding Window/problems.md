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
- [1237. Find Positive Integer Solution for a Given Equation](#1237-find-positive-integer-solution-for-a-given-equation)
- [1248. Count Number of Nice Subarrays](#1248-count-number-of-nice-subarrays)
- [1358. Number of Substrings Containing All Three Characters](#1358-number-of-substrings-containing-all-three-characters)
- [395. Longest Substring with At Least K Repeating Characters](#395-longest-substring-with-at-least-k-repeating-characters)
- [1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold](#1343-number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold)
- [2024. Maximize the Confusion of an Exam](#2024-maximize-the-confusion-of-an-exam)
- [2134. Minimum Swaps to Group All 1's Together II](#2134-minimum-swaps-to-group-all-1s-together-ii)
- [2537. Count the Number of Good Subarrays](#2537-count-the-number-of-good-subarrays)
- [438. Find All Anagrams in a String](#438-find-all-anagrams-in-a-string)
- [713. Subarray Product Less Than K](#713-subarray-product-less-than-k)

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

<br/><br/><br/><br/><br/>

---

# 1237. Find Positive Integer Solution for a Given Equation

- **Difficulty:** Medium
- **Topics:** Two Pointers, Binary Search, Monotonic Function

---

# 📖 Problem Statement

You are given a hidden function:

```python
f(x, y)
```

The implementation is unknown, but it is guaranteed that:

```text
f(x, y) < f(x + 1, y)
f(x, y) < f(x, y + 1)
```

This means the function is **strictly increasing** in both `x` and `y`.

Given a target value `z`, return every positive integer pair:

```text
(x, y)
```

such that

```text
f(x, y) == z
```

where

```text
1 ≤ x, y ≤ 1000
```

---

# Key Observation

The actual formula is hidden.

You should **not** try to guess it.

Instead, use the only important property provided:

```text
The function is strictly increasing in both directions.
```

This single property is enough to solve the problem efficiently.

---

# Step 1: Understand the Monotonic Property

We know:

```text
Increasing x
↓

f(x, y) increases
```

and

```text
Increasing y
↓

f(x, y) increases
```

Imagine the values as a matrix.

```
        x →
      ---------------->
y |
↓ |
  |
```

Every row is increasing.

Every column is increasing.

So the matrix behaves exactly like a **sorted 2D matrix**.

---

# Step 2: Brute Force Idea

Try every pair:

```python
for x in range(1,1001):
    for y in range(1,1001):
```

Check

```python
f(x,y)
```

Complexity:

```text
1000 × 1000

=

1,000,000 function calls
```

This works but ignores the monotonic property.

---

# Step 3: Better Observation

Since values only increase:

If

```text
f(x,y) > z
```

then increasing either variable will only make it larger.

We should decrease the value.

Similarly,

if

```text
f(x,y) < z
```

we must increase the value.

This naturally suggests a **two-pointer (staircase search)**.

---

# Step 4: Staircase Search

Start from the **top-right corner**.

```text
x = 1
y = 1000
```

Now evaluate

```python
value = f(x, y)
```

There are only three possibilities.

---

### Case 1

```text
value == z
```

Found a valid pair.

Store it.

Then move diagonally:

```text
x += 1
y -= 1
```

---

### Case 2

```text
value < z
```

Need a larger value.

Increase

```text
x
```

```text
x += 1
```

---

### Case 3

```text
value > z
```

Need a smaller value.

Decrease

```text
y
```

```text
y -= 1
```

---

# Why This Works

Because the function is strictly increasing.

If

```text
value < z
```

then

```text
Increasing x

↓

Increases value
```

If

```text
value > z
```

then

```text
Decreasing y

↓

Decreases value
```

Every move removes one entire row or one entire column from consideration.

---

# Visual Intuition

Imagine the matrix:

```text
Small ----------------> Large

↓

Large
```

Start here:

```text
Top Right
```

If value is:

Too large

← Move left

Too small

↓ Move down

Exactly the same idea as **Search in a Sorted 2D Matrix**.

---

# Why Move Both When Equal?

Suppose

```text
f(x,y) == z
```

Since the function is strictly increasing,

moving only right or only up can never produce the same value again.

So after recording the answer:

```text
x += 1
y -= 1
```

This skips the current row and column efficiently.

---

# Optimized Solution

```python
class Solution:
    def findSolution(self, customfunction: 'CustomFunction', z: int) -> List[List[int]]:
        result = []

        x = 1
        y = 1000

        while x <= 1000 and y >= 1:

            value = customfunction.f(x, y)

            if value == z:
                result.append([x, y])
                x += 1
                y -= 1

            elif value < z:
                x += 1

            else:
                y -= 1

        return result
```

---

# Complexity Analysis

Each iteration:

- Either `x` increases.
- Or `y` decreases.

Maximum movements:

```text
x : 1000

y : 1000
```

Total:

```text
O(1000)
```

Space:

```text
O(1)
```

(excluding the output list)

---

# Alternative Approach

For every `x`:

Perform binary search on `y`.

Complexity:

```text
1000 × log(1000)
```

which is

```text
O(1000 log 1000)
```

This also works, but the two-pointer solution is simpler and faster.

---

# Pattern Recognition

Whenever a problem states:

- Function increases with `x`
- Function increases with `y`
- Need to find a target value

Think immediately:

```text
Monotonic 2D Grid

↓

Sorted Matrix Search

↓

Two Pointers (Staircase Search)
```

---

# Mental Model

Do **not** think:

```text
Reverse engineer the formula.
```

Instead think:

```text
The function behaves like a sorted matrix.
```

Then use staircase search:

```text
Start at one corner.

↓

If value is too small

Increase x.

↓

If value is too large

Decrease y.

↓

If equal

Store answer and move diagonally.
```

---

# 📝 Key Takeaways

- The hidden formula is irrelevant.
- Use the monotonic property instead.
- Model the function values as a sorted 2D matrix.
- Apply staircase search using two pointers.
- Move:
  - **Down (`x += 1`)** if the value is too small.
  - **Left (`y -= 1`)** if the value is too large.
  - **Diagonally** after finding a valid pair.
- Time Complexity:

```text
O(1000)
```

- Space Complexity:

```text
O(1)
```

<br/><br/><br/><br/><br/>

---

# 1248. Count Number of Nice Subarrays

- **Difficulty:** Medium
- **Topics:** Sliding Window, Prefix Sum

---

# 📖 Problem Statement

Given an integer array `nums` and an integer `k`, return the number of continuous subarrays that contain **exactly `k` odd numbers**.

A subarray is called **nice** if it contains exactly `k` odd numbers.

---

## Example

### Input

```python
nums = [1,1,2,1,1]
k = 3
```

### Output

```text
2
```

### Explanation

The nice subarrays are:

```text
[1,1,2,1]

[1,2,1,1]
```

---

# Intuition

Instead of thinking about the actual values, focus only on whether a number is **odd or even**.

Convert the array mentally into:

```text
Odd  → 1

Even → 0
```

Example:

```text
[1,1,2,1,1]

↓

[1,1,0,1,1]
```

Now the problem becomes:

> Count subarrays whose sum is exactly `k`.

This is the same pattern as:

- Binary Subarrays With Sum (930)
- Subarrays With K Different Integers (992)

---

# Key Observation

Sliding window naturally counts:

```text
Subarrays with at most K
```

It does **not** directly count:

```text
Subarrays with exactly K
```

So we use the transformation:

```text
Exactly(k)

=

AtMost(k)

-

AtMost(k-1)
```

This converts an "exactly" problem into two easier "at most" problems.

---

# Sliding Window Idea

Maintain a window containing **at most `k` odd numbers**.

For every position:

1. Expand the window.
2. Count odd numbers.
3. If odd count exceeds `k`, shrink the window.
4. After the window becomes valid, every subarray ending at `right` is valid.

Number of valid subarrays ending at `right`:

```text
right - left + 1
```

---

# Why `right - left + 1`?

Suppose after shrinking:

```text
left          right

↓

[ ... valid window ... ]
```

Every starting index between

```text
left

↓

right
```

creates a valid subarray ending at `right`.

Therefore:

```text
Count

=

Window Length

=

right - left + 1
```

---

# Optimized Solution

```python
class Solution:
    def numberOfSubarrays(self, nums: List[int], k: int) -> int:

        def atMost(k):

            left = 0
            odd_count = 0
            ans = 0

            for right in range(len(nums)):

                if nums[right] % 2 == 1:
                    odd_count += 1

                while odd_count > k:
                    if nums[left] % 2 == 1:
                        odd_count -= 1
                    left += 1

                ans += right - left + 1

            return ans

        return atMost(k) - atMost(k - 1)
```

---

# Why Your Approach Was Correct

The overall idea was correct.

You recognized this as an:

```text
Exactly K

↓

AtMost(K) - AtMost(K-1)
```

problem.

That is the correct transformation.

---

# Issues in the Original Implementation

## Issue 1

The variable `count` was used for two different purposes.

You used it as:

- Number of odd elements.
- Number of valid subarrays.

These should be separate variables.

---

## Issue 2

While shrinking the window:

```python
count -= 1
```

This is incorrect.

The odd counter should decrease **only if the element leaving the window is odd**.

Correct logic:

```python
if nums[left] % 2 == 1:
    odd_count -= 1
```

---

## Issue 3

Maintain two independent variables:

```text
odd_count

↓

Current odd numbers in the window

ans

↓

Total valid subarrays
```

Separating these makes the logic much clearer.

---

# Alternative O(n) Approach

Another elegant solution is based on the **positions of odd numbers**.

Example:

```text
nums = [1,1,2,1,1]
```

Odd positions:

```text
[0,1,3,4]
```

For every group of `k` consecutive odd numbers,

count:

```text
Left Choices

×

Right Choices
```

where:

- Left choices = number of possible starting positions.
- Right choices = number of possible ending positions.

This also runs in:

```text
O(n)
```

---

# Pattern Recognition

Whenever a problem asks:

- Exactly `k` odd numbers
- Exactly `k` distinct values
- Exactly `k` ones
- Exactly `k` occurrences

Think immediately:

```text
Exactly(k)

=

AtMost(k)

-

AtMost(k-1)
```

This is one of the most important sliding window transformations.

---

# Complexity Analysis

### Time

```text
O(n)
```

Each pointer moves at most once.

---

### Space

```text
O(1)
```

Only a few variables are maintained.

---

# Mental Model

Instead of asking:

```text
How do I count exactly k odd numbers?
```

Think:

```text
Count all windows with at most k odds.

↓

Count all windows with at most (k-1) odds.

↓

Subtract them.
```

This transformation simplifies many "exactly k" sliding window problems.

---

# 📝 Key Takeaways

- Convert the problem into counting **odd numbers** only.
- Use the transformation:

```text
Exactly(k)

=

AtMost(k)

-

AtMost(k-1)
```

- Maintain:
  - `odd_count` → current odd numbers in the window.
  - `ans` → total valid subarrays.
- Every valid window contributes:

```text
right - left + 1
```

subarrays ending at the current index.

- Time Complexity:

```text
O(n)
```

- Space Complexity:

```text
O(1)
```

<br/><br/><br/><br/><br/>

---

# 1358. Number of Substrings Containing All Three Characters

- **Difficulty:** Medium
- **Topics:** Sliding Window, Two Pointers

---

# 📖 Problem Statement

Given a string `s` consisting only of the characters:

```text
'a', 'b', 'c'
```

Return the number of substrings that contain **at least one occurrence of all three characters**.

---

## Example

### Input

```python
s = "abcabc"
```

### Output

```text
10
```

### Explanation

Valid substrings include:

```text
"abc"
"abca"
"abcab"
"abcabc"
"bca"
"bcab"
"bcabc"
"cab"
"cabc"
"abc"
```

---

# First Observation

The string contains **only three possible characters**:

```text
a

b

c
```

A valid substring must contain **all three**.

Since there are only three unique characters available,

a valid substring is simply a substring with:

```text
Exactly 3 distinct characters.
```

---

# Initial Approach

A natural idea is to use the well-known transformation:

```text
Exactly(k)

=

AtMost(k)

-

AtMost(k-1)
```

For this problem:

```text
Exactly 3 distinct

=

AtMost(3)

-

AtMost(2)
```

This idea is mathematically correct.

---

# Issue in the Original Implementation

Inside the `atMost()` function:

```python
count = right - left + 1
```

This overwrites the answer every iteration.

Instead, we must accumulate all valid subarrays.

Correct statement:

```python
count += right - left + 1
```

Reason:

For every `right`, all substrings starting from

```text
left

↓

right
```

are valid.

So they must all be added to the total answer.

---

# Is the atMost Approach Correct?

Yes.

The transformation

```text
AtMost(3)

-

AtMost(2)
```

correctly counts substrings having exactly three distinct characters.

However, there is an even cleaner solution for this specific problem.

---

# Better Sliding Window Observation

Instead of counting distinct characters,

directly maintain whether the window contains:

```text
'a'

'b'

'c'
```

Whenever the current window becomes valid,

every extension of that window will also remain valid.

---

# Sliding Window Idea

Maintain:

- Left pointer
- Right pointer
- Frequency of:

```text
a

b

c
```

Expand the window.

Whenever all three frequencies become positive,

the current window is valid.

Now shrink from the left while the window remains valid.

---

# Why `count += n - right`?

Suppose the current valid window is:

```text
[left ........ right]
```

Since it already contains all three characters,

every longer substring ending later is also valid.

Valid substrings are:

```text
[left, right]

[left, right+1]

[left, right+2]

...

[left, n-1]
```

Number of such substrings:

```text
n - right
```

Therefore,

every time the window is valid:

```python
count += len(s) - right
```

Then continue shrinking.

---

# Optimized Solution

```python
class Solution:
    def numberOfSubstrings(self, s: str) -> int:

        left = 0
        count = 0

        freq = {
            'a': 0,
            'b': 0,
            'c': 0
        }

        for right in range(len(s)):

            freq[s[right]] += 1

            while (
                freq['a'] > 0 and
                freq['b'] > 0 and
                freq['c'] > 0
            ):

                count += len(s) - right

                freq[s[left]] -= 1
                left += 1

        return count
```

---

# Why This Works

Once a window satisfies the condition:

```text
Contains

a

b

c
```

Adding more characters to its right can never invalidate it.

Therefore,

every extension remains valid,

allowing us to count all of them at once.

---

# Complexity Analysis

### Time

```text
O(n)
```

Each pointer moves at most once.

---

### Space

```text
O(1)
```

Only three character frequencies are maintained.

---

# Comparison of Approaches

| Approach | Works | Notes |
|----------|-------|-------|
| `AtMost(3) - AtMost(2)` | ✅ | Correct after fixing the counting bug |
| Shrink While Valid | ✅ | Simpler and more natural for this problem |

---

# Pattern Recognition

This problem belongs to a different sliding window category.

Instead of:

```text
Exactly K
```

it asks for:

```text
Window satisfies a required condition.
```

General template:

```text
Expand window.

↓

When condition becomes valid,

↓

Count all valid substrings.

↓

Shrink while still valid.
```

---

# Mental Model

Instead of thinking:

```text
Exactly three distinct characters.
```

Think:

```text
Find the smallest valid window.

↓

Once valid,

every larger window is automatically valid.

↓

Count all of them together.

↓

Shrink to discover new valid windows.
```

This is the standard sliding window pattern for **minimum valid window counting** problems.

---

# 📝 Key Takeaways

- Since the string contains only `'a'`, `'b'`, and `'c'`, having all three means **exactly three distinct characters**.
- The transformation:

```text
Exactly(3)

=

AtMost(3)

-

AtMost(2)
```

is mathematically correct.

- In the original implementation:

```python
count = right - left + 1
```

should be

```python
count += right - left + 1
```

- A cleaner solution maintains frequencies of `'a'`, `'b'`, and `'c'` directly.
- Once a window becomes valid, add:

```text
n - right
```

because every extension is also valid.

- Time Complexity:

```text
O(n)
```

- Space Complexity:

```text
O(1)
```

<br/><br/><br/><br/><br/>

---

# 395. Longest Substring with At Least K Repeating Characters

- **Difficulty:** Medium
- **Topics:** Divide & Conquer, Sliding Window

---

# 📖 Problem Statement

Given a string `s` and an integer `k`, return the length of the **longest substring** such that **every character** in that substring appears **at least `k` times**.

If no such substring exists, return `0`.

---

## Example 1

### Input

```python
s = "aaabb"
k = 3
```

### Output

```text
3
```

### Explanation

```text
"aaa"
```

is the longest valid substring because:

```text
'a' appears 3 times.
```

---

## Example 2

### Input

```python
s = "ababbc"
k = 2
```

### Output

```text
5
```

### Explanation

```text
"ababb"
```

contains:

```text
a → 2

b → 3
```

Both satisfy the condition.

---

# First Observation

Unlike many sliding window problems, the condition is:

```text
Every character

↓

Frequency ≥ k
```

This condition depends on **all character frequencies inside the window**.

That makes it difficult to apply the normal sliding window template.

---

# Approach 1: Divide and Conquer

## Key Observation

If a character appears **less than `k` times** in a substring,

that character can **never** belong to a valid answer inside that substring.

Therefore,

it naturally becomes a **splitting point**.

---

## Example

```text
s = "ababbc"

k = 2
```

Frequency:

```text
a → 2

b → 3

c → 1
```

Character:

```text
c
```

appears fewer than `k` times.

Therefore,

no valid substring can contain `'c'`.

Split around `'c'`:

```text
"ababb"

""
```

Now solve each part recursively.

---

## Algorithm

For every substring:

1. If its length is smaller than `k`, return `0`.
2. Count frequencies.
3. Find any character whose frequency is less than `k`.
4. Split around that character.
5. Recursively solve both parts.
6. Return the maximum.
7. If every character satisfies the condition, return the entire substring length.

---

## Recursive Solution

```python
class Solution:
    def longestSubstring(self, s: str, k: int) -> int:

        if len(s) < k:
            return 0

        freq = {}

        for ch in s:
            freq[ch] = freq.get(ch, 0) + 1

        for i in range(len(s)):
            if freq[s[i]] < k:
                left = self.longestSubstring(s[:i], k)
                right = self.longestSubstring(s[i + 1:], k)
                return max(left, right)

        return len(s)
```

---

## Why This Works

Any character whose frequency is less than `k` cannot belong to a valid substring.

So it acts as a separator,

allowing the problem to be divided into independent subproblems.

---

## Complexity

Since there are only 26 lowercase letters,

the practical complexity is approximately:

```text
O(26 × n)
```

which behaves close to:

```text
O(n)
```

---

# Why Normal Sliding Window Doesn't Work

Typical sliding window problems rely on a monotonic property.

For example:

```text
Too many distinct characters

↓

Shrink window.
```

Here,

adding one character may change the validity of many others.

There is no simple "invalid → shrink" condition.

So the standard sliding window template does not apply directly.

---

# Approach 2: Sliding Window with Fixed Unique Characters

There is another elegant iterative solution.

Instead of recursion,

we fix the number of distinct characters.

---

## Key Idea

There are only:

```text
26 lowercase letters.
```

The longest valid substring must contain

some number of distinct characters.

Suppose it contains:

```text
d distinct characters.
```

We simply try:

```text
1 distinct

2 distinct

...

26 distinct
```

For each possibility,

run a sliding window.

---

## Window State

Maintain:

- Frequency array.
- Number of distinct characters.
- Number of characters whose frequency is at least `k`.

---

## Valid Window

The window is valid when:

```text
Distinct Characters

=

Target Distinct Characters

=

Characters Appearing At Least k Times
```

In other words:

```text
unique_count == unique_target

AND

unique_count == count_at_least_k
```

---

## Sliding Window Algorithm

For every:

```text
unique_target = 1 → 26
```

Maintain:

- `unique_count`
- `count_at_least_k`
- Character frequencies

Expand and shrink the window.

Whenever both conditions hold,

update the answer.

---

## Iterative Solution

```python
class Solution:
    def longestSubstring(self, s: str, k: int) -> int:

        n = len(s)
        ans = 0

        for unique_target in range(1, 27):

            freq = [0] * 26

            left = 0
            right = 0

            unique_count = 0
            count_at_least_k = 0

            while right < n:

                if unique_count <= unique_target:

                    idx = ord(s[right]) - ord('a')

                    if freq[idx] == 0:
                        unique_count += 1

                    freq[idx] += 1

                    if freq[idx] == k:
                        count_at_least_k += 1

                    right += 1

                else:

                    idx = ord(s[left]) - ord('a')

                    if freq[idx] == k:
                        count_at_least_k -= 1

                    freq[idx] -= 1

                    if freq[idx] == 0:
                        unique_count -= 1

                    left += 1

                if (
                    unique_count == unique_target
                    and
                    unique_count == count_at_least_k
                ):
                    ans = max(ans, right - left)

        return ans
```

---

# Why This Works

Suppose the optimal answer contains:

```text
d distinct characters.
```

When

```text
unique_target = d
```

the sliding window will eventually discover it.

Trying all possible distinct counts guarantees correctness.

Since there are only 26 possibilities,

the solution remains efficient.

---

# Complexity

Outer loop:

```text
26
```

Sliding window:

```text
O(n)
```

Overall:

```text
O(26 × n)
```

Space:

```text
O(26)
```

---

# Comparison of Both Approaches

| Approach | Idea | Notes |
|----------|------|-------|
| Divide & Conquer | Split around invalid characters | Conceptually elegant |
| Sliding Window | Fix the number of distinct characters | Iterative and interview-friendly |

Both approaches achieve approximately:

```text
O(26 × n)
```

---

# Pattern Recognition

If a problem states:

```text
Every character must satisfy a frequency condition.
```

Think of two possibilities.

### Option 1

Invalid characters become separators.

```text
↓

Divide & Conquer
```

---

### Option 2

The alphabet size is small.

```text
↓

Try every possible number of distinct characters.

↓

Sliding Window
```

---

# Mental Model

## Divide & Conquer

Think:

```text
Invalid characters split the string.

↓

Solve each piece independently.
```

---

## Sliding Window

Think:

```text
Fix the number of distinct characters.

↓

Maintain frequencies.

↓

Check whether every distinct character has frequency ≥ k.
```

Both viewpoints solve the same problem from different angles.

---

# 📝 Key Takeaways

- The condition depends on **every character's frequency**, so the standard sliding window template does not directly apply.
- **Divide & Conquer**:
  - Characters with frequency `< k` act as separators.
  - Split the string and solve recursively.
- **Sliding Window**:
  - Iterate over every possible number of distinct characters (1–26).
  - Track:
    - Number of distinct characters.
    - Number of characters whose frequency is at least `k`.
- A window is valid when:

```text
unique_count == unique_target

AND

unique_count == count_at_least_k
```

- Time Complexity:

```text
O(26 × n)
```

- Space Complexity:

```text
O(26)
```

<br/><br/><br/><br/><br/>

----

# 1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold

- **Difficulty:** Medium
- **Topics:** Array, Sliding Window

---

# 📖 Problem Statement

Given an integer array `arr`, and two integers `k` and `threshold`, return the number of subarrays of size `k` whose **average** is greater than or equal to `threshold`.

---

## Example 1

### Input

```python
arr = [2,2,2,2,5,5,5,8]
k = 3
threshold = 4
```

### Output

```text
3
```

### Explanation

Subarrays of size `3`:

```text
[2,2,2] → average = 2 ❌

[2,2,2] → average = 2 ❌

[2,2,5] → average = 3 ❌

[2,5,5] → average = 4 ✅

[5,5,5] → average = 5 ✅

[5,5,8] → average = 6 ✅
```

Answer:

```text
3
```

---

# Step 1: Rewrite the Condition

The problem asks:

```text
Average ≥ threshold
```

Average of a window of size `k` is:

```text
sum / k
```

So,

```text
sum / k ≥ threshold
```

Multiply both sides by `k`:

```text
sum ≥ k × threshold
```

Now we no longer need to compute averages.

Instead, we only compare sums.

---

# Key Observation

Instead of checking:

```text
Average
```

we check:

```text
Window Sum ≥ k × threshold
```

This avoids unnecessary division.

---

# Step 2: Recognize the Pattern

Notice the important phrase:

```text
Subarrays of size k
```

The window size never changes.

This immediately indicates:

```text
Fixed Size Sliding Window
```

Unlike variable-size sliding window problems:

- No shrinking.
- No expanding based on conditions.
- Just move the window one position at a time.

---

# Step 3: Sliding Window Strategy

1. Compute the sum of the first `k` elements.
2. Check whether it satisfies the condition.
3. Slide the window:
   - Remove the leftmost element.
   - Add the next element.
4. Repeat until the end of the array.

Each slide updates the sum in **O(1)** time.

---

# Example Walkthrough

```text
arr = [2,2,2,2,5,5,5,8]

k = 3

threshold = 4
```

Required sum:

```text
3 × 4 = 12
```

First window:

```text
[2,2,2]

sum = 6

6 < 12
```

Slide:

```text
Remove 2

Add 2

Window = [2,2,2]

sum = 6
```

Slide:

```text
Remove 2

Add 5

Window = [2,2,5]

sum = 9
```

Slide:

```text
Remove 2

Add 5

Window = [2,5,5]

sum = 12

Valid
```

Continue similarly:

```text
[5,5,5] → 15 ✅

[5,5,8] → 18 ✅
```

Answer:

```text
3
```

---

# Algorithm

1. Compute:

```text
target = k × threshold
```

2. Compute the sum of the first window.

3. If:

```text
window_sum ≥ target
```

increase the answer.

4. Slide the window:

```text
window_sum += incoming_element

window_sum -= outgoing_element
```

5. Repeat.

---

# Optimal Solution

```python
class Solution:
    def numOfSubarrays(self, arr: List[int], k: int, threshold: int) -> int:

        target = k * threshold

        window_sum = sum(arr[:k])

        count = 0

        if window_sum >= target:
            count += 1

        for i in range(k, len(arr)):
            window_sum += arr[i]
            window_sum -= arr[i - k]

            if window_sum >= target:
                count += 1

        return count
```

---

# Why This Works

Each new window differs from the previous window by exactly:

- One element entering.
- One element leaving.

Instead of recalculating the sum every time:

```text
O(k)
```

we update it in:

```text
O(1)
```

This makes the entire algorithm linear.

---

# Complexity Analysis

### Time

```text
O(n)
```

Each element enters and leaves the window exactly once.

---

### Space

```text
O(1)
```

Only a few variables are used.

---

# Pattern Recognition

Whenever you see:

```text
Subarrays of fixed size k
```

Immediately think:

```text
Fixed Size Sliding Window
```

No shrinking logic is required.

---

# Sliding Window Categories

| Problem Type | Pattern |
|-------------|---------|
| Fixed size `k` | Fixed Size Sliding Window |
| Longest valid window | Expand and shrink when invalid |
| Exactly `k` condition | `atMost(k) - atMost(k-1)` |
| Minimum valid window | Expand until valid, then shrink |
| Frequency constraints | Sliding Window / Divide & Conquer (depending on the problem) |

---

# Mental Model

Think:

```text
Fixed-size window

↓

Compute first window

↓

Slide one step

↓

Remove one element

+

Add one element

↓

Check condition

↓

Repeat
```

---

# 📝 Key Takeaways

- Convert the average condition into a sum condition:

```text
sum ≥ k × threshold
```

- Since the window size is fixed:

```text
Fixed Size Sliding Window
```

is the ideal approach.

- Update the window sum in **O(1)** by:

```text
window_sum += new_element

window_sum -= old_element
```

- Overall Complexity:

```text
Time  : O(n)

Space : O(1)
```

<br/><br/><br/><br/><br/>

---

# 2024. Maximize the Confusion of an Exam

- **Difficulty:** Medium
- **Topics:** String, Sliding Window

---

# 📖 Problem Statement

A teacher is preparing a True/False exam.

You are given:

- A string `answerKey` consisting of only `'T'` and `'F'`.
- An integer `k`, representing the maximum number of answers you can change.

Your goal is to maximize the length of consecutive identical answers after changing at most `k` characters.

Return the maximum possible length.

---

## Example 1

### Input

```python
answerKey = "TTFF"
k = 2
```

### Output

```text
4
```

### Explanation

Flip both `'F'` to `'T'`.

```text
TTTT
```

Longest consecutive answers = **4**

---

## Example 2

### Input

```python
answerKey = "TFFT"
k = 1
```

### Output

```text
3
```

### Explanation

Option 1:

```text
TFFT
↓

FFFT
```

Longest consecutive `'F'` = **3**

Option 2:

```text
TFFT
↓

TFFF
```

Longest consecutive `'F'` = **3**

---

# Step 1: Understand the Problem

We need the **longest consecutive identical characters**.

There are only two possible characters:

```text
'T'
'F'
```

Since we can flip at most `k` characters, we can think in two ways:

- Make the entire window all `'T'`
- Make the entire window all `'F'`

Take the larger answer.

---

# Step 2: Reformulate the Problem

Suppose we want every character in the window to become `'T'`.

Then:

Every `'F'` inside the window must be flipped.

So the window is valid if:

```text
Number of 'F' ≤ k
```

Similarly, if we want every character to become `'F'`:

```text
Number of 'T' ≤ k
```

So the problem becomes:

```text
Find the longest substring containing
at most k "bad" characters.
```

---

# Step 3: Recognize the Pattern

This is exactly the same pattern as:

- 1004. Max Consecutive Ones III
- 424. Longest Repeating Character Replacement

Sliding window template:

```text
Expand window

↓

If bad characters > k

↓

Shrink window

↓

Update maximum length
```

---

# Step 4: Sliding Window Strategy

For a chosen target character (`'T'`):

- Move the right pointer.
- If current character is not `'T'`, increase `bad_count`.
- If `bad_count > k`, move the left pointer until the window becomes valid.
- Update the maximum window size.

Repeat the same for `'F'`.

Answer:

```text
max(result_for_T, result_for_F)
```

---

# Example Walkthrough

```text
answerKey = "TFFT"

k = 1
```

### Make everything `'T'`

Window:

```text
T F F T
```

Bad characters:

```text
F
```

When bad characters become:

```text
2
```

Shrink the window.

Maximum window obtained:

```text
2
```

---

### Make everything `'F'`

Window:

```text
T F F
```

Flip one `'T'`:

```text
F F F
```

Length:

```text
3
```

Answer:

```text
max(2,3) = 3
```

---

# Algorithm

For each target (`'T'` and `'F'`):

1. Initialize:
   - `left = 0`
   - `bad_count = 0`
   - `max_length = 0`

2. Expand the window using `right`.

3. If current character is not the target:

```text
bad_count += 1
```

4. While:

```text
bad_count > k
```

Shrink the window.

5. Update:

```text
max_length = max(max_length, window_size)
```

6. Return the maximum result of both targets.

---

# Optimal Solution

```python
class Solution:
    def maxConsecutiveAnswers(self, answerKey: str, k: int) -> int:

        def longestWithTarget(target):
            left = 0
            bad_count = 0
            max_len = 0

            for right in range(len(answerKey)):

                if answerKey[right] != target:
                    bad_count += 1

                while bad_count > k:
                    if answerKey[left] != target:
                        bad_count -= 1
                    left += 1

                max_len = max(max_len, right - left + 1)

            return max_len

        return max(
            longestWithTarget('T'),
            longestWithTarget('F')
        )
```

---

# Alternative One-Pass Solution

Instead of checking `'T'` and `'F'` separately, we can keep track of both counts.

Observation:

To make the window uniform, we only need to flip the **smaller group**.

So the window is valid if:

```text
min(count_T, count_F) ≤ k
```

---

## Code

```python
class Solution:
    def maxConsecutiveAnswers(self, answerKey: str, k: int) -> int:
        left = 0
        count = {'T': 0, 'F': 0}
        max_len = 0

        for right in range(len(answerKey)):
            count[answerKey[right]] += 1

            while min(count['T'], count['F']) > k:
                count[answerKey[left]] -= 1
                left += 1

            max_len = max(max_len, right - left + 1)

        return max_len
```

---

# Why This Works

To make every character in the window identical:

- Keep the majority character.
- Flip the minority character.

The number of flips required is:

```text
min(count_T, count_F)
```

If it exceeds `k`, shrink the window.

---

# Complexity Analysis

### Two-Pass Solution

**Time**

```text
O(n)
```

Two linear scans.

**Space**

```text
O(1)
```

---

### One-Pass Solution

**Time**

```text
O(n)
```

**Space**

```text
O(1)
```

---

# Pattern Recognition

Whenever a problem says:

- Longest consecutive
- Flip at most `k`
- Replace at most `k` characters

Think:

```text
Sliding Window

↓

Expand

↓

If bad characters > k

↓

Shrink

↓

Update maximum
```

---

# Similar Problems

| Problem | Pattern |
|---------|---------|
| 1004. Max Consecutive Ones III | Longest window with at most `k` bad elements |
| 424. Longest Repeating Character Replacement | Longest window after replacing at most `k` characters |
| 2024. Maximize the Confusion of an Exam | Longest window after flipping at most `k` answers |

---

# Mental Model

```text
Choose a target character

↓

Treat all other characters as "bad"

↓

Keep at most k bad characters

↓

Sliding Window

↓

Longest valid window
```

---

# 📝 Key Takeaways

- Solve the problem twice:
  - Make everything `'T'`
  - Make everything `'F'`
- Use a sliding window with at most `k` bad characters.
- Alternative one-pass solution tracks counts of both `'T'` and `'F'`.
- Complexity:

```text
Time  : O(n)

Space : O(1)
```

<br/><br/><br/><br/><br/>

---

# 2134. Minimum Swaps to Group All 1's Together II

- **Difficulty:** Medium
- **Topics:** Array, Sliding Window

---

# 📖 Problem Statement

You are given a **binary circular array** `nums`.

You may swap **any two elements**.

Your goal is to group **all the 1's together** using the minimum number of swaps.

Return the minimum number of swaps required.

---

## Example 1

### Input

```python
nums = [0,1,0,1,1,0,0]
```

### Output

```text
1
```

### Explanation

There are three `1`s in the array.

One optimal arrangement is:

```text
[0,0,1,1,1,0,0]
```

Only one swap is required.

---

## Example 2

### Input

```python
nums = [1,1,0,0,1]
```

### Output

```text
0
```

### Explanation

Because the array is circular,

```text
1 1 0 0 1
↑         ↑
```

the three `1`s are already consecutive.

---

# Step 1: Understand the Problem

The problem mentions:

```text
Minimum swaps
```

Many people immediately think:

```text
Simulate swaps
```

That is **not** the correct approach.

Instead, think:

> Where should the final block of all `1`s be?

---

# Step 2: Important Observation

Suppose the array contains:

```text
total_ones = number of 1's
```

If all `1`s are grouped together,

the final group must have length:

```text
total_ones
```

For example:

```text
nums = [0,1,0,1,1,0]

total_ones = 3
```

The final grouped block must occupy exactly **3 consecutive positions**.

---

# Step 3: Reformulate the Problem

Instead of asking:

```text
How do I swap?
```

Ask:

```text
Which window of size total_ones is the best place
to keep all the 1's?
```

Suppose a window of length `total_ones` already contains:

```text
X ones
```

Then it also contains:

```text
total_ones - X zeros
```

Those zeros must be replaced by the missing ones outside the window.

Therefore:

```text
Minimum swaps
=
Number of zeros inside the chosen window
```

Since:

```text
zeros = total_ones - ones
```

we get:

```text
minimum_swaps = total_ones - maximum_ones_in_any_window
```

This is the key insight.

---

# Step 4: Handle the Circular Array

Because the array is circular,

windows may wrap around the end.

Example:

```text
1 1 0 0 1
```

The last and first elements are adjacent.

A simple trick is:

```python
nums = nums + nums
```

Now every circular window becomes a normal window.

We only need to consider the first `n` possible starting positions.

---

# Step 5: Sliding Window Strategy

1. Count the total number of `1`s.
2. Window size = `total_ones`.
3. Duplicate the array.
4. Find the maximum number of `1`s inside any window of size `total_ones`.
5. Compute:

```text
answer = total_ones - max_ones
```

---

# Example Walkthrough

```text
nums = [0,1,0,1,1,0,0]
```

Count ones:

```text
total_ones = 3
```

Window size:

```text
3
```

Possible windows:

```text
[0,1,0] → 1 one

[1,0,1] → 2 ones

[0,1,1] → 2 ones

[1,1,0] → 2 ones

...
```

Maximum ones inside any window:

```text
2
```

Therefore:

```text
minimum_swaps = 3 - 2 = 1
```

---

# Algorithm

1. Count total number of `1`s.

2. If:

```text
total_ones ≤ 1
```

return:

```text
0
```

3. Duplicate the array.

4. Compute the number of `1`s in the first window.

5. Slide the window across all circular starting positions.

6. Track the maximum number of `1`s.

7. Return:

```text
total_ones - max_ones
```

---

# Optimal Solution

```python
class Solution:
    def minSwaps(self, nums: List[int]) -> int:

        total_ones = sum(nums)

        if total_ones <= 1:
            return 0

        nums_extended = nums + nums
        n = len(nums)

        window_ones = sum(nums_extended[:total_ones])
        max_ones = window_ones

        for i in range(total_ones, total_ones + n):
            window_ones += nums_extended[i]
            window_ones -= nums_extended[i - total_ones]

            max_ones = max(max_ones, window_ones)

        return total_ones - max_ones
```

---

# Why This Works

The final block must contain exactly all the `1`s.

Every zero inside that block must be swapped with a `1` outside.

So minimizing swaps is equivalent to minimizing zeros inside the block.

Since:

```text
zeros = window_size - ones
```

we simply maximize the number of ones already present.

---

# Complexity Analysis

### Time

```text
O(n)
```

One pass to count ones and one sliding window pass.

---

### Space

```text
O(n)
```

Because of the duplicated array.

> **Note:** It can also be implemented in **O(1)** extra space using modulo indexing instead of duplicating the array.

---

# Pattern Recognition

Whenever you see:

- Group all `1`s together
- Minimum swaps
- Binary array

Think:

```text
Window Size = Total Number of 1's
```

Then:

```text
Find window with maximum number of 1's
```

Finally:

```text
Answer = Total Ones - Maximum Ones in Window
```

---

# Similar Problems

| Problem | Pattern |
|---------|---------|
| 1151. Minimum Swaps to Group All 1's Together | Fixed-size sliding window |
| 2134. Minimum Swaps to Group All 1's Together II | Fixed-size sliding window + Circular array |
| Fixed-size window problems | Maintain window statistics while sliding |

---

# Mental Model

```text
Count total number of 1's

↓

That becomes the window size

↓

Find the window containing the most 1's

↓

Zeros inside that window
=
Swaps needed

↓

Answer = Total Ones - Max Ones
```

---

# 📝 Key Takeaways

- Do **not** simulate swaps.
- Think about the **final grouped block**.
- Window size is always:

```text
Number of 1's
```

- Maximize the number of `1`s already inside that window.
- Use a fixed-size sliding window.
- Handle circular arrays by duplicating the array (or using modulo).

**Complexity**

```text
Time  : O(n)

Space : O(n)
```

<br/><br/><br/><br/><br/>

---

# 2537. Count the Number of Good Subarrays

- **Difficulty:** Medium
- **Topics:** Array, Hash Table, Sliding Window

---

# 📖 Problem Statement

Given an integer array `nums` and an integer `k`, return the number of **good subarrays**.

A subarray is considered **good** if it contains **at least `k` equal pairs**.

A pair is defined as:

```text
(i, j)

where

i < j
and
nums[i] == nums[j]
```

---

## Example 1

### Input

```python
nums = [1,1,1,1,1]
k = 10
```

### Output

```text
1
```

### Explanation

Frequency of `1`:

```text
5
```

Number of equal pairs:

```text
5C2 = 10
```

Only the entire array has 10 pairs.

---

## Example 2

### Input

```python
nums = [3,1,4,3,2,2,4]
k = 2
```

### Output

```text
4
```

---

# Step 1: Understand the Question

A subarray is **good** if it contains at least `k` equal pairs.

Example:

```text
[1,1]
```

Pairs:

```text
1
```

---

```text
[1,1,1]
```

Frequency:

```text
3
```

Pairs:

```text
3C2 = 3
```

---

```text
[1,1,1,1]
```

Pairs:

```text
4C2 = 6
```

The more duplicates we have, the more equal pairs are formed.

---

# Step 2: Why Brute Force Doesn't Work

Generate every subarray:

```text
O(n²)
```

For every subarray, compute frequencies:

```text
O(n)
```

Overall:

```text
O(n³)
```

Too slow.

---

# Step 3: Key Observation

Instead of recomputing combinations every time,

maintain the number of pairs dynamically.

Suppose we already have:

```text
Frequency of x = f
```

When adding another `x`:

```text
New pairs formed = f
```

Why?

Because the new element pairs with each existing occurrence.

Example:

Current:

```text
1 1 1
```

Frequency:

```text
3
```

Add another `1`:

```text
1 1 1 1
```

New pairs:

```text
(4th with 1st)

(4th with 2nd)

(4th with 3rd)

= 3 new pairs
```

So while expanding:

```text
pairs += frequency[x]

frequency[x] += 1
```

This is the most important idea.

---

# Step 4: Sliding Window Reformulation

We need:

```text
Subarrays with pairs ≥ k
```

This is an **"at least"** condition.

Notice an important property:

If a window already has:

```text
pairs ≥ k
```

then making the window larger can only increase or maintain the number of pairs.

This monotonic property makes sliding window possible.

---

# Step 5: Sliding Window Strategy

Maintain:

- Frequency map
- Current number of equal pairs
- Left pointer
- Right pointer

Expand the window by moving `right`.

Whenever:

```text
pairs ≥ k
```

the current window is valid.

---

# Step 6: Counting Trick

Suppose:

```text
[left, right]
```

already satisfies:

```text
pairs ≥ k
```

Then every larger window:

```text
[left, right]

[left, right+1]

[left, right+2]

...

[left, n-1]
```

will also satisfy the condition.

So instead of counting one by one:

```text
Answer += n - right
```

Then shrink the window.

---

# Example

```text
nums = [1,1,1,1,1]

k = 10
```

Expand:

```text
1

1 1

1 1 1

1 1 1 1

1 1 1 1 1
```

Only after the full array:

```text
pairs = 10
```

Window becomes valid.

Count:

```text
n - right

=

5 - 4

=

1
```

Answer:

```text
1
```

---

# Algorithm

For every `right`:

### Add current element

```text
pairs += frequency[element]

frequency[element] += 1
```

---

### While window is valid

```text
pairs ≥ k
```

Count:

```text
answer += n - right
```

Remove left element.

Continue shrinking.

---

# Optimal Solution

```python
from collections import defaultdict

class Solution:
    def countGood(self, nums: List[int], k: int) -> int:

        freq = defaultdict(int)

        left = 0
        pairs = 0
        ans = 0
        n = len(nums)

        for right in range(n):

            pairs += freq[nums[right]]
            freq[nums[right]] += 1

            while pairs >= k:

                ans += n - right

                freq[nums[left]] -= 1
                pairs -= freq[nums[left]]

                left += 1

        return ans
```

---

# Why Removing Works

Suppose frequency before removal:

```text
f
```

After removing one occurrence:

```text
frequency = f - 1
```

The removed element contributed exactly:

```text
f - 1
```

pairs.

After decrement:

```python
freq[x] -= 1
pairs -= freq[x]
```

Since `freq[x]` now equals `f-1`, we subtract exactly the lost pairs.

---

# Complexity Analysis

### Time

```text
O(n)
```

Each element enters and leaves the window once.

---

### Space

```text
O(n)
```

For the frequency map.

---

# Pattern Recognition

Whenever you see:

- Count subarrays
- At least `k`
- Condition based on duplicate frequencies

Think:

```text
Sliding Window

+

Frequency Map

+

Running Pair Count
```

---

# Similar Problems

| Problem | Pattern |
|---------|---------|
| 2537. Count the Number of Good Subarrays | Sliding window + running pair count |
| 1358. Number of Substrings Containing All Three Characters | Shrink while valid |
| Minimum Window Substring | Shrink while valid |
| Count subarrays with at least K | Sliding window counting |

---

# Mental Model

```text
Expand window

↓

Update frequency

↓

Update pair count

↓

If pairs ≥ k

↓

All larger windows are also valid

↓

Answer += n - right

↓

Shrink window
```

---

# 📝 Key Takeaways

- Never recompute combinations for every window.
- Maintain pair count dynamically.
- Adding a duplicate creates:

```text
frequency[x]
```

new pairs.

- Removing an element removes:

```text
new_frequency[x]
```

pairs.

- Use the monotonic property:

```text
pairs ≥ k
```

to count all larger windows efficiently.

**Complexity**

```text
Time  : O(n)

Space : O(n)
```

<br/><br/><br/><br/><br/>

---

# 438. Find All Anagrams in a String

**Difficulty:** Medium  
**Pattern:** Fixed Size Sliding Window + Frequency Count

---

# Problem Statement

Given two strings:

- `s` (text)
- `p` (pattern)

Return all starting indices where an **anagram** of `p` appears in `s`.

An anagram means both strings contain **exactly the same characters with the same frequencies**, but the order can be different.

---

## Example

### Input

```text
s = "cbaebabacd"
p = "abc"
```

Possible windows of size 3:

```text
cba ✔
bae ✘
aeb ✘
eba ✘
bab ✘
aba ✘
bac ✔
acd ✘
```

Output

```text
[0,6]
```

---

# Observation

Notice that every valid substring must have length equal to `len(p)`.

If

```text
p = "abc"
```

then every candidate substring must also have length **3**.

So this becomes a **Fixed Size Sliding Window** problem.

---

# Brute Force Approach

For every substring of length `len(p)`:

1. Count frequencies.
2. Compare with frequency of `p`.

Example:

```text
Window = "cba"

Frequency

a : 1
b : 1
c : 1

Pattern

a : 1
b : 1
c : 1

Equal ✔
```

Time Complexity

```text
O(n * k)
```

where

- n = length of s
- k = length of p

Too slow.

---

# Optimal Approach

Instead of recomputing frequency every time,

Maintain a **fixed window**.

Whenever window moves:

- Add new character
- Remove old character

Then compare frequencies.

---

# Sliding Window Intuition

Suppose

```text
s = "cbaebabacd"
p = "abc"
```

Window size = 3

Initial window

```text
[c b a]
```

Move one step

```text
[b a e]
```

Instead of recounting:

Remove

```text
c
```

Add

```text
e
```

That's all.

---

# Algorithm

### Step 1

Compute frequency of pattern.

```python
freqP = Counter(p)
```

---

### Step 2

Build frequency of first window.

```python
freqS = Counter(s[:len(p)])
```

---

### Step 3

Compare frequencies.

If equal

Store answer.

---

### Step 4

Slide window.

Every move:

```text
Add right character

Remove left character
```

---

### Step 5

Again compare frequencies.

If equal

Store starting index.

---

# Dry Run

Input

```text
s = "cbaebabacd"
p = "abc"
```

Window size

```text
3
```

---

## Window 1

```text
cba
```

Frequency

```text
a : 1
b : 1
c : 1
```

Pattern

```text
a : 1
b : 1
c : 1
```

Equal

Answer

```text
[0]
```

---

## Window 2

Remove

```text
c
```

Add

```text
e
```

Window

```text
bae
```

Frequency

```text
a : 1
b : 1
e : 1
```

Not equal.

---

## Window 3

Window

```text
aeb
```

Not equal.

---

## Window 4

Window

```text
eba
```

Not equal.

---

## Window 5

Window

```text
bab
```

Not equal.

---

## Window 6

Window

```text
aba
```

Not equal.

---

## Window 7

Remove

```text
a
```

Add

```text
c
```

Window

```text
bac
```

Frequency

```text
a : 1
b : 1
c : 1
```

Matches pattern.

Answer

```text
[0,6]
```

---

# Code

```python
from collections import Counter

class Solution:
    def findAnagrams(self, s: str, p: str):

        m = len(p)
        n = len(s)

        if m > n:
            return []

        freqP = Counter(p)

        freqS = Counter(s[:m])

        ans = []

        if freqS == freqP:
            ans.append(0)

        for right in range(m, n):

            # Add new character
            freqS[s[right]] += 1

            # Remove left character
            leftChar = s[right - m]
            freqS[leftChar] -= 1

            if freqS[leftChar] == 0:
                del freqS[leftChar]

            if freqS == freqP:
                ans.append(right - m + 1)

        return ans
```

---

# Why Your Approach Didn't Work

Your idea was close, but there were three major mistakes.

---

## Mistake 1

You reset the whole window.

```python
freqS = defaultdict(int)
```

Sliding window should **never restart**.

Instead,

remove only one character from the left.

---

## Mistake 2

You reset after finding an answer.

```python
freqS = defaultdict(int)
```

This misses overlapping anagrams.

Example

```text
s = "abab"

p = "ab"
```

Correct answer

```text
[0,1,2]
```

Your reset would miss

```text
1
2
```

---

## Mistake 3

Your window size was changing.

This problem requires

```text
Window Size = len(p)
```

Always.

---

# Time Complexity

Building first window

```text
O(m)
```

Sliding window

```text
O(n)
```

Comparing frequencies

```text
O(26)
```

Since there are only lowercase English letters.

Overall

```text
O(n)
```

---

# Space Complexity

```text
O(26)

≈ O(1)
```

---

# Pattern Recognition

Whenever you read

> **Find all substrings of size k**

Immediately think

```text
Fixed Size Sliding Window
```

General Template

```python
Build first window

Check answer

for every next position:

    Add right character

    Remove left character

    Check answer
```

---

# Key Takeaways

✅ Window size is always fixed.

✅ Never reset the window.

✅ Only:

- Add one character
- Remove one character

✅ Compare frequencies after every slide.

---

# Similar Problems

- 567. Permutation in String
- 1456. Maximum Number of Vowels in a Substring of Given Length
- 1343. Number of Subarrays of Size K and Average Greater Than or Equal to Threshold
- 643. Maximum Average Subarray I
- 3254. Find the Power of K-Size Subarrays I

---

# Sliding Window Pattern

```python
# Build first window

for i in range(windowSize):
    add()

check_answer()

# Slide window

for right in range(windowSize, n):

    add(right)

    remove(right-windowSize)

    check_answer()
```

> **Golden Rule:**  
> If the problem asks for **all substrings/windows of a fixed length**, use a **fixed-size sliding window**. Never restart the window—slide it by adding the new element and removing the old one.

<br/><br/><br/><br/><br/>

---

# 713. Subarray Product Less Than K

**Difficulty:** Medium  
**Pattern:** Variable Size Sliding Window

---

# Problem Statement

Given:

- An array of **positive integers** `nums`
- An integer `k`

Return the number of **contiguous subarrays** whose **product** is **strictly less than `k`**.

---

## Example

### Input

```text
nums = [10,5,2,6]
k = 100
```

Output

```text
8
```

Valid subarrays

```text
[10]
[5]
[2]
[6]
[10,5]
[5,2]
[2,6]
[5,2,6]
```

Notice

```text
[10,5,2]
```

has product

```text
100
```

which is **NOT strictly less** than 100.

---

# Brute Force Approach

Generate every possible subarray.

For every subarray:

- Compute product.
- If product < k, increment answer.

### Code

```python
ans = 0

for i in range(n):

    product = 1

    for j in range(i,n):

        product *= nums[j]

        if product < k:
            ans += 1
```

### Complexity

```text
Time : O(n²)

Space : O(1)
```

Too slow for

```text
n = 30000
```

---

# Key Observation

All numbers are **positive**.

Therefore,

when we expand the window,

```text
product
```

can only

- increase
- or remain same (if multiplied by 1)

It will **never decrease**.

This gives us a monotonic property.

---

# Sliding Window Intuition

Maintain

```text
Window Product < k
```

Whenever product becomes

```text
>= k
```

we must shrink from the left until

```text
product < k
```

again.

---

# Why Sliding Window Works

Suppose

```text
Current Product = 150

k = 100
```

Can adding more positive numbers make it

```text
<100 ?
```

No.

It can only increase.

Therefore,

the only option is

```text
Move Left Pointer
```

This is why Sliding Window works.

---

# Algorithm

### Step 1

Initialize

```python
left = 0
product = 1
answer = 0
```

---

### Step 2

Expand the window.

```python
product *= nums[right]
```

---

### Step 3

If product becomes invalid

```text
product >= k
```

Shrink.

```python
while product >= k:
    product //= nums[left]
    left += 1
```

---

### Step 4

Now every subarray ending at

```text
right
```

is valid.

Number of such subarrays

```python
right - left + 1
```

Add them to answer.

---

# Why

```python
answer += right-left+1
```

works

This is the most important concept.

Suppose current window

```text
5 2 6
```

Current indices

```text
left = 1

right = 3
```

Product

```text
60
```

Valid.

How many valid subarrays end at

```text
6
```

They are

```text
[6]

[2,6]

[5,2,6]
```

Exactly

```text
3
```

Which equals

```text
right-left+1

3-1+1

3
```

Hence

```python
answer += right-left+1
```

---

# Dry Run

Input

```text
nums = [10,5,2,6]

k = 100
```

---

## Step 1

Window

```text
10
```

Product

```text
10
```

Valid.

Subarrays ending here

```text
[10]
```

Count

```text
1
```

Answer

```text
1
```

---

## Step 2

Window

```text
10 5
```

Product

```text
50
```

Still valid.

Subarrays ending here

```text
[5]

[10,5]
```

Count

```text
2
```

Answer

```text
3
```

---

## Step 3

Multiply

```text
50 × 2

=

100
```

Window

```text
10 5 2
```

Invalid.

Shrink.

Remove

```text
10
```

Product

```text
10
```

Window

```text
5 2
```

Now valid.

Subarrays ending here

```text
[2]

[5,2]
```

Count

```text
2
```

Answer

```text
5
```

---

## Step 4

Multiply

```text
10 × 6

=

60
```

Window

```text
5 2 6
```

Still valid.

Subarrays ending here

```text
[6]

[2,6]

[5,2,6]
```

Count

```text
3
```

Answer

```text
8
```

Final Answer

```text
8
```

---

# Correct Code

```python
class Solution:
    def numSubarrayProductLessThanK(self, nums, k):

        if k <= 1:
            return 0

        left = 0
        product = 1
        answer = 0

        for right in range(len(nums)):

            product *= nums[right]

            while product >= k:
                product //= nums[left]
                left += 1

            answer += right - left + 1

        return answer
```

---

# Mistakes in My Original Code

## Mistake 1

Used

```python
if
```

instead of

```python
while
```

Wrong

```python
if product >= k:
```

Correct

```python
while product >= k:
```

Reason

Removing one element may still leave the product invalid.

---

## Mistake 2

Removed both

```python
nums[left]

nums[right]
```

Wrong

```python
product //= nums[left]
product //= nums[right]
```

Only remove

```python
nums[left]
```

because the right element is still inside the window.

Correct

```python
product //= nums[left]
left += 1
```

---

## Mistake 3

Forgot edge case

If

```text
k <= 1
```

Every product is at least

```text
1
```

Therefore,

no valid subarray exists.

Need

```python
if k <= 1:
    return 0
```

---

# Complexity

### Time

```text
O(n)
```

Both pointers move only forward.

---

### Space

```text
O(1)
```

---

# Pattern Recognition

Whenever you see

```text
Count / Longest

Subarray

AND

sum/product/count <= K
```

Think

```text
Variable Size Sliding Window
```

Ask yourself

> If the window becomes invalid after expanding,
> can adding more elements ever make it valid again?

If the answer is

```text
NO
```

Sliding Window is applicable.

---

# Sliding Window Template

```python
left = 0

for right in range(n):

    include(nums[right])

    while window_is_invalid:

        remove(nums[left])

        left += 1

    answer += right-left+1
```

---

# Key Takeaways

✅ Numbers are positive.

✅ Product only increases while expanding.

✅ Shrink until product becomes valid.

✅ Never remove the right element while shrinking.

✅ Every valid window contributes

```text
right-left+1
```

new subarrays.

---

# Similar Problems

- 209. Minimum Size Subarray Sum
- 1004. Max Consecutive Ones III
- 2024. Maximize the Confusion of an Exam
- 424. Longest Repeating Character Replacement
- 904. Fruit Into Baskets
- 1493. Longest Subarray of 1's After Deleting One Element

---

# Interview Tip

This problem teaches one of the most important sliding window principles:

> **Maintain a window invariant.**

Here, the invariant is:

```text
Current Window Product < k
```

Every expansion or shrink operation is done only to preserve this invariant. Once you think in terms of maintaining invariants instead of resetting windows, variable-size sliding window problems become much easier to solve.

<br/><br/><br/><br/><br/>

---

