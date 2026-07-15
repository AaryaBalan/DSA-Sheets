# Graph Problems

Welcome to the graph problems section! Here you will find various data structure and algorithm problems related to Graphs, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [1791. Find Center of Star Graph](#1791-find-center-of-star-graph)
- [797. All Paths From Source to Target](#797-all-paths-from-source-to-target)
- [1557. Minimum Number of Vertices to Reach All Nodes](#1557-minimum-number-of-vertices-to-reach-all-nodes)
- [994. Rotting Oranges](#994-rotting-oranges)
- [547. Number of Provinces](#547-number-of-provinces)
- [207. Course Schedule](#207-course-schedule)

<br><br><br><br><br>

---

# 1791. Find Center of Star Graph

## Difficulty
Easy

---

# Problem Statement

You are given an **undirected star graph**.

Your task is to find the **center node**.

Return the center of the graph.

---

# First, What is a Graph?

A graph consists of

- Nodes (Vertices)
- Edges (Connections)

Example

```
1 ----- 2

      /

     3
```

Here

Nodes

```
1
2
3
```

Edges

```
1 - 2

2 - 3
```

---

# What is an Undirected Graph?

Undirected means

```
1 ----- 2
```

You can travel

```
1 → 2

and

2 → 1
```

There is no direction.

---

# What is a Star Graph?

A Star Graph has

- Exactly one center node
- Every other node is connected to that center
- No connections between outer nodes

Example

```
        1
        |
        |
4 ------2------3
        |
        |
        5
```

Who is connected to everyone?

```
2
```

Node 2 is the center.

---

# Important Property

Only ONE node has

```
Degree = n - 1
```

Degree means

> Number of edges connected to a node.

Example

```
        1
        |
        |
4 ------2------3
        |
        |
        5
```

Count connections.

Node 2

```
Connected to

1
3
4
5
```

Degree

```
4
```

Node 1

```
Degree = 1
```

Node 3

```
Degree = 1
```

Node 4

```
Degree = 1
```

Node 5

```
Degree = 1
```

Only one node has the highest degree.

That is the center.

---

# Understanding the Input

Example

```python
edges = [[1,2],[2,3],[4,2]]
```

Many beginners think

```
What is this 2D array?
```

Actually each row represents one edge.

Let's read them one by one.

---

## First Edge

```
[1,2]
```

Means

```
1 ----- 2
```

---

## Second Edge

```
[2,3]
```

Now graph becomes

```
1

|

2 -----3
```

---

## Third Edge

```
[4,2]
```

Graph becomes

```
      1
      |
      |
4 -----2-----3
```

Now ask yourself

Which node is connected to every other node?

Node

```
2
```

Answer

```
2
```

---

# Example 2

Input

```python
edges =

[
 [1,2],
 [5,1],
 [1,3],
 [1,4]
]
```

Let's draw it.

First edge

```
1 -----2
```

Second edge

```
5

|

1 -----2
```

Third edge

```
5

|

1 -----2

|

3
```

Fourth edge

```
      5
      |
      |
4 -----1-----2
      |
      |
      3
```

Who is connected to everyone?

```
1
```

Answer

```
1
```

---

# How Should We Think?

Suppose someone asks

> Find the most popular person in a group.

What would you do?

Count

How many people know each person.

Exactly the same idea.

Count

How many edges each node has.

The one with the maximum count

↓

Center.

---

# Brute Force Approach

Count the degree of every node.

Example

```
      1
      |
      |
4 -----2-----3
```

Degree

```
1 = 1

2 = 3

3 = 1

4 = 1
```

Maximum

```
2
```

Answer

```
2
```

---

# Dry Run

Input

```python
edges =
[
 [1,2],
 [2,3],
 [4,2]
]
```

Dictionary

Initially

```
{}
```

Read

```
[1,2]
```

Increase degree

```
1 : 1

2 : 1
```

Dictionary

```
{
1:1,
2:1
}
```

---

Read

```
[2,3]
```

Increase degree

```
2 : 2

3 : 1
```

Dictionary

```
{
1:1,
2:2,
3:1
}
```

---

Read

```
[4,2]
```

Increase degree

```
4 : 1

2 : 3
```

Dictionary

```
{
1:1,
2:3,
3:1,
4:1
}
```

Now find maximum.

```
Node 2

Degree = 3
```

Return

```
2
```

---

# Python Solution (Degree Counting)

```python
from collections import defaultdict

class Solution:
    def findCenter(self, edges):

        degree = defaultdict(int)

        for u, v in edges:

            degree[u] += 1
            degree[v] += 1

        for node in degree:

            if degree[node] == len(edges):
                return node
```

---

# Code Explanation

Create dictionary

```python
degree = defaultdict(int)
```

Initially

```
{}
```

---

Read every edge

```python
for u, v in edges:
```

Suppose

```
[2,3]
```

Increase

```python
degree[2] += 1
degree[3] += 1
```

Because

```
2

and

3
```

both gained one connection.

---

Finally

Loop through every node.

```python
for node in degree:
```

Remember

A star graph has

```
n nodes

n-1 edges
```

The center is connected to every other node.

Therefore

Degree of center

```
n-1
```

Since

```
edges = n-1
```

We simply check

```python
degree[node] == len(edges)
```

Return that node.

---

# Can We Do Better?

Yes!

This is where interviewers expect you to think.

---

# Observation

Consider

```
Edges

[1,2]

[2,3]

[2,4]

[2,5]
```

Look only at the first two edges.

```
[1,2]

[2,3]
```

Which node appears in BOTH?

```
2
```

That must be the center.

Why?

Because

Every edge in a star graph must contain the center.

So the common node between the first two edges is always the answer.

---

# Dry Run

```
edges

[
 [1,2],
 [2,3],
 [2,4]
]
```

First edge

```
1

2
```

Second edge

```
2

3
```

Common node

```
2
```

Done.

No need to process the remaining edges.

---

# Optimal Python Solution

```python
class Solution:
    def findCenter(self, edges):

        if edges[0][0] in edges[1]:
            return edges[0][0]

        return edges[0][1]
```

---

# Why Does This Work?

Suppose

```
Center = 5
```

Every edge must contain

```
5
```

Example

```
[5,1]

[5,2]

[5,3]

[5,4]
```

First two edges

```
[5,1]

[5,2]
```

Common node

```
5
```

Always.

---

# Time Complexity

### Degree Counting

```
Time = O(n)

Space = O(n)
```

---

### Optimal Solution

```
Time = O(1)

Space = O(1)
```

---

# Is This a Tree?

Almost.

A star graph is actually a **special type of tree**.

Remember

A tree has

- Connected graph
- No cycles
- n nodes
- n-1 edges

A star graph satisfies all of these.

It is simply a tree where

One node connects to every other node.

```
        Center
       / | | \
      /  | |  \
     A   B C   D
```

---

# Pattern Recognition

Whenever you see

- Star Graph
- Center Node
- One node connected to everyone

Think

```
Find the node with the highest degree

OR

Use the first two edges trick
```

---

# Key Takeaways

- A **Star Graph** is a special kind of **Tree**.
- The center node is connected to every other node.
- The center has **Degree = n - 1**.
- Brute Force: Count degrees.
- Optimal: The common node in the first two edges is always the center.
- This is one of the easiest graph problems, and it's mainly about recognizing the properties of a star graph rather than performing graph traversal.

<br/><br/><br/><br/><br/>

---

# 797. All Paths From Source to Target

## Problem Statement

You are given a **Directed Acyclic Graph (DAG)** represented as an adjacency list.

- Nodes are numbered from **0** to **n - 1**.
- `graph[i]` contains all the nodes that can be reached directly from node `i`.
- You need to find **every possible path** from node `0` (source) to node `n-1` (target).

---

## Example Input

```python
graph = [[1,2],[3],[3],[]]
```

Let's first understand what this graph means.

```
graph[0] = [1,2]
graph[1] = [3]
graph[2] = [3]
graph[3] = []
```

Graph Representation

```
        0
      /   \
     1     2
      \   /
        3
```

Possible paths are

```
0 → 1 → 3

0 → 2 → 3
```

Expected Output

```python
[
    [0,1,3],
    [0,2,3]
]
```

---

# Observation

The graph is a **DAG**.

That means

- No cycles
- No infinite recursion
- We can safely perform DFS.

The idea is

> Start from node 0.

Whenever we reach node `n-1`, we have found one complete path.

Store that path.

Continue exploring the remaining branches.

---

# DFS + Backtracking

Suppose our DFS function is

```python
dfs(node, path)
```

where

- `node` → current node
- `path` → current path from source

---

## Algorithm

```
Append current node to path

If current node is destination
    Save a copy of path
    Return

Visit every neighbour

Remove current node from path
```

Notice the important pattern

```
Append

Explore

Pop
```

This is called **Backtracking**.

---

# Code

```python
class Solution:
    def allPathsSourceTarget(self, graph):
        ans = []
        target = len(graph) - 1

        def dfs(node, path):

            path.append(node)

            if node == target:
                ans.append(path[:])

            else:
                for neighbour in graph[node]:
                    dfs(neighbour, path)

            path.pop()

        dfs(0, [])

        return ans
```

---

# Complete Dry Run

Input

```python
graph = [[1,2],[3],[3],[]]
```

Target

```
3
```

Initially

```
ans = []

path = []
```

Call

```
dfs(0, [])
```

---

## Step 1

Current node

```
0
```

Append

```
path = [0]
```

Not destination.

Neighbours

```
1
2
```

Go to first neighbour.

---

## Step 2

Call

```
dfs(1,[0])
```

Append

```
path = [0,1]
```

Not destination.

Neighbour

```
3
```

Call DFS.

---

## Step 3

Call

```
dfs(3,[0,1])
```

Append

```
path = [0,1,3]
```

Current node is target.

Store copy

```
ans =

[
    [0,1,3]
]
```

Now backtrack.

Pop

```
path = [0,1]
```

Return.

---

Back to node 1.

No more neighbours.

Backtrack.

Pop

```
path = [0]
```

Return.

---

Back to node 0.

Next neighbour is

```
2
```

Call

```
dfs(2,[0])
```

---

## Step 4

Append

```
path = [0,2]
```

Neighbour

```
3
```

Call DFS.

---

## Step 5

Append

```
path = [0,2,3]
```

Destination reached.

Store copy.

```
ans =

[
    [0,1,3],
    [0,2,3]
]
```

Backtrack.

Pop

```
path = [0,2]
```

Return.

---

Back to node 2.

No neighbours.

Pop

```
path = [0]
```

Return.

---

Back to node 0.

No neighbours.

Pop

```
path = []
```

Return.

DFS ends.

Final answer

```python
[
    [0,1,3],
    [0,2,3]
]
```

---

# Complete Recursion Tree

```
dfs(0)

path=[0]

            0
          /   \
         /     \
        1       2
        |       |
        3       3
```

Execution order

```
dfs(0)

    dfs(1)

        dfs(3)
        save [0,1,3]

    dfs(2)

        dfs(3)
        save [0,2,3]
```

---

# How Backtracking Works

When DFS reaches node 3

```
path = [0,1,3]
```

We save it.

Now we return.

Before returning

```
path.pop()
```

Now

```
path = [0,1]
```

Return again.

```
path.pop()
```

Now

```
path = [0]
```

Now DFS can explore

```
0 → 2
```

If we **don't pop**, then

```
path would become

[0,1,3,2]
```

which is incorrect.

That's why every

```
append()
```

must have a matching

```
pop()
```

---

# Why do we use `path[:]`?

Suppose we write

```python
ans.append(path)
```

Current path

```
path = [0,1,3]
```

Later DFS pops

```
path = [0,1]

path = [0]

path = []
```

Since lists are mutable,

the answer also changes.

Finally,

```
ans

[
    []
]
```

or incorrect values.

Instead

```python
ans.append(path[:])
```

creates a **copy**.

```
path

[0,1,3]
```

is copied into

```
[0,1,3]
```

Now future modifications do not affect the stored answer.

---

# Time Complexity

Let

- `P` = Number of valid paths
- `L` = Average length of a path

Each path is copied once.

Time Complexity

```
O(P × L)
```

---

# Space Complexity

Recursion depth

```
O(n)
```

Answer storage

```
O(P × L)
```

Overall

```
O(n + P × L)
```

---

# Key Takeaways

✅ The graph is a DAG, so no `visited` array is needed.

✅ DFS explores one path at a time.

✅ `append()` adds the current node.

✅ `pop()` removes it before returning (Backtracking).

✅ Always store `path[:]` instead of `path`.

✅ The recursion naturally explores every possible path from source to destination.

<br/><br/><br/><br/><br/>

---


# 1557. Minimum Number of Vertices to Reach All Nodes

> How to Think About Graph Problems (Using LeetCode 1557)
---

# Before Anything Else...

If you're struggling with this problem, **it's completely normal**.

This problem is difficult **not because the coding is hard**, but because it tests **how you think** about graphs.

Many beginners (including me when I first learned graphs) immediately think:

> "Should I use DFS?"
>
> "Should I use BFS?"
>
> "Should I use recursion?"

For this problem, **none of those are the first thing you should think about**.

---

# Step 1: Forget the Algorithms

Don't think about DFS, BFS, recursion, or trees.

Instead, ask only one question:

> **What is the problem asking me to find?**

Read the question carefully.

It says:

> Find the **smallest set of vertices** from which **all nodes are reachable**.

Notice what it **doesn't** ask.

It does **not** ask:

- Find a path
- Count paths
- Traverse the graph
- Detect a cycle
- Visit every node using DFS

It simply asks:

> Which nodes **must** be chosen?

That changes everything.

---

# Step 2: Draw the Graph

Input:

```text
n = 6

edges =

[
 [0,1],
 [0,2],
 [2,5],
 [3,4],
 [4,2]
]
```

Let's draw it.

```
        0
       / \
      /   \
     1     2
            |
            |
            v
            5


        3
        |
        |
        v
        4
        |
        |
        v
        2
```

Forget coding.

Just observe.

---

# Step 3: Imagine Starting from Node 0

What nodes can you visit?

```
0

↓

1

↓

2

↓

5
```

Visited

```
0
1
2
5
```

Can you visit

```
3 ?
```

No.

Can you visit

```
4 ?
```

No.

Therefore,

starting only from **0** is not enough.

---

# Step 4: Start from Node 3

Now imagine starting here.

```
3

↓

4

↓

2

↓

5
```

Visited

```
3
4
2
5
```

Now combine both starting points.

```
Start from 0

+

Start from 3
```

Now every node is covered.

```
0
1
2
3
4
5
```

Answer

```text
[0,3]
```

---

# Step 5: Why ONLY 0 and 3?

Now inspect every node.

---

## Node 0

Can anyone reach node 0?

Look at the graph.

```
0
```

No arrows point into it.

Therefore,

if you don't start from 0,

you will never reach 0.

So 0 **must** be chosen.

---

## Node 3

Same question.

Can anyone reach node 3?

No.

No incoming arrows.

Therefore,

3 **must** be chosen.

---

## Node 2

Can someone reach node 2?

Yes.

```
0 → 2

4 → 2
```

So we don't need to start from 2.

Someone else can already reach it.

---

## Node 1

Already reachable.

```
0 → 1
```

No need to start there.

---

## Node 5

Already reachable.

```
2 → 5
```

Again,

no need to start there.

---

# The Pattern

Look at the chosen nodes.

```
0

3
```

What is special about them?

They have

```
NO incoming edges.
```

That is the entire trick.

---

# Think of Water Flow 💧

Imagine arrows are pipes.

Water flows in the direction of the arrows.

```
0

↓

2

↓

5
```

Water naturally reaches

```
2

↓

5
```

Do we need another water source at

```
2
```

No.

Water already arrives there.

---

Now look at

```
3

↓

4
```

Nothing brings water into 3.

So we must place a water source there.

---

This idea leads to a very important graph concept.

---

# Indegree

Indegree means

> **How many arrows come INTO a node?**

Example

```
0 → 2

3 → 2

5 → 2
```

Three arrows enter node 2.

```
Indegree(2) = 3
```

---

Example

```
0
```

No arrows enter.

```
Indegree(0) = 0
```

---

# The Main Observation

Every node with

```
Indegree = 0
```

must be included in the answer.

Why?

Because nothing can reach it.

If nothing reaches it,

the only way to visit it

is to start there.

---

# Dry Run

Input

```
edges

0 → 1

0 → 2

2 → 5

3 → 4

4 → 2
```

Initially

```
indegree

0 0 0 0 0 0
```

---

Process edge

```
0 → 1
```

Increase indegree of 1.

```
0 1 0 0 0 0
```

---

Process

```
0 → 2
```

```
0 1 1 0 0 0
```

---

Process

```
2 → 5
```

```
0 1 1 0 0 1
```

---

Process

```
3 → 4
```

```
0 1 1 0 1 1
```

---

Process

```
4 → 2
```

```
0 1 2 0 1 1
```

Final indegree array

| Node | 0 | 1 | 2 | 3 | 4 | 5 |
|------|---|---|---|---|---|---|
| Indegree | 0 | 1 | 2 | 0 | 1 | 1 |

Nodes having indegree 0

```
0

3
```

Answer

```
[0,3]
```

---

# Code

```python
class Solution:
    def findSmallestSetOfVertices(self, n, edges):

        indegree = [0] * n

        for u, v in edges:
            indegree[v] += 1

        answer = []

        for node in range(n):
            if indegree[node] == 0:
                answer.append(node)

        return answer
```

---

# Why Didn't We Use DFS?

This is an important lesson.

Many graph problems require DFS.

This one doesn't.

Why?

Because the question is asking

> "Which nodes cannot be reached by anyone else?"

To answer that,

we only need to know

how many incoming edges each node has.

Traversal is unnecessary.

---

# How Should You Think During Interviews?

Instead of immediately thinking

```
DFS?

BFS?

Recursion?
```

follow this process.

---

## Step 1

Ask

> **What exactly is the question asking?**

Examples

- Can I reach a node?
- Shortest path?
- Connected components?
- Minimum starting points?
- Detect cycles?

---

## Step 2

Ask

> **What information do I need?**

For this problem,

you need to know

```
Which nodes are unreachable from others?
```

That immediately suggests

```
Incoming edges
```

instead of traversal.

---

## Step 3

Ask

> **Do I actually need DFS or BFS?**

Sometimes the answer is yes.

Sometimes the answer is no.

For this problem,

the answer is

```
No.
```

---

# A Better Way to Learn Graphs

Instead of memorizing algorithms,

classify problems first.

| If the problem asks... | Think about... |
|-------------------------|----------------|
| Can I reach a node? | DFS / BFS |
| Number of connected components | DFS / BFS |
| Detect cycles | DFS or Kahn's Algorithm |
| Shortest path (unweighted) | BFS |
| Shortest path (weighted) | Dijkstra |
| Valid task ordering | Topological Sort |
| Minimum starting vertices | Indegree |
| Course prerequisites | Topological Sort |

This habit is what experienced programmers develop.

---

# Your Biggest Concern

> "I can't imagine recursion or trees in my mind."

Here's something important:

**You don't always need recursion.**

Many graph problems have nothing to do with recursive thinking.

Recursion is just one tool.

The real skill is recognizing **what property** the problem is asking about.

Once you identify the property,

choosing the algorithm becomes much easier.

---

# How to Improve Your Thinking

When solving every graph problem, follow these questions **in order**:

1. What is the problem asking?
2. Is this about paths, groups, ordering, cycles, or dependencies?
3. What information do I need?
4. Can I solve it without traversal?
5. If traversal is needed, should I use DFS or BFS?

Don't start with the algorithm.

Start with the **problem's goal**.

---

# My Assessment of Your Progress

From the kinds of questions you've been asking, here's what I notice:

✅ You understand how graphs are represented.

✅ You know the basics of DFS and BFS.

✅ You're practicing medium-level graph problems.

The main thing you're missing is **pattern recognition**.

Right now, you're asking:

> "Which algorithm should I use?"

Try changing that to:

> "What property is this problem asking me to find?"

That small change in thinking will make graph problems much easier over time.

---

# Final Takeaway

Whenever you see a graph problem:

❌ Don't immediately think:

```
DFS?

BFS?

Recursion?
```

✅ Instead, ask:

```
What is the problem asking me to find?
```

Once you answer that question, the right algorithm—or sometimes no traversal at all—often becomes much clearer.

<br/><br/><br/><br/><br/>

---

# 994. Rotting Oranges

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

<br/><br/><br/><br/><br/>

---

# 547. Number of Provinces

## Difficulty
Medium

---

# Problem Statement

There are **n cities**.

Some cities are connected by roads.

Some cities are not connected.

A city can be connected in two ways:

- **Directly Connected**
- **Indirectly Connected**

Your task is to find **how many separate groups of connected cities exist.**

Each group is called a **Province**.

---

# Understanding the Terms

## 1. Direct Connection

Suppose we have three cities.

```
0 ----- 1
```

City 0 and City 1 have a road.

So they are **directly connected**.

---

## 2. Indirect Connection

Now suppose

```
0 ----- 1 ----- 2
```

Notice

There is **no direct road**

```
0 --------X-------- 2
```

But city 0 can still reach city 2 by travelling

```
0 → 1 → 2
```

Therefore,

City 0 and City 2 are **indirectly connected**.

---

# What is a Province?

A province is simply a group of cities where **every city can reach every other city** either directly or indirectly.

For example,

```
0 ----- 1 ----- 2
```

All cities are connected.

So

```
Province 1

0
1
2
```

Only **one province** exists.

---

Now consider

```
0 ----- 1


2 ----- 3


4
```

Here,

Cities 0 and 1 form one group.

Cities 2 and 3 form another group.

City 4 is alone.

Therefore

```
Province 1 = {0,1}

Province 2 = {2,3}

Province 3 = {4}
```

Answer = **3**

---

# Understanding the Input Matrix

The input is

```python
isConnected =
[
 [1,1,0],
 [1,1,0],
 [0,0,1]
]
```

Many people get confused here.

Let's understand it carefully.

Rows represent the current city.

Columns tell whether that city is connected to another city.

Let's label everything.

```
      0 1 2

0 -> [1 1 0]

1 -> [1 1 0]

2 -> [0 0 1]
```

---

## Reading Row 0

```
[1 1 0]
```

Means

City 0 is connected to

```
City 0 ✔

City 1 ✔

City 2 ✘
```

---

## Reading Row 1

```
[1 1 0]
```

Means

City 1 is connected to

```
City 0 ✔

City 1 ✔

City 2 ✘
```

---

## Reading Row 2

```
[0 0 1]
```

Means

City 2 is connected to

```
City 0 ✘

City 1 ✘

City 2 ✔
```

---

# Convert Matrix into a Graph

Instead of thinking about the matrix,

draw it.

```
0 ----- 1


2
```

Now the question becomes very easy.

How many separate groups?

```
{0,1}

{2}
```

Answer

```
2
```

---

# Key Observation

The problem is asking

> "How many disconnected groups are there?"

Whenever you see

- Connected
- Reachable
- Group of nodes
- Components

Think

> **Connected Components**

---

# How Should We Solve It?

Suppose we have

```
0 ----- 1 ----- 2


3 ----- 4
```

Let's start from City 0.

Visit

```
0
```

Can we reach another city?

Yes

```
0

↓

1
```

Can we continue?

Yes

```
0

↓

1

↓

2
```

Can we go anywhere else?

No.

So

```
0
1
2
```

belongs to one province.

---

Now move to the next city.

City 1

Already visited.

Skip it.

City 2

Already visited.

Skip it.

City 3

Not visited.

Start exploring again.

```
3

↓

4
```

Now

```
3
4
```

becomes another province.

Answer

```
2 Provinces
```

---

# Which Algorithm?

Whenever we need to

- Visit every connected node

we use either

- DFS
- BFS

Both work perfectly.

Most people solve this using DFS.

---

# Dry Run

Input

```python
isConnected =

[
 [1,1,0],
 [1,1,0],
 [0,0,1]
]
```

Initially

```
Visited

[False, False, False]

Province = 0
```

---

## Start from City 0

Not visited.

Increase province.

```
Province = 1
```

Visit City 0.

```
Visited

[T, F, F]
```

Look at row 0.

```
[1,1,0]
```

Connected to

```
City 0

City 1
```

City 1 is unvisited.

Visit it.

```
Visited

[T, T, F]
```

Look at row 1.

```
[1,1,0]
```

Connected to

```
City 0

City 1
```

Already visited.

Done.

---

Move to City 1.

Already visited.

Skip.

---

Move to City 2.

Not visited.

Increase province.

```
Province = 2
```

Visit City 2.

Visited

```
[T,T,T]
```

Done.

No more cities.

Answer

```
2
```

---

# Example 2

```
[
 [1,0,0],
 [0,1,0],
 [0,0,1]
]
```

Graph

```
0


1


2
```

Each city is isolated.

Start from 0

Province = 1

Start from 1

Province = 2

Start from 2

Province = 3

Answer

```
3
```

---

# Python Code (DFS)

```python
class Solution:
    def findCircleNum(self, isConnected):
        n = len(isConnected)

        visited = [False] * n

        provinces = 0

        def dfs(city):

            visited[city] = True

            for neighbor in range(n):

                if (
                    isConnected[city][neighbor] == 1
                    and not visited[neighbor]
                ):
                    dfs(neighbor)

        for city in range(n):

            if not visited[city]:

                provinces += 1
                dfs(city)

        return provinces
```

---

# Code Explanation

## Step 1

Find number of cities.

```python
n = len(isConnected)
```

Suppose

```
[
 [1,1,0],
 [1,1,0],
 [0,0,1]
]
```

Then

```
n = 3
```

---

## Step 2

Create a visited array.

```python
visited = [False] * n
```

Initially

```
[False, False, False]
```

It tells us which cities are already explored.

---

## Step 3

Province counter.

```python
provinces = 0
```

Initially

```
0
```

---

## Step 4

DFS Function

```python
def dfs(city):
```

This function visits **every city connected to the current city**.

---

First mark the city visited.

```python
visited[city] = True
```

Suppose

```
City = 0
```

Visited becomes

```
[T,F,F]
```

---

Now check every possible neighbor.

```python
for neighbor in range(n):
```

For City 0

```
Neighbor = 0

Neighbor = 1

Neighbor = 2
```

---

If

```python
isConnected[city][neighbor] == 1
```

and

```python
neighbor not visited
```

then

```python
dfs(neighbor)
```

Keep exploring.

---

## Step 5

Main Loop

```python
for city in range(n):
```

Visit every city.

If

```python
not visited[city]
```

means

New province found.

Increase answer.

```python
provinces += 1
```

Then explore the entire province.

```python
dfs(city)
```

---

# Why Does This Work?

Suppose

```
0 ----- 1 ----- 2
```

Starting DFS from 0 automatically visits

```
0

↓

1

↓

2
```

So later,

when the loop reaches

```
1
```

Already visited.

Skip.

When it reaches

```
2
```

Already visited.

Skip.

One DFS call explored **one complete province**.

Every new DFS call means we discovered another disconnected group.

---

# Time Complexity

There are **n cities**.

For every city, we may scan its entire row in the matrix.

```
Time = O(n²)
```

---

# Space Complexity

Visited array

```
O(n)
```

Recursive DFS stack

Worst case

```
O(n)
```

Overall

```
Space = O(n)
```

---

# Pattern Recognition

Whenever you see words like

- Connected directly or indirectly
- Count groups
- Connected components
- Reachable cities
- Friend circles
- Number of groups
- Network clusters

Immediately think

```
DFS / BFS

Connected Components
```

---

# Key Takeaways

- A **province** is just a **connected component** in a graph.
- The matrix represents an **undirected graph**.
- Convert the matrix mentally into a graph.
- Start DFS/BFS from every **unvisited** city.
- Each new DFS/BFS call discovers exactly **one province**.
- Count the number of DFS/BFS calls.

This is one of the classic **Connected Components** problems and is often the first graph traversal problem asked in coding interviews.

<br/><br/><br/><br/><br/>

---

# 207. Course Schedule

## Difficulty
Medium

---

# Problem Statement

There are **numCourses** courses numbered from

```
0 → numCourses - 1
```

Some courses have prerequisites.

A prerequisite means:

> **You must complete one course before taking another.**

You are given an array

```python
prerequisites = [[a, b]]
```

which means

```
To study course a,
you MUST finish course b first.
```

Your task is simple.

Return

```
True
```

if it is possible to finish **all courses**.

Otherwise return

```
False
```

---

# Understanding the Question

Suppose there are

```
2 courses

0
1
```

and

```python
prerequisites = [[1,0]]
```

This means

```
To study course 1

↓

You must first complete

↓

Course 0
```

Draw it like this

```
0  ----->  1
```

Meaning

```
Take 0 first

then

take 1
```

Possible?

Yes.

Answer

```
True
```

---

# Example 2

```
Courses

0
1
```

Prerequisites

```python
[[1,0],
 [0,1]]
```

Let's draw it.

First pair

```
0 -----> 1
```

Second pair

```
1 -----> 0
```

Now the graph becomes

```
0 ------> 1
^          |
|          |
|__________|
```

Think carefully.

To study

```
Course 0
```

You need

```
Course 1
```

But

To study

```
Course 1
```

You need

```
Course 0
```

Which one can you start?

None.

This is impossible.

Answer

```
False
```

---

# What is Actually Happening?

Forget courses.

Imagine tasks.

```
Task A

depends on

Task B
```

You must finish

```
B

↓

then

A
```

Easy.

But suppose

```
A depends on B

B depends on C

C depends on A
```

Can you start?

No.

Because

```
A needs C

↓

C needs B

↓

B needs A
```

You're stuck forever.

This is called a

# Cycle

---

# Biggest Observation

Whenever a problem says

- before
- prerequisite
- dependency
- order
- schedule

Immediately think

```
Directed Graph
```

because

```
Course 0

↓

Course 1
```

has a direction.

You cannot go backwards.

---

# How Should We Draw the Graph?

Suppose

```python
prerequisites =
[
 [1,0],
 [2,0],
 [3,1],
 [3,2]
]
```

Meaning

```
To study 1

Need 0
```

```
0 → 1
```

Next

```
To study 2

Need 0
```

```
0 → 2
```

Next

```
To study 3

Need 1
```

```
1 → 3
```

Next

```
To study 3

Need 2
```

```
2 → 3
```

Graph

```
        0
      /   \
     1     2
      \   /
        3
```

Possible?

Yes.

One order is

```
0

↓

1

↓

2

↓

3
```

or

```
0

↓

2

↓

1

↓

3
```

Both work.

Answer

```
True
```

---

# When Does It Become Impossible?

Suppose

```
0 → 1

1 → 2

2 → 0
```

Graph

```
0 → 1
↑    |
|    ↓
2 ←--
```

Can you start anywhere?

No.

Every course depends on another.

Impossible.

---

# What Are We Actually Checking?

We are NOT finding

- shortest path
- longest path
- minimum cost

We only want to know

> **Does this graph contain a cycle?**

If

```
Cycle exists

↓

False
```

If

```
No cycle

↓

True
```

---

# Why?

Suppose

```
A

↓

B

↓

C
```

Easy.

Complete

```
A

↓

B

↓

C
```

Done.

Now suppose

```
A

↓

B

↓

C

↓

A
```

Can never start.

---

# How Do We Detect a Cycle?

There are two common methods.

## Method 1

DFS + Recursion Stack

(Used very often in interviews)

---

## Method 2

Topological Sort (Kahn's Algorithm)

We'll first understand DFS.

---

# DFS Idea

Imagine

```
0

↓

1

↓

2
```

Start DFS.

Visit

```
0
```

Then

```
1
```

Then

```
2
```

No cycle.

Return.

---

Now imagine

```
0

↓

1

↓

2

↓

0
```

DFS

```
0

↓

1

↓

2

↓

0
```

Wait.

We are trying to visit

```
0
```

again

while it is still inside our current DFS path.

That means

```
Cycle Found
```

---

# Three States

Instead of only

```
Visited
```

we use

```
0 = Not Visited

1 = Visiting

2 = Finished
```

---

Initially

```
Course

0 1 2

State

0 0 0
```

---

Suppose

Visit 0

```
1 0 0
```

Visit 1

```
1 1 0
```

Visit 2

```
1 1 1
```

If 2 again reaches

```
0
```

State of 0 is still

```
1

(Visiting)
```

Meaning

Cycle.

---

If DFS finishes

```
0
```

Mark

```
2
```

Meaning

```
Finished

Never revisit.
```

---

# Dry Run

Example

```python
numCourses = 4

prerequisites =
[
 [1,0],
 [2,1],
 [3,2]
]
```

Graph

```
0

↓

1

↓

2

↓

3
```

State

```
0 0 0 0
```

Visit

```
0

↓

1

↓

2

↓

3
```

No cycle.

Finish

```
2 2 2 2
```

Answer

```
True
```

---

# Dry Run 2

```
0

↓

1

↓

2

↓

0
```

State

```
0 0 0
```

Visit

```
1 0 0
```

Visit

```
1 1 0
```

Visit

```
1 1 1
```

Now

2 goes to

0

State of 0

```
1

Still visiting
```

Cycle found.

Return

```
False
```

---

# Python Code (DFS)

```python
from collections import defaultdict

class Solution:
    def canFinish(self, numCourses, prerequisites):

        graph = defaultdict(list)

        # Build graph
        for course, prereq in prerequisites:
            graph[prereq].append(course)

        # 0 = Unvisited
        # 1 = Visiting
        # 2 = Finished
        state = [0] * numCourses

        def dfs(course):

            # Cycle detected
            if state[course] == 1:
                return False

            # Already processed
            if state[course] == 2:
                return True

            # Mark as visiting
            state[course] = 1

            # Visit neighbours
            for neighbor in graph[course]:
                if not dfs(neighbor):
                    return False

            # Finished
            state[course] = 2

            return True

        # Start DFS from every course
        for course in range(numCourses):
            if state[course] == 0:
                if not dfs(course):
                    return False

        return True
```

---

# Code Explanation

## Step 1

Create graph.

```python
graph = defaultdict(list)
```

Suppose

```python
[[1,0],[2,0]]
```

Graph becomes

```
0

↓

1

↓

2
```

Stored as

```python
graph[0] = [1,2]
```

---

## Step 2

State array

```python
state = [0] * numCourses
```

Initially

```
0 0 0 0
```

Meaning

Nobody visited.

---

## Step 3

DFS

```python
dfs(course)
```

Visit current course.

---

If

```python
state == 1
```

Means

Already inside current DFS path.

Cycle.

Return False.

---

If

```python
state == 2
```

Already completely explored.

Return True.

---

Otherwise

Mark

```python
state = 1
```

Meaning

Currently exploring.

---

Visit every neighbour.

```python
for neighbor in graph[course]:
```

Continue DFS.

---

After finishing

Mark

```python
state = 2
```

Meaning

Finished forever.

---

# Complexity

Suppose

```
V = Courses

E = Prerequisites
```

Every course is visited once.

Every edge is explored once.

```
Time = O(V + E)
```

Space

Graph

```
O(V + E)
```

DFS Stack

```
O(V)
```

---

# Pattern Recognition

Whenever you see

- Course Schedule
- Dependency
- Task Ordering
- Build System
- Package Installation
- Before / After
- Can Finish
- Circular Dependency

Immediately think

```
Directed Graph
```

Then ask yourself

> **"Is there a cycle?"**

If

```
Cycle exists

↓

Impossible
```

If

```
No cycle

↓

Possible
```

---

# Key Takeaways

- This is **NOT** a tree problem. It is a **Directed Graph** problem because dependencies have a direction.
- Convert each prerequisite `[a, b]` into a directed edge `b → a`.
- The real question is: **Does the directed graph contain a cycle?**
- Use **DFS with three states** (Unvisited, Visiting, Finished) or **Topological Sort (Kahn's Algorithm)** to detect cycles.
- **Cycle → False (cannot finish all courses).**
- **No Cycle → True (all courses can be completed).**