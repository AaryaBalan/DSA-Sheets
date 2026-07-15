# Graph Problems

Welcome to the graph problems section! Here you will find various data structure and algorithm problems related to Graphs, along with their detailed explanations, intuition, and optimal solutions.

## Questions

- [1791. Find Center of Star Graph](#1791-find-center-of-star-graph)
- [797. All Paths From Source to Target](#797-all-paths-from-source-to-target)
- [1557. Minimum Number of Vertices to Reach All Nodes](#1557-minimum-number-of-vertices-to-reach-all-nodes)
- [841. Keys and Rooms](#841-keys-and-rooms)
- [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance](#1334-find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance)
- [994. Rotting Oranges](#994-rotting-oranges)
- [547. Number of Provinces](#547-number-of-provinces)
- [207. Course Schedule](#207-course-schedule)
- [1584. Min Cost to Connect All Points](#1584-min-cost-to-connect-all-points)
- [130. Surrounded Regions](#130-surrounded-regions)
- [1020. Number of Enclaves](#1020-number-of-enclaves)
- [3249. Count the Number of Good Nodes](#3249-count-the-number-of-good-nodes)
- [200. Number of Islands](#200-number-of-islands)
- [210. Course Schedule II](#210-course-schedule-ii)

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

# 841. Keys and Rooms

## Difficulty
Medium

---

# Problem Statement

There are **n rooms** numbered from

```
0 → n-1
```

Initially,

- Room **0** is already unlocked.
- Every other room is locked.

Inside every room, there may be some **keys**.

Each key opens another room.

Your goal is to determine whether it is possible to visit **every room**.

Return

```
True
```

if all rooms can be visited.

Otherwise return

```
False
```

---

# Understanding the Question

Imagine there are four rooms.

```
Room 0

Room 1

Room 2

Room 3
```

Initially

```
Only Room 0 is open.
```

Inside Room 0 you find

```
Key 1
```

Now you can open

```
Room 1
```

Inside Room 1

```
Key 2
```

Now you can open

```
Room 2
```

Inside Room 2

```
Key 3
```

Now you can open

```
Room 3
```

All rooms visited.

Answer

```
True
```

---

# Understanding the Input

Input

```python
rooms = [[1],[2],[3],[]]
```

This means

Room 0 contains

```
Key 1
```

Room 1 contains

```
Key 2
```

Room 2 contains

```
Key 3
```

Room 3 contains

```
No keys
```

Let's draw it.

```
Room 0

↓

Room 1

↓

Room 2

↓

Room 3
```

Can we visit every room?

Yes.

Answer

```
True
```

---

# Example 2

Input

```python
rooms =

[
 [1,3],
 [3,0,1],
 [2],
 [0]
]
```

Let's draw it.

Room 0

contains

```
1

3
```

So

```
0

↓

1

↓

3
```

Room 1 contains

```
3

0

1
```

Still

```
0

1

3
```

Room 3

contains

```
0
```

Nothing new.

Now ask

Can we enter

```
Room 2
```

No.

Because

The only key to Room 2 is inside

```
Room 2
```

Impossible.

Answer

```
False
```

---

# Biggest Observation

Think about the rooms as graph nodes.

```
Room

↓

Node
```

Keys become

```
Edges
```

Example

```
Room 0 contains key 1

↓

0 → 1
```

Room 1 contains

```
Key 2

↓

1 → 2
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

Now the problem becomes

> Starting from node **0**, can we visit every node?

---

# Which Algorithm?

We need to

- Start from one node.
- Visit every reachable node.

This is exactly

```
Graph Traversal
```

We can use

- DFS
- BFS

Both work perfectly.

---

# How Should We Think?

Suppose you're inside a building.

Initially

```
Only Room 0 is open.
```

You collect every key.

Whenever you find a new key,

you immediately visit that room.

Repeat until

there are no new rooms left.

Finally ask

```
Did I visit every room?
```

If yes

```
True
```

Otherwise

```
False
```

---

# Dry Run

Input

```python
rooms =

[
 [1],
 [2],
 [3],
 []
]
```

Initially

Queue

```
0
```

Visited

```
{0}
```

---

Pop

```
0
```

Keys

```
1
```

Visit

```
1
```

Queue

```
1
```

Visited

```
0

1
```

---

Pop

```
1
```

Keys

```
2
```

Visit

```
2
```

Queue

```
2
```

Visited

```
0

1

2
```

---

Pop

```
2
```

Keys

```
3
```

Visit

```
3
```

Queue

```
3
```

Visited

```
0

1

2

3
```

---

Pop

```
3
```

No keys.

Queue

Empty.

Visited

```
4 rooms
```

Total rooms

```
4
```

Answer

```
True
```

---

# Your Solution

```python
class Solution:
    def canVisitAllRooms(self, rooms):

        q = deque([rooms[0]])

        seen = set()

        seen.add(0)

        while q:

            node = q.popleft()

            for room in node:

                if room not in seen:

                    seen.add(room)

                    q.append(rooms[room])

        return len(seen) == len(rooms)
```

---

# Is Your Code Correct?

Yes.

It works correctly.

Instead of storing

```
Room Number
```

inside the queue,

you store

```
Keys inside the room.
```

For example

Queue

```
[[1]]
```

instead of

```
[0]
```

Both work.

---

# Cleaner BFS Solution

Most interviewers write

```python
from collections import deque

class Solution:
    def canVisitAllRooms(self, rooms):

        queue = deque([0])

        visited = {0}

        while queue:

            room = queue.popleft()

            for key in rooms[room]:

                if key not in visited:

                    visited.add(key)

                    queue.append(key)

        return len(visited) == len(rooms)
```

Notice

Queue stores

```
Room Numbers
```

instead of

```
Key Lists
```

This is easier to read and debug.

---

# Time Complexity

Suppose

```
V = Rooms

E = Total Keys
```

Every room is visited once.

Every key is processed once.

```
Time = O(V + E)
```

---

# Space Complexity

Visited

```
O(V)
```

Queue

```
O(V)
```

Overall

```
O(V)
```

---

# Pattern Recognition

Whenever you read

- Start from one node
- Can reach all nodes?
- Visit every room
- Explore all reachable nodes

Immediately think

```
Graph Traversal

↓

DFS

or

BFS
```

---

# Key Takeaways

- Treat every room as a **graph node**.
- Treat every key as a **directed edge**.
- The question becomes:
  > "Starting from node 0, can I reach every node?"
- This is a **graph traversal** problem.
- Both **DFS** and **BFS** solve it.
- Your solution is correct, but storing room numbers in the queue is a cleaner and more standard approach.

<br/><br/><br/><br/><br/>

---

# 1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance

---

# Step 1: Understand the Question

You are given:

- `n` cities numbered from `0` to `n-1`
- Roads connecting some cities
- Every road has a **distance (weight)**
- A `distanceThreshold`

Your task is:

For **every city**, find **how many other cities can be reached** whose **shortest distance** is **less than or equal to** `distanceThreshold`.

Finally,

- Choose the city with the **smallest number of reachable cities**.
- If multiple cities have the same count, return the **largest city number**.

---

## Important Keyword

The most important sentence is:

> reachable through **some path**

Notice it does **NOT** say direct road.

It means you are allowed to travel through intermediate cities.

Example:

```
0 ----3---- 1 ----2---- 2
```

Distance from 0 to 2 is

```
3 + 2 = 5
```

Even though there is no direct road.

So this immediately tells us:

> We need the **shortest distance** between every pair of cities.

---

# Example 1

```
n = 4

Edges

0 ---3--- 1
           | \
          1|  \4
           |   \
           2---1---3
```

Threshold = 4

Let's calculate.

---

## City 0

Reachable cities:

```
0 → 1 = 3

0 → 2

0→1→2

3+1=4
```

Cannot reach city 3 because

```
0→1→2→3

3+1+1=5
```

Greater than threshold.

Reachable

```
1
2
```

Count = 2

---

## City 1

```
1→0 = 3

1→2 = 1

1→3

1→2→3

1+1=2
```

Reachable

```
0
2
3
```

Count = 3

---

## City 2

Reachable

```
0
1
3
```

Count = 3

---

## City 3

Reachable

```
1
2
```

Count = 2

---

Smallest count

```
City 0 -> 2

City 3 -> 2
```

Tie.

Question says

> Return largest city number.

Answer

```
3
```

---

# Step 2: Identify the Topic

Ask yourself

What is this?

```
Cities

Roads

Weights
```

This is a **weighted graph**.

---

Now ask

What do I need?

```
Shortest distance
```

So this becomes a **Shortest Path Problem**.

---

# Step 3: Which Algorithm?

Whenever you see

```
Shortest Path
```

Think of

- Dijkstra
- Bellman Ford
- Floyd Warshall

Now check constraints.

```
n <= 100
```

Very small.

That means

```
100³

=

1,000,000
```

Only one million operations.

Perfect for Floyd-Warshall.

---

# Why Floyd-Warshall?

Suppose

```
0 -----10-----1

0---4---2---3---1
```

Current distance

```
0→1 = 10
```

Going through city 2

```
4+3=7
```

Smaller.

So update

```
dist[0][1]=7
```

Floyd-Warshall keeps asking

```
Can going through city k make this path shorter?
```

Formula

```
dist[i][j]

=

min(

dist[i][j],

dist[i][k]+dist[k][j]

)
```

---

# Step 4: Algorithm

## Initialize matrix

Initially

```
      0    1    2    3

0     0   inf  inf  inf

1    inf   0   inf  inf

2    inf inf    0   inf

3    inf inf  inf    0
```

---

## Fill direct roads

Suppose

```
0-1 = 3

1-2 = 1
```

Matrix

```
0   3  inf inf

3   0   1  inf

inf 1   0  inf

inf inf inf 0
```

---

## Floyd-Warshall

```
for k

    for i

        for j

            if dist[i][k]+dist[k][j] < dist[i][j]:

                update
```

Now every shortest distance is known.

---

## Count reachable cities

For every city

```
count=0

for every city

    if distance<=threshold

        count++
```

Ignore itself.

---

## Keep answer

Need

```
Minimum count
```

Tie

```
Largest city
```

Easy trick

```
if count<=minimum:

    answer=i
```

Notice

```
<=
```

Not

```
<
```

Later city automatically replaces earlier one.

---

# Python Code

```python
from typing import List

class Solution:
    def findTheCity(self, n: int, edges: List[List[int]], distanceThreshold: int) -> int:

        INF = float('inf')

        # Step 1: Create distance matrix
        dist = [[INF] * n for _ in range(n)]

        # Distance from a city to itself is 0
        for i in range(n):
            dist[i][i] = 0

        # Step 2: Fill direct roads
        for u, v, w in edges:
            dist[u][v] = w
            dist[v][u] = w

        # Step 3: Floyd-Warshall
        for k in range(n):
            for i in range(n):
                for j in range(n):

                    if dist[i][k] + dist[k][j] < dist[i][j]:

                        dist[i][j] = dist[i][k] + dist[k][j]

        # Step 4: Find answer
        min_count = float('inf')
        answer = -1

        for i in range(n):

            count = 0

            for j in range(n):

                if i != j and dist[i][j] <= distanceThreshold:
                    count += 1

            if count <= min_count:
                min_count = count
                answer = i

        return answer
```

---

# Dry Run

Suppose

```
n = 4

Threshold = 4
```

After Floyd matrix becomes

```
      0 1 2 3

0     0 3 4 5

1     3 0 1 2

2     4 1 0 1

3     5 2 1 0
```

Now count.

City 0

```
3
4
5
```

Reachable

```
3

4
```

Count

```
2
```

---

City 1

```
3

1

2
```

Count

```
3
```

---

City 2

Count

```
3
```

---

City 3

Reachable

```
2

1
```

Count

```
2
```

Minimum

```
2
```

Cities

```
0

3
```

Largest

```
3
```

Answer

```
3
```

---

# Time Complexity

Creating matrix

```
O(n²)
```

Floyd

```
O(n³)
```

Counting

```
O(n²)
```

Overall

```
O(n³)
```

Since

```
n<=100
```

This is perfectly acceptable.

---

# How to Think During an Interview

Most people get stuck because they try to code immediately.

Instead, follow these questions.

---

## Question 1

What is the input?

```
Cities

Roads

Weights
```

Immediately think

> Weighted Graph

---

## Question 2

What is being asked?

```
Reachable cities
```

Reachable using

```
some path
```

So we need

```
Shortest Path
```

---

## Question 3

Do I need shortest path from one city or every city?

Here,

We must check every city.

So we need

```
All-Pairs Shortest Path
```

---

## Question 4

Which algorithm?

Small constraints

```
n<=100
```

means

```
Floyd-Warshall
```

Large constraints

```
n=100000
```

would mean

```
Run Dijkstra from every city
```

---

# General Thinking Pattern for Graph Problems

Whenever you see a graph question, ask these in order.

### Step 1

Is it a graph?

Look for words like

```
Cities

Roads

Flights

Nodes

Edges
```

---

### Step 2

What is being asked?

- Connectivity?
- Shortest Path?
- Cycle?
- Minimum Cost?
- Topological Order?

---

### Step 3

If it is Shortest Path

Ask

```
One source?

or

Every source?
```

One source

```
Dijkstra
```

Every source

```
Floyd-Warshall

or

Run Dijkstra from every node
```

---

### Step 4

Always check constraints.

Small `n`

```
100
```

Usually allows

```
O(n³)
```

Large `n`

```
100000
```

Needs

```
O(E log V)
```

---

# Interview Summary

Think in this exact order:

```
Input
↓

Graph?

↓

Weighted?

↓

Need shortest path?

↓

One source or all sources?

↓

Check constraints

↓

Choose algorithm

↓

Compute shortest paths

↓

Count reachable cities

↓

Return minimum count

↓

If tie → largest index
```

If you practice following this flow on every graph problem, you'll quickly identify the right algorithm instead of getting stuck on where to begin.

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

<br/><br/><br/><br/><br/>

---

# 1584. Min Cost to Connect All Points

**Difficulty:** Medium

**Topics**
- Graph
- Minimum Spanning Tree (MST)
- Greedy
- Prim's Algorithm
- Kruskal's Algorithm
- Heap (Priority Queue)

---

# Problem Statement

You are given `n` points on a 2D plane.

```
points[i] = [x, y]
```

Connecting two points costs:

```
|x1-x2| + |y1-y2|
```

(Manhattan Distance)

Return the **minimum cost** required to connect all points.

There should be exactly **one path** between every pair of points.

---

# First Step (Don't Think About Algorithms Yet)

Forget MST.

Read the question carefully.

The problem is simply saying:

"I have several cities."

Connecting two cities has some cost.

I want every city connected.

Spend the least money.

Example:

```
A ----- B
 \      |
  \     |
   C ---D
```

Many possible connections exist.

Question:

Which roads should I build?

---

# Important Observation

It NEVER asks:

> Find shortest path.

It NEVER asks:

> Visit every node.

It NEVER asks:

> Reach destination.

Instead it asks:

> Connect everything with minimum total cost.

That sentence should immediately remind you of

# Minimum Spanning Tree (MST)

---

# What is a Minimum Spanning Tree?

Suppose we have

```
A
B
C
D
```

Possible roads:

```
A-B = 4
A-C = 2
A-D = 8

B-C = 5
B-D = 3

C-D = 6
```

We DON'T need every road.

We only need enough roads so every city is reachable.

For 4 cities,

Minimum roads needed

```
4-1 = 3
```

because a tree with N nodes always has N-1 edges.

Now choose cheapest roads.

Example:

```
A-C =2
B-D =3
A-B =4

Total =9
```

That's an MST.

---

# Pattern Recognition

Whenever you see

```
Connect all nodes
Minimum total cost
No cycles
```

Think immediately

```
Minimum Spanning Tree
```

There are only two famous algorithms.

1. Prim's Algorithm
2. Kruskal's Algorithm

---

# Which One Should I Use?

For this problem

Use

# Prim's Algorithm

Because

We don't need to build every edge explicitly.

Kruskal would require

```
n² edges
```

which is unnecessary.

---

# Before Thinking About Heap

Imagine this.

Cities

```
A

B

C

D
```

Suppose you're standing at A.

Question:

Which city should you connect next?

Obviously,

the cheapest one.

Then again,

after adding that city,

again choose

the cheapest possible connection.

Notice something?

We repeatedly need

```
minimum cost edge
```

Whenever we repeatedly need the minimum,

think

```
Min Heap
```

---

# Prim's Algorithm Intuition

Start from any point.

Suppose

```
A
```

Visited

```
A
```

Now calculate costs

```
A→B =4
A→C =2
A→D =8
```

Heap

```
2,C

4,B

8,D
```

Pop smallest.

```
C
```

Visit C.

Now add

```
C→B

C→D
```

Heap becomes

```
4,B
5,B
6,D
8,D
```

Pop smallest.

If already visited,

ignore.

Continue until all nodes visited.

That's Prim.

---

# Why Does It Work?

Every time,

we connect

one new point

using

the cheapest possible edge.

Greedy choice.

Mathematically,

this always produces an MST.

---

# Dry Run

Input

```
[[0,0],
 [2,2],
 [3,10],
 [5,2],
 [7,0]]
```

Let's name them

```
0
1
2
3
4
```

---

## Step 1

Visit

```
0
```

Cost

```
0
```

Heap

```
4 ->1

13->2

7 ->3

7 ->4
```

---

## Step 2

Pop

```
4
```

Visit node

```
1
```

Total

```
4
```

Now insert

```
1→2 =9

1→3 =3

1→4 =7
```

Heap

```
3->3

7->3

7->4

9->2

13->2
```

---

## Step 3

Pop

```
3->3
```

Visit node 3.

Total

```
7
```

Insert

```
3→2 =10

3→4 =4
```

Heap

```
4->4

7->3

7->4

9->2

10->2

13->2
```

---

## Step 4

Pop

```
4->4
```

Visit node 4.

Total

```
11
```

Insert

```
4→2 =14
```

---

## Step 5

Smallest remaining

```
9->2
```

Visit node 2.

Total

```
20
```

Visited all nodes.

Answer

```
20
```

---

# Thinking Process (Interview)

Instead of memorizing Prim,

train your brain to ask

### Question 1

Do I need all nodes connected?

YES

↓

Graph

---

### Question 2

Need minimum total cost?

YES

↓

Greedy

---

### Question 3

Need exactly one path?

YES

↓

Tree

---

### Question 4

Minimum Tree?

↓

Minimum Spanning Tree

---

### Question 5

How do I repeatedly choose smallest edge?

↓

Heap

---

Entire thinking chain

```
Graph

↓

Tree

↓

Minimum Tree

↓

MST

↓

Prim

↓

Heap
```

---

# Brute Force

Idea

Try every possible combination of edges.

Check

- Connected?
- Minimum?

Complexity

```
Exponential
```

Impossible.

---

# Better Brute Force

Construct every edge.

```
n² edges
```

Sort.

Apply Kruskal.

Complexity

```
Edges = n²

Sorting

O(n² log n)
```

Works.

But not optimal here.

---

# Optimized Approach (Prim)

Maintain

Visited Set

Min Heap

Current Cost

Each iteration

Take cheapest edge

Visit new node

Push new edges

Done.

---

# Time Complexity

For every node

we compute distance to all other nodes.

```
O(n²)
```

Heap operations

```
O(log n)
```

Overall

```
O(n² log n)
```

With optimization,

Prim can also run in

```
O(n²)
```

which is the standard interview solution.

---

# Python (Heap Version)

```python
from heapq import heappush, heappop

class Solution:
    def minCostConnectPoints(self, points):

        n = len(points)

        visited = set()

        heap = [(0, 0)]   # (cost, node)

        answer = 0

        while len(visited) < n:

            cost, node = heappop(heap)

            if node in visited:
                continue

            visited.add(node)
            answer += cost

            x1, y1 = points[node]

            for nxt in range(n):

                if nxt not in visited:

                    x2, y2 = points[nxt]

                    dist = abs(x1 - x2) + abs(y1 - y2)

                    heappush(heap, (dist, nxt))

        return answer
```

---

# Optimized Prim (No Heap)

Observation

We don't actually need a heap.

Keep

```
minDist[i]
```

which stores the minimum cost to connect node `i` to the current MST.

At every step:

- Pick the unvisited node with the smallest `minDist`
- Mark it visited
- Update distances for the remaining nodes

This avoids pushing duplicate edges into a heap.

### Time Complexity

```
O(n²)
```

### Space Complexity

```
O(n)
```

### Python Code

```python
class Solution:
    def minCostConnectPoints(self, points):
        n = len(points)

        minDist = [float("inf")] * n
        visited = [False] * n

        minDist[0] = 0
        answer = 0

        for _ in range(n):

            node = -1

            for i in range(n):
                if not visited[i] and (node == -1 or minDist[i] < minDist[node]):
                    node = i

            visited[node] = True
            answer += minDist[node]

            x1, y1 = points[node]

            for j in range(n):

                if not visited[j]:

                    x2, y2 = points[j]

                    dist = abs(x1 - x2) + abs(y1 - y2)

                    if dist < minDist[j]:
                        minDist[j] = dist

        return answer
```

---

# Pattern Recognition Cheat Sheet

If a problem says:

✅ Connect all cities

✅ Connect all computers

✅ Connect all islands

✅ Minimum wiring cost

✅ Build roads with minimum cost

✅ Connect every node

Immediately think

```
Minimum Spanning Tree (MST)
```

Then decide:

- Sparse graph (few edges) → **Prim (Heap)** or **Kruskal**
- Dense graph (many edges, like every point connected to every other point) → **Prim O(n²)** is often the best choice

---

# Interview Mindset

Don't memorize:

```
"Use Prim."
```

Instead build this reasoning:

```
Need to connect all nodes
        ↓
Need minimum total cost
        ↓
Need a tree (N-1 edges, no cycles)
        ↓
Minimum Spanning Tree
        ↓
Grow the tree greedily
        ↓
Repeatedly choose the cheapest edge
        ↓
Use Prim's Algorithm
```

If you can explain this chain during an interview, you're demonstrating understanding rather than memorization.

<br/><br/><br/><br/><br/>

---

# 130. Surrounded Regions

**Difficulty:** Medium

**Topics**
- Graph
- DFS
- BFS
- Matrix
- Flood Fill

---

# Problem Statement

You are given an `m × n` board containing only

```
'X'
'O'
```

A region is formed by connecting adjacent `'O'` cells.

Two cells are connected only

- Up
- Down
- Left
- Right

(not diagonally)

A region is **surrounded** if **none of its cells touches the boundary** of the board.

Your task is to convert every surrounded `'O'` into `'X'`.

The modification must be done **in-place**.

---

# Example

Input

```
X X X X
X O O X
X X O X
X O X X
```

Output

```
X X X X
X X X X
X X X X
X O X X
```

Notice

The bottom `'O'`

```
X O X X
```

touches the boundary.

Therefore it can never be surrounded.

So it remains unchanged.

---

# Understanding the Problem

The first mistake many beginners make is asking

> Which regions are surrounded?

Instead ask

> Which regions are definitely NOT surrounded?

This completely changes the way you solve the problem.

---

# Key Observation

An `'O'` can never be captured if

- It is on the boundary.
- It is connected to a boundary `'O'`.

Example

```
O O X

X O X

X X X
```

The middle `'O'`

```
X O X
```

looks surrounded.

But it is connected to

```
O
```

on the boundary.

Therefore

it is also safe.

---

# The Reverse Thinking Trick

Instead of finding

```
Surrounded Regions
```

find

```
Safe Regions
```

Once safe regions are marked,

everything else becomes surrounded automatically.

This is a very common interview trick.

---

# Visual Intuition

Board

```
X X X X

X O O X

X X O X

X O X X
```

Boundary

```
X X X X

X       X

X       X

X O X X
```

There is only one boundary `'O'`

```
(3,1)
```

Start DFS from here.

Visited

```
X X X X

X O O X

X X O X

X # X X
```

(`#` means safe.)

Notice

The three middle `'O'`

```
O O
  O
```

were never visited.

Therefore

they are surrounded.

Convert them into

```
X
```

Finally

```
X X X X

X X X X

X X X X

X O X X
```

Done.

---

# Pattern Recognition

Whenever a problem says

```
Capture Region

Boundary

Connected Cells

Grid

DFS

BFS
```

Think

```
Flood Fill
```

But instead of starting from the region,

start from the boundary.

---

# How to Think (Interview Mindset)

Suppose interviewer gives

```
X X X

X O X

X X X
```

Question

Should we flip it?

Obviously

Yes.

Now suppose

```
O X X

O O X

X X X
```

Should we flip?

No.

Why?

Because

```
O
```

touches boundary.

Everything connected to it is safe.

Now ask

> Can I find all safe cells first?

YES.

That is the entire solution.

---

# Algorithm

Step 1

Visit every boundary cell.

If it is

```
'O'
```

start DFS.

---

Step 2

DFS visits every connected

```
'O'
```

Mark them as

```
Safe
```

(using either

```
visited[][]

or

'#'
```

)

---

Step 3

Traverse the board again.

Every

```
'O'
```

that wasn't visited

↓

must be surrounded.

Convert

```
O → X
```

---

Step 4

Restore

```
#
```

back to

```
O
```

(if using in-place marking)

---

# Why Does This Work?

Suppose a region

is NOT connected to boundary.

Can DFS starting from boundary ever reach it?

No.

Therefore

it remains unvisited.

Hence

it must be surrounded.

This is why the algorithm is correct.

---

# Dry Run

Input

```
X X X X

X O O X

X X O X

X O X X
```

---

## Step 1

Boundary

```
X X X X

X       X

X       X

X O X X
```

Boundary O

```
(3,1)
```

---

## Step 2

Run DFS

Visited

```
0 0 0 0

0 0 0 0

0 0 0 0

0 1 0 0
```

Only

```
(3,1)
```

is reachable.

---

## Step 3

Traverse entire board

Cell

```
(1,1)
```

Not visited

↓

Convert

```
O → X
```

Cell

```
(1,2)
```

Not visited

↓

Convert

Cell

```
(2,2)
```

Not visited

↓

Convert

Cell

```
(3,1)
```

Visited

↓

Leave unchanged.

---

Final Board

```
X X X X

X X X X

X X X X

X O X X
```

Correct.

---

# DFS Solution (Using Visited Matrix)

## Intuition

We maintain a

```
visited
```

matrix.

Every boundary-connected `'O'`

is marked

```
visited = True
```

Later

only unvisited `'O'`

are converted into `'X'`.

---

## Python Code

```python
class Solution:
    def solve(self, board):

        if not board:
            return

        rows = len(board)
        cols = len(board[0])

        visited = [[False] * cols for _ in range(rows)]

        directions = [
            (-1,0),
            (1,0),
            (0,-1),
            (0,1)
        ]

        def dfs(r, c):

            visited[r][c] = True

            for dr, dc in directions:

                nr = r + dr
                nc = c + dc

                if (
                    0 <= nr < rows and
                    0 <= nc < cols and
                    board[nr][nc] == "O" and
                    not visited[nr][nc]
                ):
                    dfs(nr, nc)

        for i in range(rows):

            if board[i][0] == "O":
                dfs(i, 0)

            if board[i][cols-1] == "O":
                dfs(i, cols-1)

        for j in range(cols):

            if board[0][j] == "O":
                dfs(0, j)

            if board[rows-1][j] == "O":
                dfs(rows-1, j)

        for i in range(rows):
            for j in range(cols):

                if board[i][j] == "O" and not visited[i][j]:
                    board[i][j] = "X"
```

---

# Optimized Solution (Interview Preferred)

Instead of using

```
visited
```

use the board itself.

Whenever DFS visits a safe `'O'`

change it into

```
#
```

After DFS

Board

```
X X X X

X O O X

X X O X

X # X X
```

Now

convert

```
O → X
```

Then

```
# → O
```

Done.

No extra matrix needed.

---

# Optimized Code

```python
class Solution:
    def solve(self, board):

        if not board:
            return

        rows = len(board)
        cols = len(board[0])

        directions = [
            (-1,0),
            (1,0),
            (0,-1),
            (0,1)
        ]

        def dfs(r, c):

            board[r][c] = "#"

            for dr, dc in directions:

                nr = r + dr
                nc = c + dc

                if (
                    0 <= nr < rows and
                    0 <= nc < cols and
                    board[nr][nc] == "O"
                ):
                    dfs(nr, nc)

        for i in range(rows):

            if board[i][0] == "O":
                dfs(i, 0)

            if board[i][cols-1] == "O":
                dfs(i, cols-1)

        for j in range(cols):

            if board[0][j] == "O":
                dfs(0, j)

            if board[rows-1][j] == "O":
                dfs(rows-1, j)

        for i in range(rows):
            for j in range(cols):

                if board[i][j] == "O":
                    board[i][j] = "X"

                elif board[i][j] == "#":
                    board[i][j] = "O"
```

---

# Complexity Analysis

### DFS

```
Every cell is visited at most once.
```

Time

```
O(m × n)
```

Space

Visited Matrix Version

```
O(m × n)
```

Optimized Version

```
O(1)
```

Extra space

(ignoring recursion stack)

Recursion Stack

Worst Case

```
O(m × n)
```

---

# Common Mistakes

❌ Searching every `'O'` separately.

❌ Trying to determine if each region is surrounded individually.

❌ Forgetting that boundary-connected `'O'` cells are always safe.

❌ Forgetting the visited check, causing infinite recursion.

❌ Wrong visited matrix dimensions (`rows × cols`).

❌ Forgetting to restore temporary markers (`#` → `O`) in the optimized solution.

---

# Pattern Recognition

When you see

- Grid
- Connected Components
- Boundary
- Capture Regions
- Islands
- Flood Fill

Think

```
DFS / BFS
```

Then ask

> Can I solve it by starting from the boundary instead of from every region?

If the answer is yes,

that is usually the optimal solution.

---

# Interview Thinking Process

```
Need to capture surrounded regions
                │
                ▼
What regions are definitely safe?
                │
                ▼
Boundary 'O' cells
                │
                ▼
DFS/BFS from every boundary 'O'
                │
                ▼
Mark all reachable 'O' cells as safe
                │
                ▼
Any remaining 'O' must be surrounded
                │
                ▼
Convert remaining 'O' → 'X'
                │
                ▼
Restore temporary marks (if using in-place marking)
```

---

# Key Takeaways

✅ Think **reverse**: find safe regions instead of surrounded regions.

✅ Boundary-connected `'O'` cells can never be captured.

✅ DFS/BFS from the boundary naturally marks all safe cells.

✅ Remaining `'O'` cells are guaranteed to be surrounded.

✅ This is a classic **Flood Fill / Boundary DFS** interview pattern.

<br/><br/><br/><br/><br/>

---

# 1020. Number of Enclaves

**Difficulty:** Medium

**Topics**
- Graph
- DFS
- BFS
- Matrix
- Flood Fill
- Connected Components

---

# Problem Statement

You are given an `m × n` grid.

```
0 → Sea
1 → Land
```

You can move

- Up
- Down
- Left
- Right

only through land (`1`).

Your task is to count **how many land cells can NEVER reach the boundary**.

These land cells are called **Enclaves**.

---

# Example

Input

```
0 0 0 0
1 0 1 0
0 1 1 0
0 0 0 0
```

Output

```
3
```

Why?

The left land

```
1
```

touches the boundary.

So it can escape.

The remaining three land cells

```
1 1
  1
```

are trapped.

They cannot reach any boundary.

Answer

```
3
```

---

# First Intuition

When beginners read this problem, they usually think

> For every land cell,
>
> Can it reach the boundary?

That certainly works.

Suppose there are

```
1000
```

land cells.

You start DFS from every land.

That becomes

```
O((m×n)²)
```

Very slow.

So let's think differently.

---

# Reverse Thinking

Instead of asking

> Which land is trapped?

Ask

> Which land is definitely NOT trapped?

This is the key idea.

---

# Important Observation

A land cell is **NOT** an enclave if

- it is on the boundary
- OR it can reach a boundary land

Example

```
1 1 0

0 1 0

0 0 0
```

The middle land

```
1
```

looks trapped.

But

```
1
↑
1
```

It can reach the boundary.

Therefore

it is NOT an enclave.

---

# Reverse Strategy

Instead of finding trapped land,

remove all land that can escape.

Whatever remains

↓

must be trapped.

This is the easiest way to solve the problem.

---

# Visual Intuition

Grid

```
0 0 0 0

1 0 1 0

0 1 1 0

0 0 0 0
```

Boundary

```
0 0 0 0

1       0

0       0

0 0 0 0
```

Start DFS from

```
(1,0)
```

Mark it visited

```
0 0 0 0

-1 0 1 0

0 1 1 0

0 0 0 0
```

Nothing else is connected.

Now

count remaining

```
1
```

There are

```
3
```

Answer

```
3
```

---

# Pattern Recognition

Whenever a problem says

- Boundary
- Connected Cells
- Grid
- Escape
- Reach Boundary

Think

```
Boundary DFS
```

This is exactly the same pattern as

- Surrounded Regions
- Pacific Atlantic Water Flow
- Number of Islands (reverse thinking)

---

# How To Think (Interview)

Suppose interviewer gives

```
0 0 0

0 1 0

0 0 0
```

Question

Can this land escape?

No.

Now suppose

```
1 1 0

0 1 0

0 0 0
```

Can middle land escape?

Yes.

Because

```
Middle

↓

Left

↓

Boundary
```

Instead of checking every land,

ask

> Can I remove every land that escapes?

YES.

Everything left

must be trapped.

---

# Mathematical Logic

Let's divide all land into two groups.

Group A

```
Can reach boundary
```

Group B

```
Cannot reach boundary
```

DFS from every boundary land

will visit

ONLY

Group A.

Group B

can never be reached.

Therefore

Remaining land

=

Enclave.

This is why the algorithm is correct.

---

# Algorithm

Step 1

Visit every boundary cell.

If it is

```
1
```

Run DFS.

---

Step 2

DFS visits every connected land.

Mark them

```
-1
```

meaning

```
Safe
```

---

Step 3

Traverse entire grid.

Count remaining

```
1
```

Those are enclaves.

---

# Dry Run

Input

```
0 0 0 0

1 0 1 0

0 1 1 0

0 0 0 0
```

---

## Step 1

Boundary land

```
(1,0)
```

---

## Step 2

DFS

```
0 0 0 0

-1 0 1 0

0 1 1 0

0 0 0 0
```

No neighbours.

Stop.

---

## Step 3

Traverse grid

Remaining

```
1

1

1
```

Count

```
3
```

Answer

```
3
```

---

# Dry Run (Example 2)

Input

```
0 1 1 0

0 0 1 0

0 0 1 0

0 0 0 0
```

Boundary land

```
(0,1)

(0,2)
```

Run DFS

```
0 -1 -1 0

0  0 -1 0

0  0 -1 0

0  0  0 0
```

Remaining land

```
None
```

Answer

```
0
```

---

# Your Code

Your overall approach is **correct**.

You correctly identified the reverse DFS strategy.

However, there is **one bug**.

You wrote

```python
0 < nj < c
```

This is incorrect.

It should be

```python
0 <= nj < c
```

Otherwise,

column

```
0
```

is never visited.

Suppose

```
1 1
```

From

```
(0,1)
```

DFS can never move back to

```
(0,0)
```

because

```
nj = 0
```

fails the condition.

This causes incorrect answers.

---

# Correct Python Solution

```python
class Solution:
    def numEnclaves(self, grid):

        rows = len(grid)
        cols = len(grid[0])

        directions = [
            (-1,0),
            (1,0),
            (0,-1),
            (0,1)
        ]

        def dfs(r, c):

            grid[r][c] = -1

            for dr, dc in directions:

                nr = r + dr
                nc = c + dc

                if (
                    0 <= nr < rows and
                    0 <= nc < cols and
                    grid[nr][nc] == 1
                ):
                    dfs(nr, nc)

        for i in range(rows):

            if grid[i][0] == 1:
                dfs(i, 0)

            if grid[i][cols-1] == 1:
                dfs(i, cols-1)

        for j in range(cols):

            if grid[0][j] == 1:
                dfs(0, j)

            if grid[rows-1][j] == 1:
                dfs(rows-1, j)

        answer = 0

        for i in range(rows):
            for j in range(cols):

                if grid[i][j] == 1:
                    answer += 1

        return answer
```

---

# Complexity Analysis

### Time

Every cell is visited at most once.

```
O(m × n)
```

---

### Space

Recursion stack

Worst case

```
O(m × n)
```

No extra visited matrix is used.

---

# Similar Problems

This exact thinking appears in many interview questions.

| Problem | Idea |
|----------|------|
| Number of Islands | DFS on connected components |
| Surrounded Regions | Boundary DFS |
| Number of Enclaves | Boundary DFS |
| Pacific Atlantic Water Flow | Reverse DFS from boundaries |
| Flood Fill | DFS/BFS |
| Max Area of Island | DFS |

---

# Pattern Recognition

Whenever you see

- Grid
- Connected Cells
- Boundary
- Escape
- Reach Edge

Think

```
Boundary DFS
```

Then ask

> Instead of checking every cell,
>
> can I start from the boundary?

If yes,

that is usually the optimal solution.

---

# Interview Thinking Process

```
Need trapped land
        │
        ▼
What land is definitely NOT trapped?
        │
        ▼
Boundary land
        │
        ▼
DFS from every boundary land
        │
        ▼
Mark every reachable land
        │
        ▼
Remaining land cannot reach boundary
        │
        ▼
Count remaining land
```

---

# Key Takeaways

✅ Don't search from every land cell.

✅ Think in reverse: remove all land that can escape.

✅ Boundary-connected land is always safe.

✅ DFS/BFS from the boundary naturally marks all safe land.

✅ Remaining land cells are exactly the enclaves.

This **reverse-thinking + boundary DFS** pattern is one of the most common graph techniques in coding interviews and appears in multiple LeetCode problems.

<br/><br/><br/><br/><br/>

---

# 3249. Count the Number of Good Nodes

**Difficulty:** Medium

**Topics**
- Tree
- DFS
- Postorder Traversal
- Recursion
- Subtree Size

---

# Problem Statement

You are given an **undirected tree** with `n` nodes numbered from `0` to `n-1`.

The tree is rooted at node `0`.

A node is called **Good** if

> **Every child subtree of that node has exactly the same size.**

Return the total number of good nodes.

---

# Understanding the Problem

The statement

> "All child subtrees have the same size"

confuses many people.

Let's first understand

## What is a subtree?

Suppose we have

```
        0
      /   \
     1     2
    / \
   3   4
```

The subtree rooted at

```
1
```

contains

```
    1
   / \
  3   4
```

Size = **3**

The subtree rooted at

```
2
```

contains

```
2
```

Size = **1**

---

# What Makes a Node Good?

Take this tree

```
        0
      /   \
     1     2
    / \   / \
   3  4  5  6
```

Subtree sizes

```
Node 1 = 3

Node 2 = 3
```

Node

```
0
```

has children

```
1
2
```

Both subtree sizes are

```
3
```

Therefore

```
Node 0 is GOOD.
```

---

Now consider

```
        0
      /   \
     1     2
    /
   3
```

Subtree sizes

```
Node 1 = 2

Node 2 = 1
```

Different.

Therefore

```
Node 0 is NOT GOOD.
```

---

# Important Observation

Leaves have

```
No children
```

Question

Do they satisfy

> "All child subtrees have equal size"?

Yes.

Because there are

```
No child subtrees.
```

Nothing violates the condition.

Therefore

Every leaf is automatically GOOD.

---

# First Thought (Brute Force)

Suppose we check every node.

For every node

calculate every child subtree size.

Problem

Subtree sizes are recalculated again and again.

Example

```
0

↓

1

↓

2

↓

3
```

Subtree of

```
3
```

gets computed

many times.

Time

```
O(N²)
```

Too slow.

---

# Better Thinking

Ask yourself

> What information does a parent need?

Parent only needs

```
Child subtree sizes.
```

Nothing else.

Therefore

each child should compute

its subtree size

and return it

to the parent.

This immediately suggests

```
Postorder DFS
```

(children first, parent later)

---

# Why Postorder DFS?

Suppose

```
        0
      /   \
     1     2
```

Can node

```
0
```

know whether its child subtree sizes are equal

before

visiting

```
1

2
```

No.

Therefore

Parent

must wait.

Children compute first.

Exactly

```
Postorder
```

---

# Core Idea

Each DFS call should return

```
Subtree Size
```

Example

```
dfs(node)

returns

size of subtree rooted at node
```

Parent collects

```
3

3

3
```

All equal?

Yes.

Good node.

---

# Building the Algorithm

## Step 1

Convert

```
edges
```

into

```
Adjacency List
```

Example

```
0-1

0-2

1-3

1-4
```

becomes

```
0 → [1,2]

1 → [0,3,4]

2 → [0]

3 → [1]

4 → [1]
```

---

## Step 2

Run DFS from

```
0
```

---

## Step 3

Every node computes

its subtree size.

Initially

```
size = 1
```

(count itself)

---

## Step 4

Visit every child.

Child returns

```
childSize
```

Store it.

Add to

```
size
```

---

## Step 5

After processing children

Suppose child sizes are

```
3

3

3
```

All equal?

Good.

Suppose

```
3

4

3
```

Not good.

---

## Step 6

Return

```
size
```

to parent.

---

# Visual Dry Run

Example

```
        0
      /   \
     1     2
    / \   / \
   3  4  5  6
```

---

## Leaf 3

Subtree

```
3
```

Size

```
1
```

Return

```
1
```

---

Leaf 4

Return

```
1
```

---

Node 1

Receives

```
1

1
```

Equal

Good

Subtree

```
1+1+1=3
```

Return

```
3
```

---

Node 2

Same

Returns

```
3
```

---

Node 0

Receives

```
3

3
```

Equal

Good

Subtree

```
7
```

Done.

---

# Dry Run (Example 2)

Tree

```
0
├──1
│  ├──2
│  │  ├──3
│  │  │  ├──4
│  │  │  └──8
│  │  └──7
│  └──6
└──5
```

Start from leaves

```
4

6

7

8

5
```

Each returns

```
1
```

Now

Node

```
3
```

Children

```
4

8
```

Sizes

```
1

1
```

Good

Returns

```
3
```

Continue upward.

Eventually

Node

```
1
```

gets

```
5

1
```

Different.

Not Good.

Continue.

---

# Why This Works

Every subtree size

is computed

exactly once.

Parent uses already-computed answers.

No repeated work.

---

# Mathematical Logic

Suppose

```
size(node)

=

1

+

Σ size(children)
```

This is the recurrence.

For every node

```
Subtree Size

=

Itself

+

All child subtree sizes
```

While computing this

we simply check

```
All child sizes equal?
```

If yes

Answer++

---

# Correct Python Solution

```python
from collections import defaultdict

class Solution:
    def countGoodNodes(self, edges):

        n = len(edges) + 1

        graph = defaultdict(list)

        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)

        answer = 0

        def dfs(node, parent):

            nonlocal answer

            subtree = 1

            child_sizes = []

            for nei in graph[node]:

                if nei == parent:
                    continue

                size = dfs(nei, node)

                child_sizes.append(size)

                subtree += size

            if (
                len(child_sizes) <= 1 or
                len(set(child_sizes)) == 1
            ):
                answer += 1

            return subtree

        dfs(0, -1)

        return answer
```

---

# Complexity Analysis

## Building Graph

```
O(N)
```

---

## DFS

Every node visited once.

```
O(N)
```

---

## Space

Adjacency List

```
O(N)
```

Recursion Stack

Worst

```
O(N)
```

---

# Why We Pass Parent?

The tree is

**undirected**

Example

```
0 ---- 1
```

Adjacency

```
0 → 1

1 → 0
```

Without

```
parent
```

DFS

```
0

↓

1

↓

0

↓

1

↓

...
```

Infinite recursion.

Passing

```
parent
```

prevents going back.

---

# Pattern Recognition

Whenever a problem asks

- Subtree Size
- Number of Descendants
- Height of Subtree
- Diameter
- Tree DP
- Information needed by parent

Think

```
Postorder DFS
```

because

children must compute first.

---

# Interview Thinking Process

```
Need child subtree sizes
        │
        ▼
Children must finish first
        │
        ▼
Postorder DFS
        │
        ▼
Each child returns subtree size
        │
        ▼
Parent compares all child sizes
        │
        ▼
If equal → Good Node
        │
        ▼
Return subtree size to parent
```

---

# Common Mistakes

❌ Forgetting the graph is **undirected**.

❌ Not passing the parent, causing infinite recursion.

❌ Trying to recompute subtree sizes repeatedly (`O(N²)`).

❌ Forgetting that leaf nodes are automatically good.

❌ Checking node values instead of subtree sizes.

---

# Similar Problems

| Problem | Pattern |
|---------|---------|
| Subtree of Another Tree | Postorder DFS |
| Count Complete Tree Nodes | Subtree information |
| Diameter of Binary Tree | Postorder DFS |
| Binary Tree Maximum Path Sum | Postorder DFS |
| Sum of Distances in Tree | Tree DP |
| Count Good Nodes | Postorder + Subtree Size |

---

# Key Takeaways

✅ The parent only needs one thing from each child: **its subtree size**.

✅ Since parents depend on children, use **Postorder DFS**.

✅ Each DFS call returns the size of its subtree.

✅ A node is **good** if all returned child subtree sizes are identical.

✅ Computing subtree sizes once gives an **O(N)** solution.

---

# Mental Model

Whenever you solve a tree problem, ask yourself:

```
Does the parent need information from its children?
```

If **YES**, think:

```
Postorder DFS
```

Then ask:

```
What should each DFS call return?
```

In this problem, the answer is:

```
Return the size of the subtree rooted at the current node.
```

Once you identify what each recursive call should return, the implementation becomes straightforward.

<br/><br/><br/><br/><br/>

---

# 200. Number of Islands

**Difficulty:** Medium

**Topics**
- Graph
- DFS
- BFS
- Matrix
- Flood Fill
- Connected Components

---

# Problem Statement

You are given an `m × n` grid.

Each cell contains

```
'1' → Land
'0' → Water
```

An **Island** is formed by connecting adjacent land cells.

You can move only

- Up
- Down
- Left
- Right

(Diagonal cells are **NOT** connected.)

Return the **number of islands** in the grid.

---

# Example 1

Input

```
1 1 1 1 0
1 1 0 1 0
1 1 0 0 0
0 0 0 0 0
```

Output

```
1
```

Explanation

All land cells are connected together.

So there is only **one island**.

---

# Example 2

Input

```
1 1 0 0 0

1 1 0 0 0

0 0 1 0 0

0 0 0 1 1
```

Output

```
3
```

Explanation

Island 1

```
1 1
1 1
```

Island 2

```
1
```

Island 3

```
1 1
```

Total Islands

```
3
```

---

# Understanding the Problem

Imagine a real map.

```
🌊 🌊 🌊 🌊

🏝️ 🏝️ 🌊 🌊

🏝️ 🏝️ 🌊 🏝️

🌊 🌊 🌊 🏝️
```

Every connected piece of land is one island.

The question is simply

> "How many separate groups of land exist?"

---

# First Observation

Suppose you stand on one land cell.

Can you visit every land belonging to that island?

Yes.

Using

- DFS
- BFS

Once you visit them,

you never need to visit them again.

This is the entire solution.

---

# Thinking Like an Interviewer

Most beginners think

```
Count every 1.
```

Wrong.

Because

```
1 1

1 1
```

contains

```
4 land cells
```

but

only

```
1 island
```

So we don't count cells.

We count

```
Connected Components
```

---

# What is a Connected Component?

Suppose

```
1 1

1 0
```

Every land cell is reachable from every other land cell.

This entire group

is one

```
Connected Component
```

Another example

```
1 0 1
```

Now there are

```
2 Connected Components
```

Therefore

```
2 Islands
```

---

# The Main Idea

Whenever we discover a new land cell

```
1
```

that hasn't been visited,

we have found

```
A New Island.
```

Increase answer by

```
1
```

Then

visit the entire island

using DFS/BFS.

Mark everything visited.

Continue.

---

# Visual Intuition

Grid

```
1 1 0

1 0 0

0 0 1
```

Start scanning.

First land

```
1
```

Answer

```
1
```

Run DFS

Visited

```
✓ ✓ 0

✓ 0 0

0 0 1
```

Continue scanning.

Next unvisited land

```
1
```

Answer

```
2
```

Done.

---

# Why DFS Works

Suppose

```
1 1 1

1 1 0

0 0 0
```

Start DFS

```
↓

1
```

Visit

```
↓

1
```

Visit

```
↓

1
```

Eventually

every connected land cell

gets visited.

No land from this island remains unvisited.

Therefore

the next land we find

must belong to a different island.

---

# Algorithm

## Step 1

Traverse every cell.

---

## Step 2

If cell is

```
1
```

A new island is found.

Increase answer.

---

## Step 3

Run DFS.

Visit every connected

```
1
```

Mark them

```
visited
```

or

change them into

```
0
```

---

## Step 4

Continue scanning.

---

## Step 5

Return answer.

---

# Dry Run

Input

```
1 1 0 0 0

1 1 0 0 0

0 0 1 0 0

0 0 0 1 1
```

Initially

```
Answer = 0
```

---

## Scan Row 0

First cell

```
1
```

New island.

Answer

```
1
```

Run DFS

Grid becomes

```
0 0 0 0 0

0 0 0 0 0

0 0 1 0 0

0 0 0 1 1
```

---

Continue scanning.

Next land

```
(2,2)
```

New island.

Answer

```
2
```

Run DFS

Grid

```
0 0 0 0 0

0 0 0 0 0

0 0 0 0 0

0 0 0 1 1
```

---

Continue scanning.

Next land

```
(3,3)
```

Answer

```
3
```

DFS visits

```
(3,3)

↓

(3,4)
```

Grid

```
0 0 0 0 0

0 0 0 0 0

0 0 0 0 0

0 0 0 0 0
```

Finished.

Return

```
3
```

---

# Mathematical Logic

Suppose there are

```
k
```

connected groups.

Every DFS

marks exactly

```
One Connected Component.
```

Therefore

```
Number of DFS Calls

=

Number of Islands
```

This is the key mathematical idea.

---

# Why We Mark Visited

Suppose

```
1 1

1 1
```

Without marking visited

every land cell starts a new DFS.

Answer becomes

```
4
```

Wrong.

After marking

Only first land starts DFS.

Remaining lands

are already visited.

Answer

```
1
```

Correct.

---

# DFS Implementation

Instead of maintaining a visited matrix,

we can directly change

```
1 → 0
```

because

once land is visited,

we never need it again.

This saves memory.

---

# Python Solution (DFS)

```python
class Solution:

    def numIslands(self, grid):

        rows = len(grid)
        cols = len(grid[0])

        directions = [
            (-1,0),
            (1,0),
            (0,-1),
            (0,1)
        ]

        def dfs(r, c):

            grid[r][c] = "0"

            for dr, dc in directions:

                nr = r + dr
                nc = c + dc

                if (
                    0 <= nr < rows and
                    0 <= nc < cols and
                    grid[nr][nc] == "1"
                ):

                    dfs(nr, nc)

        islands = 0

        for i in range(rows):
            for j in range(cols):

                if grid[i][j] == "1":

                    islands += 1

                    dfs(i, j)

        return islands
```

---

# BFS Solution

```python
from collections import deque

class Solution:

    def numIslands(self, grid):

        rows = len(grid)
        cols = len(grid[0])

        directions = [
            (-1,0),
            (1,0),
            (0,-1),
            (0,1)
        ]

        islands = 0

        for i in range(rows):
            for j in range(cols):

                if grid[i][j] == "1":

                    islands += 1

                    queue = deque()

                    queue.append((i, j))

                    grid[i][j] = "0"

                    while queue:

                        r, c = queue.popleft()

                        for dr, dc in directions:

                            nr = r + dr
                            nc = c + dc

                            if (
                                0 <= nr < rows and
                                0 <= nc < cols and
                                grid[nr][nc] == "1"
                            ):

                                grid[nr][nc] = "0"

                                queue.append((nr, nc))

        return islands
```

---

# Complexity Analysis

## Time

Every cell is visited only once.

```
O(m × n)
```

---

## Space

DFS recursion stack

Worst Case

```
O(m × n)
```

BFS queue

Worst Case

```
O(m × n)
```

---

# Common Mistakes

❌ Counting every land cell instead of every island.

❌ Forgetting to mark visited cells.

❌ Using diagonal movement.

❌ Starting DFS from water cells.

❌ Revisiting already explored land.

---

# Pattern Recognition

Whenever you see

- Grid
- Connected Cells
- Groups
- Islands
- Regions
- Components

Think

```
Connected Components
```

Immediately ask

> Can DFS/BFS visit the entire group?

If yes,

then

```
One DFS

=

One Group
```

---

# Similar Problems

| Problem | Pattern |
|----------|----------|
| Number of Islands | Connected Components |
| Max Area of Island | Connected Components |
| Surrounded Regions | Boundary DFS |
| Number of Enclaves | Boundary DFS |
| Flood Fill | DFS/BFS |
| Rotting Oranges | Multi-source BFS |

---

# Interview Thinking Process

```
Need number of islands
          │
          ▼
Island = Connected land cells
          │
          ▼
Need to count connected components
          │
          ▼
Scan every cell
          │
          ▼
Found unvisited land?
          │
     Yes ──────────────► New Island
          │                  │
          ▼                  ▼
Increase answer      DFS/BFS entire island
          │                  │
          └──────────► Mark visited
                           │
                           ▼
Continue scanning
```

---

# Mental Model

When solving any grid problem, ask yourself these questions:

### Question 1

What does one group represent?

```
Here

↓

One Island
```

---

### Question 2

How do I explore one group?

```
DFS

or

BFS
```

---

### Question 3

How do I avoid counting the same group again?

```
Mark visited.
```

---

### Question 4

When should I increase the answer?

```
Only when we discover

an unvisited land cell.

NOT

for every land cell.
```

---

# Key Takeaways

✅ An island is simply a **connected component of land cells**.

✅ Every DFS/BFS completely explores **one island**.

✅ Therefore,

```
Number of DFS/BFS calls

=

Number of Islands.
```

✅ This is one of the most fundamental graph patterns and forms the basis for many advanced grid problems like **Max Area of Island**, **Surrounded Regions**, and **Number of Enclaves**.

<br/><br/><br/><br/><br/>

---

# 210. Course Schedule II

**Difficulty:** Medium

**Topics**
- Graph
- Topological Sort
- BFS (Kahn's Algorithm)
- DAG (Directed Acyclic Graph)
- In-degree

---

# Before Solving, Let's Review Your Solution

Your solution is **NOT correct**.

The biggest issue is that you're treating this as a normal graph traversal (BFS), but this problem is **NOT asking you to traverse the graph**.

It is asking you to find an order that satisfies all dependencies.

This is called a **Topological Ordering**.

Let's analyze your mistakes one by one.

---

# Mistake 1: Starting BFS from only one node

You wrote

```python
q = deque([prerequisites[0][1]])
```

Suppose

```
numCourses = 4

prerequisites =

[
 [1,0],
 [3,2]
]
```

Graph

```
0 → 1

2 → 3
```

There are two independent components.

Your queue becomes

```
[0]
```

BFS visits

```
0

↓

1
```

But

```
2

↓

3
```

is never visited.

Answer becomes

```
[0,1]
```

Correct answer should contain

```
0

1

2

3
```

---

# Mistake 2: Graph Traversal ≠ Valid Course Order

Suppose

```
0 → 2

1 → 2
```

Graph

```
0

 \

  2

 /

1
```

Course

```
2
```

requires BOTH

```
0

and

1
```

Your BFS

starting from

```
0
```

produces

```
0

↓

2
```

But

```
1
```

has not been completed.

So

```
0,2
```

is invalid.

This is the biggest conceptual mistake.

---

# Mistake 3: No In-degree Tracking

Suppose

```
1 depends on 0

2 depends on 0

3 depends on 1

3 depends on 2
```

Graph

```
      0
     / \
    1   2
     \ /
      3
```

Can we take

```
3
```

immediately after

```
1
```

No.

Because

```
2
```

is still unfinished.

Your algorithm doesn't check this.

Topological Sort does.

---

# Mistake 4

You wrote

```python
if len(ans)==len(graph):
```

But

```
graph
```

contains only nodes that appear as prerequisites.

Suppose

```
numCourses = 4

edges

0→1
```

Graph contains

```
0
```

only.

Length

```
1
```

But there are actually

```
4
```

courses.

Always compare against

```
numCourses
```

---

# Mistake 5

You repeatedly do

```python
if node not in ans
```

Checking

```
in list
```

takes

```
O(n)
```

Making your BFS slower.

---

# Your Thinking

Your thought process was

```
Prerequisite

↓

Graph

↓

Run BFS

↓

Answer
```

This would work for

- Graph traversal
- Reachability
- Shortest path

But

Course Schedule

is different.

It asks

```
Can I legally take this course now?
```

That depends on

```
All prerequisites finished.
```

Not simply

```
Have I visited it?
```

---

# Understanding the Problem

Suppose you have

```
Math

↓

Physics

↓

Quantum Physics
```

Can you directly study

```
Quantum Physics
```

No.

You must finish

```
Math

↓

Physics
```

first.

Now imagine hundreds of courses.

Question

In what order can I study them?

---

# Convert Into Graph

Given

```
[1,0]
```

Meaning

```
0

↓

1
```

Because

```
0

must be completed

before

1
```

Every prerequisite

becomes

```
Directed Edge
```

---

# Example

```
numCourses = 4

[1,0]

[2,0]

[3,1]

[3,2]
```

Graph

```
      0
     / \
    1   2
     \ /
      3
```

---

# What Is Actually Being Asked?

Find an order

such that

every edge

goes

```
Earlier

↓

Later
```

Example

```
0

↓

1

↓

3
```

Valid

```
0

1

3
```

Invalid

```
3

1

0
```

---

# Important Observation

A course can be taken only if

it has

```
NO remaining prerequisites.
```

Question

How do we know that?

Count

```
Incoming Edges.
```

---

# What Is In-degree?

In-degree means

```
How many prerequisites are still needed?
```

Example

```
0

↓

2

1

↓

2
```

Node

```
2
```

has

```
In-degree =2
```

because

two courses

must be completed.

---

# Core Idea

Initially

take every course

whose

```
In-degree =0
```

Why?

Because

they require

nothing.

After completing one course

remove its outgoing edges.

Some other courses

may now have

```
In-degree =0
```

Take them next.

Repeat.

This algorithm is called

```
Kahn's Algorithm
```

---

# Why Does This Work?

Suppose

```
0 → 1 → 2
```

Initially

```
Indegree

0 :0

1 :1

2 :1
```

Take

```
0
```

Remove edge

```
0→1
```

Now

```
1

has indegree

0
```

Take

```
1
```

Remove

```
1→2
```

Now

```
2

becomes

0
```

Everything naturally unlocks.

---

# Building the Algorithm

## Step 1

Build graph.

```
Prerequisite

↓

Course
```

---

## Step 2

Compute

```
Indegree
```

for every node.

---

## Step 3

Put every node with

```
Indegree=0
```

inside queue.

---

## Step 4

While queue not empty

Take one course.

Append answer.

---

## Step 5

Remove outgoing edges.

Decrease indegree.

Whenever

```
Indegree becomes 0
```

Push into queue.

---

## Step 6

Finished.

If answer contains

```
numCourses
```

Return answer.

Else

Cycle exists.

Return

```
[]
```

---

# Dry Run

Example

```
numCourses=4

[1,0]

[2,0]

[3,1]

[3,2]
```

Graph

```
      0
     / \
    1   2
     \ /
      3
```

---

## Build In-degree

```
0 :0

1 :1

2 :1

3 :2
```

Queue

```
[0]
```

---

## Pop

Take

```
0
```

Answer

```
[0]
```

Remove

```
0→1

0→2
```

New indegree

```
0

0

0

2
```

Queue

```
1

2
```

---

## Pop

Take

```
1
```

Answer

```
0

1
```

Remove

```
1→3
```

Indegree

```
3

↓

1
```

Still not zero.

---

## Pop

Take

```
2
```

Answer

```
0

1

2
```

Remove

```
2→3
```

Indegree

```
0
```

Queue

```
3
```

---

## Pop

Take

```
3
```

Answer

```
0

1

2

3
```

Finished.

---

# What About Cycles?

Suppose

```
0→1

1→0
```

Graph

```
0

↓

1

↑

|
```

Indegree

```
1

1
```

Queue

```
Empty
```

Nothing can start.

Answer

```
[]
```

Correct.

---

# Python Solution (Kahn's Algorithm)

```python
from collections import defaultdict, deque

class Solution:

    def findOrder(self, numCourses, prerequisites):

        graph = defaultdict(list)

        indegree = [0] * numCourses

        for course, pre in prerequisites:

            graph[pre].append(course)

            indegree[course] += 1

        queue = deque()

        for i in range(numCourses):

            if indegree[i] == 0:
                queue.append(i)

        answer = []

        while queue:

            node = queue.popleft()

            answer.append(node)

            for neighbor in graph[node]:

                indegree[neighbor] -= 1

                if indegree[neighbor] == 0:

                    queue.append(neighbor)

        if len(answer) == numCourses:

            return answer

        return []
```

---

# Complexity Analysis

Building Graph

```
O(V+E)
```

BFS

```
O(V+E)
```

Total

```
O(V+E)
```

Space

```
O(V+E)
```

---

# Pattern Recognition

Whenever you see

- Course Prerequisites
- Build Order
- Dependency
- Task Scheduling
- Install Packages
- Compilation Order

Think

```
Topological Sort
```

---

# Interview Thinking Process

```
Need valid order
        │
        ▼
Dependencies exist
        │
        ▼
Directed Graph
        │
        ▼
Need every prerequisite before course
        │
        ▼
Topological Sort
        │
        ▼
Use In-degree
        │
        ▼
Start with nodes having In-degree = 0
        │
        ▼
Remove edges
        │
        ▼
New nodes become available
        │
        ▼
Continue until all courses processed
```

---

# Common Mistakes

❌ Treating it as a normal BFS/DFS traversal.

❌ Starting from a single node.

❌ Ignoring disconnected components.

❌ Not tracking in-degrees.

❌ Forgetting cycle detection.

❌ Comparing answer length with the graph size instead of `numCourses`.

---

# Similar Problems

| Problem | Pattern |
|----------|---------|
| Course Schedule I | Cycle Detection |
| Course Schedule II | Topological Sort |
| Alien Dictionary | Topological Sort |
| Parallel Courses | Topological Sort |
| Sequence Reconstruction | Topological Sort |
| Build System / Package Manager | Topological Sort |

---

# Key Takeaways

✅ This is **not** a graph traversal problem—it is a **dependency ordering** problem.

✅ A course is available **only when all its prerequisites are complete**.

✅ The number of remaining prerequisites is the **in-degree**.

✅ Start with all courses having **in-degree = 0**.

✅ Repeatedly remove completed courses and unlock new ones.

✅ If every course is processed, you found a valid order. Otherwise, a cycle exists, so no valid ordering is possible.

<br/><br/><br/><br/><br/>

---
