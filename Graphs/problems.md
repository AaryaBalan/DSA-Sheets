# LeetCode 994 - Rotting Oranges

## Difficulty
Medium

---

# Problem Statement

You are given an `m x n` grid.

Each cell contains one of the following:

- `0` → Empty cell
- `1` → Fresh Orange
- `2` → Rotten Orange

Every minute,

A rotten orange rots all **4-directionally adjacent** fresh oranges.

```
    Up
Left  X  Right
    Down
```

Diagonal cells do **not** rot.

Return the **minimum number of minutes** required until there are **no fresh oranges left**.

If some oranges can never rot, return `-1`.

---

# First Observation

Suppose the grid is

```
2 1 1
1 1 0
0 1 1
```

At minute 0

```
2 1 1
1 1 0
0 1 1
```

Minute 1

```
2 2 1
2 1 0
0 1 1
```

Minute 2

```
2 2 2
2 2 0
0 1 1
```

Minute 3

```
2 2 2
2 2 0
0 2 1
```

Minute 4

```
2 2 2
2 2 0
0 2 2
```

Answer = **4**

Notice something important.

The rotten oranges spread exactly like **waves**.

```
Time 0

2

↓

Time 1

2
2

↓

Time 2

2
2 2

↓

Time 3

2 2 2
```

Whenever you see something spreading level by level, think

> **Breadth First Search (BFS)**

---

# Why Not DFS?

Suppose you use DFS.

```
2 1 1

1 1 1
```

DFS may go

```
2

↓

↓

↓

```

before exploring nearby oranges.

But the problem says

> Everything rots **simultaneously every minute.**

DFS cannot simulate simultaneous spreading naturally.

BFS explores

```
Level 0

2

Level 1

neighbors

Level 2

neighbors of neighbors
```

Exactly matching the problem.

---

# Biggest Trick

Most BFS problems start from **one source.**

Example:

```
S . . .

```

But here,

there may be many rotten oranges.

Example

```
2 1 2

1 1 1

2 1 2
```

All rotten oranges start spreading together.

So instead of

```
queue = [(one source)]
```

we do

```
queue = [(every rotten orange)]
```

This is called

# Multi-Source BFS

---

# Idea

Step 1

Find every rotten orange.

```
2 1 1
1 2 1
0 1 2
```

Queue initially becomes

```
[(0,0),
 (1,1),
 (2,2)]
```

All of them start spreading together.

---

Step 2

Count fresh oranges.

Suppose

Fresh = 6

Every time we rot one,

Fresh--

When Fresh becomes

```
0
```

we are done.

---

# BFS Logic

While queue isn't empty

Take one rotten orange

```
(r,c)
```

Look at its four neighbors.

```
Up

(r-1,c)

Down

(r+1,c)

Left

(r,c-1)

Right

(r,c+1)
```

If neighbor is

```
Fresh (1)
```

Then

Make it rotten.

```
grid[newr][newc] = 2
```

Decrease fresh count.

Push into queue.

---

# Why Modify Original Grid?

You tried

```python
dupGrid = [row[:] for row in grid]
```

That works.

But it is unnecessary.

Once an orange becomes rotten,

it never becomes fresh again.

So we can safely modify

```
grid
```

directly.

No copy required.

---

# Level Order BFS

Instead of storing time inside queue

```
(i,j,time)
```

we process one level at a time.

Suppose queue contains

```
Minute 0

(0,0)
```

Size = 1

Process exactly one node.

After processing,

queue becomes

```
Minute 1

(0,1)
(1,0)
```

Size = 2

Process exactly two nodes.

After that,

queue contains

Minute 2 oranges.

Each layer equals exactly one minute.

---

# Dry Run

Grid

```
2 1 1
1 1 0
0 1 1
```

Fresh = 6

Queue

```
[(0,0)]
```

Minutes = 0

---

## Minute 1

Process

```
(0,0)
```

Rot

```
(0,1)
(1,0)
```

Grid

```
2 2 1
2 1 0
0 1 1
```

Queue

```
[(0,1),
 (1,0)]
```

Minutes = 1

Fresh = 4

---

## Minute 2

Process both

```
(0,1)

(1,0)
```

New rotten

```
(0,2)

(1,1)
```

Grid

```
2 2 2
2 2 0
0 1 1
```

Queue

```
[(0,2),
 (1,1)]
```

Minutes = 2

Fresh = 2

---

## Minute 3

Process

```
(0,2)

(1,1)
```

New rotten

```
(2,1)
```

Grid

```
2 2 2
2 2 0
0 2 1
```

Queue

```
[(2,1)]
```

Minutes = 3

Fresh = 1

---

## Minute 4

Process

```
(2,1)
```

Rot

```
(2,2)
```

Grid

```
2 2 2
2 2 0
0 2 2
```

Fresh = 0

Answer

```
4
```

---

# Impossible Case

```
2 1 1
0 1 1
1 0 1
```

Bottom left orange

```
1
```

has no path from any rotten orange.

BFS finishes.

Fresh > 0

Return

```
-1
```

---

# Python Solution

```python
from collections import deque

class Solution:
    def orangesRotting(self, grid):
        rows = len(grid)
        cols = len(grid[0])

        queue = deque()
        fresh = 0

        # Find all rotten oranges and count fresh ones
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 2:
                    queue.append((r, c))
                elif grid[r][c] == 1:
                    fresh += 1

        # No fresh oranges
        if fresh == 0:
            return 0

        minutes = 0

        directions = [
            (-1, 0),
            (1, 0),
            (0, -1),
            (0, 1)
        ]

        while queue and fresh > 0:

            # Process one minute (one BFS level)
            for _ in range(len(queue)):
                r, c = queue.popleft()

                for dr, dc in directions:
                    nr = r + dr
                    nc = c + dc

                    if (
                        0 <= nr < rows and
                        0 <= nc < cols and
                        grid[nr][nc] == 1
                    ):
                        grid[nr][nc] = 2
                        fresh -= 1
                        queue.append((nr, nc))

            minutes += 1

        if fresh == 0:
            return minutes

        return -1
```

# My Solution

```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        rows = len(grid)
        cols = len(grid[0])
        time = 0
        maxTime = 0
        fresh = 0
        queue = deque([])
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 2:
                    queue.append((i, j, time))
                if grid[i][j] == 1:
                    fresh += 1
        directions = [
            (-1, 0),
            (1, 0),
            (0, -1), 
            (0, 1)
        ]
        while queue:
            r, c, t = queue.popleft()
            for dr, dc in directions:
                nr = r + dr
                nc = c + dc
                if (
                    0 <= nr < rows and
                    0 <= nc < cols and
                    grid[nr][nc] == 1
                ):
                    grid[nr][nc] = 2
                    queue.append((nr, nc, t+1))
                    maxTime = max(maxTime, t+1)
                    fresh -= 1
        return maxTime if fresh == 0 else -1
```

---

# Time Complexity

Suppose

```
Rows = m

Columns = n
```

Every cell is visited at most once.

Therefore

```
Time = O(m × n)
```

---

# Space Complexity

Queue may contain every orange.

Worst case

```
Space = O(m × n)
```

---

# Pattern Recognition

Whenever you see words like

- spread
- infection
- virus
- fire
- ripple
- shortest time
- minimum steps
- nearest distance
- every minute
- simultaneously

Immediately think

```
Multi-Source BFS
```

Examples:

- Rotting Oranges
- Walls and Gates
- 01 Matrix
- Map of Highest Peak
- Shortest Bridge
- Escape the Fire

This problem is one of the classic introductions to recognizing and applying Multi-Source BFS.