# Sliding Window Problems

Welcome to the sliding window problems section! Here you will find various data structure and algorithm problems related to Sliding Window, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [1004. Max Consecutive Ones III](#1004-max-consecutive-ones-iii)
- [424. Longest Repeating Character Replacement](#424-longest-repeating-character-replacement)
- [930. Binary Subarrays With Sum](#930-binary-subarrays-with-sum)
- [1423. Maximum Points You Can Obtain from Cards](#1423-maximum-points-you-can-obtain-from-cards)

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