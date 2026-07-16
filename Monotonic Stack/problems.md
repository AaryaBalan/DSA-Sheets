# Monotonic Stack Problems

Welcome to the monotonic stack problems section! Here you will find various data structure and algorithm problems related to Monotonic Stack, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [316. Remove Duplicate Letters](#316-remove-duplicate-letters)

<br><br><br><br><br>

---

# 316. Remove Duplicate Letters

> **Difficulty:** Medium  
> **Technique:** Greedy + Monotonic Stack + Hash Set + Last Occurrence

---

# 📚 Problem Statement

Given a string `s`, remove duplicate letters so that:

1. Every letter appears **exactly once**.
2. The final string is the **smallest lexicographical string** possible.

---

## Example 1

Input

```text
s = "bcabc"
```

Output

```text
abc
```

---

## Example 2

Input

```text
s = "cbacdcbc"
```

Output

```text
acdb
```

---

# 🤔 What is Lexicographical Order?

Think of it like dictionary order.

```
abc < acb < bac < bca < cab
```

Smaller letters should appear as early as possible.

Example

```
abc
```

is smaller than

```
bac
```

because

```
a < b
```

---

# 🎯 What is the Goal?

Suppose

```
s = bcabc
```

Characters are

```
b
c
a
b
c
```

We want

```
Every character only once.
```

Possible answers

```
bca
cab
abc
```

Smallest among them

```
abc
```

---

# ❌ First Thought (Wrong)

We can use a set.

```
set(s)

↓

{'a','b','c'}
```

Sort it

```
abc
```

Will this always work?

No.

Example

```
cbacdcbc
```

Sorted unique

```
abcd
```

But

```
abcd
```

is **NOT** a subsequence of the original string.

We must preserve the original order.

---

# ❌ Second Thought

Take the first occurrence.

```
cbacdcbc

Take first occurrence

c
b
a
d

↓

cbad
```

Not smallest.

---

# 💡 Key Observation

We cannot freely rearrange letters.

We can only decide

```
Keep

or

Remove
```

while maintaining order.

So we need a greedy strategy.

---

# 🧠 Greedy Thinking

Suppose

```
Stack

c
```

Now we see

```
b
```

Question

Should we keep

```
c
```

before

```
b
```

No.

Because

```
b < c
```

Smaller character should come first.

But...

Can we remove c?

Only if another c appears later.

Otherwise we lose c forever.

This gives our rule.

---

# ⭐ The Three Rules

Whenever a new character arrives

## Rule 1

If it is already inside the answer

Ignore it.

Example

```
abcbc

Second b

↓

Ignore
```

---

## Rule 2

If current character is smaller than stack top

```
Current

↓

b

Top

↓

c
```

We would like

```
bc

↓

cb ❌

Better

↓

bc ✅
```

So pop.

BUT...

Only if c appears again later.

---

## Rule 3

If stack top never appears again

Don't pop.

Because

```
Lose character forever.
```

---

# 🔥 Why Do We Need Last Occurrence?

We must know

```
Will this character appear again?
```

Example

```
cbacdcbc
```

Last positions

```
c → 7

b → 6

a → 2

d → 4
```

Now while processing

if

```
current index

<

last occurrence
```

then

```
Safe to remove.
```

Otherwise

Don't remove.

---

# 🧠 Data Structures Needed

## 1. Stack

Build answer.

---

## 2. Set

Check quickly

```
Already in answer?
```

O(1)

---

## 3. Dictionary

Store last occurrence.

Example

```
{
'c':7,
'b':6,
'a':2,
'd':4
}
```

---

# 🚶 Dry Run 1

```
bcabc
```

---

## Step 1

```
b
```

Stack

```
b
```

---

## Step 2

```
c
```

Top

```
b
```

Is

```
c < b ?

No.
```

Push

```
bc
```

---

## Step 3

```
a
```

Current

```
a
```

Top

```
c
```

```
a < c

Yes
```

Does c appear later?

Yes

Pop

```
b
```

Now

Top

```
b
```

```
a < b

Yes
```

Does b appear later?

Yes

Pop

Stack

```
[]
```

Push

```
a
```

---

## Step 4

Current

```
b
```

Push

```
ab
```

---

## Step 5

Current

```
c
```

Push

```
abc
```

Finished.

Answer

```
abc
```

---

# 🚶 Dry Run 2

```
cbacdcbc
```

Last occurrence

```
c → 7
b → 6
a → 2
d → 4
```

---

### Read c

```
c
```

---

### Read b

```
b < c

Yes

c appears later

Pop
```

Stack

```
b
```

---

### Read a

```
a < b

Yes

b appears later

Pop
```

Stack

```
a
```

---

### Read c

Push

```
ac
```

---

### Read d

Push

```
acd
```

---

### Read c

Already exists

Ignore

---

### Read b

Compare

```
b < d

Yes
```

Can remove d?

No

Last occurrence of d

Already passed.

Cannot pop.

Push

```
acdb
```

---

Remaining characters

Already present

Ignore.

Answer

```
acdb
```

---

# 🎯 Algorithm

1. Store last occurrence of every character.
2. Traverse string.
3. Ignore duplicate characters already in stack.
4. While

```
Current character

<

Stack Top

AND

Stack Top appears later
```

Pop.

5. Push current character.

6. Return stack.

---

# ✅ Python Solution

```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:

        last = {}

        # Store last occurrence
        for i, ch in enumerate(s):
            last[ch] = i

        stack = []
        visited = set()

        for i, ch in enumerate(s):

            # Ignore duplicates
            if ch in visited:
                continue

            # Maintain lexicographical order
            while (
                stack
                and ch < stack[-1]
                and last[stack[-1]] > i
            ):
                visited.remove(stack.pop())

            stack.append(ch)
            visited.add(ch)

        return "".join(stack)
```

---

# ⏱ Complexity

### Time

```
O(N)
```

Each character

- pushed once
- popped at most once

---

### Space

```
O(26)

≈ O(1)
```

because only lowercase English letters exist.

---

# 🧠 Pattern Recognition

Whenever you see

- Remove duplicate characters
- Keep relative order
- Smallest lexicographical string
- Remove characters greedily

Think

```
Greedy

+

Monotonic Stack

+

Last Occurrence

+

Visited Set
```

---

# 📝 Key Takeaways

```
Need unique characters
        ↓
Need smallest lexicographical order
        ↓
Maintain answer using a stack
        ↓
If current character is smaller
        ↓
Pop larger characters
        ↓
But only if they appear again later
        ↓
Use last occurrence dictionary
        ↓
Use visited set to avoid duplicates
        ↓
Final Answer
```