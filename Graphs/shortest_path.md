# Introduction

This is a beginner-friendly guide to shortest path problems in graphs. It explains how to solve shortest paths in a DAG using topological ordering and relaxation, why BFS works for unweighted graphs, and when Dijkstra is the right choice for weighted graphs with positive edge weights.

## Concepts and Theories
- [Shortest Path in Directed Acyclic Graph](#shortest-path-in-directed-acyclic-graph-dag)
- [Shortest Path in Undirected Graph with Unit Weights](#shortest-path-in-undirected-graph-with-unit-weights)
- [Dijkstra's Algorithm (Priority Queue)](#dijkstras-algorithm-priority-queue)
- [Dijkstra's Algorithm Using (Set)](#dijkstras-algorithm-set)

## Problems
- [1091. Shortest Path in Binary Matrix](#1091-shortest-path-in-binary-matrix)
- [1631. Path With Minimum Effort](#1631-path-with-minimum-effort)
- [787. Cheapest Flights Within K Stops](#787-cheapest-flights-within-k-stops)

<br/><br/><br/>

# Theories and Concept

# Shortest Path in Directed Acyclic Graph (DAG)

## Problem Statement

You are given a **Directed Acyclic Graph (DAG)** with weighted edges.

Your task is to find the **shortest distance from the source node (0)** to every other node.

If a node cannot be reached from the source, return **-1** for that node.

---

# What is a DAG?

A DAG is a graph that has two properties:

1. **Directed**
   - Every edge has a direction.
   - Example:

```
0 ----> 1 ----> 2
```

You can move

```
0 → 1 → 2
```

but not

```
2 → 1
```

unless another edge exists.

---

2. **Acyclic**

There are **no cycles**.

Example:

```
0 → 1 → 2
```

Valid DAG

But

```
0 → 1 → 2
↑         |
|_________|
```

is **not** a DAG because it contains a cycle.

---

# Example

```
0 ----2----> 1
 \
  \1
   \
    > 4

4 ----2----> 2

1 ----3----> 2
```

To reach node 2:

Path 1

```
0 → 1 → 2

Cost

2 + 3 = 5
```

Path 2

```
0 → 4 → 2

Cost

1 + 2 = 3
```

Shortest distance

```
3
```

---

# Brute Force Thinking

Suppose someone asks

> Find the shortest distance from 0 to every node.

A beginner may think:

```
Run DFS from source.
```

Problem:

DFS explores one path completely.

It does **not** guarantee the shortest path.

---

Another idea:

```
Run BFS.
```

Problem:

BFS works only when every edge has equal weight.

Here edges have different weights.

Example

```
0 --100--> 1

0 --1--> 2 --1--> 1
```

BFS chooses

```
0 → 1
```

because it has fewer edges,

but the shortest weighted path is

```
0 → 2 → 1

Cost = 2
```

---

# Why Topological Sort?

The key property of a DAG is:

> There are **no cycles**.

That means there exists an order of nodes such that:

```
Every edge goes from left to right.
```

Example

```
6
↓
5
↓
4
↓
2
↓
0
↓
1
↓
3
```

This is called the **Topological Order**.

---

# Main Idea

Instead of repeatedly searching for shortest paths,

we process nodes in **topological order**.

By the time we process a node,

every possible node that can reach it has already been processed.

Therefore,

its shortest distance is already known.

---

# Algorithm

## Step 1

Create the graph.

Since edges have weights,

store

```
(neighbor, weight)
```

instead of only

```
neighbor
```

Example

```
0

↓

[(1,2),(4,1)]
```

Meaning

```
0 → 1 (weight 2)

0 → 4 (weight 1)
```

---

## Step 2

Find the **Topological Sort** using DFS.

Example

```
6 5 4 2 0 1 3
```

Store it inside a stack.

---

## Step 3

Create a distance array.

Initially

```
INF INF INF INF INF INF
```

Source

```
0
```

becomes

```
0 INF INF INF INF INF
```

---

## Step 4

Pop nodes from the topological stack.

For every outgoing edge,

perform **Relaxation**.

---

# What is Relaxation?

Suppose

```
Current Node = u

Neighbor = v

Edge Weight = w
```

Current shortest distance to

```
u

=

dist[u]
```

If we go

```
u → v
```

new distance becomes

```
dist[u] + w
```

If

```
dist[u] + w

<

dist[v]
```

update

```
dist[v]
```

This process is called **Relaxation**.

---

# Relaxation Formula

```
if dist[u] + weight < dist[v]:

    dist[v] = dist[u] + weight
```

---

# Dry Run

Suppose

```
0 --2--> 1

0 --1--> 4

4 --2--> 2

2 --6--> 3

4 --4--> 5

5 --1--> 3
```

Topological Order

```
0 4 5 1 2 3
```

---

Initially

```
dist

0 INF INF INF INF INF
```

---

Process

```
0
```

Relax

```
0→1

Distance = 2
```

Update

```
dist[1]=2
```

Relax

```
0→4

Distance=1
```

Update

```
dist[4]=1
```

Distance array

```
0 2 INF INF 1 INF
```

---

Process

```
4
```

Relax

```
4→2

1+2=3
```

Update

```
dist[2]=3
```

Relax

```
4→5

1+4=5
```

Update

```
dist[5]=5
```

Distance

```
0 2 3 INF 1 5
```

---

Process

```
5
```

Relax

```
5→3

5+1=6
```

Update

```
dist[3]=6
```

---

Process

```
1
```

Relax

```
1→2

2+3=5
```

Already

```
dist[2]=3
```

No update.

---

Process

```
2
```

Relax

```
2→3

3+6=9
```

Already

```
dist[3]=6
```

No update.

---

Final Answer

```
0 2 3 6 1 5
```

---

# Python Code

```python
from collections import defaultdict

class Solution:
    def shortestPath(self, n, m, edges):

        graph = defaultdict(list)

        # Build Graph
        for u, v, wt in edges:
            graph[u].append((v, wt))

        # -----------------------
        # Topological Sort
        # -----------------------
        visited = [False] * n
        stack = []

        def topo(node):
            visited[node] = True

            for nei, wt in graph[node]:
                if not visited[nei]:
                    topo(nei)

            stack.append(node)

        for i in range(n):
            if not visited[i]:
                topo(i)

        # -----------------------
        # Shortest Path
        # -----------------------
        INF = float("inf")

        dist = [INF] * n
        dist[0] = 0

        while stack:

            node = stack.pop()

            if dist[node] == INF:
                continue

            for nei, wt in graph[node]:

                if dist[node] + wt < dist[nei]:
                    dist[nei] = dist[node] + wt

        for i in range(n):
            if dist[i] == INF:
                dist[i] = -1

        return dist
```

---

# Time Complexity

### Building Graph

```
O(E)
```

---

### Topological Sort

```
O(V + E)
```

---

### Relaxation

Every edge is relaxed exactly once.

```
O(E)
```

---

### Overall

```
O(V + E)
```

---

# Space Complexity

Graph

```
O(V + E)
```

Visited

```
O(V)
```

Stack

```
O(V)
```

Distance

```
O(V)
```

Overall

```
O(V + E)
```

---

# Key Interview Pattern

When you see:

- Directed Graph
- **No Cycles (DAG)**
- Weighted Edges
- Shortest Path from a Source

Think immediately:

```
DAG
        ↓
Topological Sort
        ↓
Distance Array
        ↓
Relax Every Edge Once
        ↓
O(V + E)
```

The absence of cycles is what makes this approach optimal: processing nodes in topological order guarantees that when you relax a node's outgoing edges, all possible shorter paths to that node have already been finalized.

<br/><br/><br/><br/><br/>

---

# Shortest Path in Undirected Graph with Unit Weights

> Why BFS Works for Shortest Path When Every Edge Has Unit Weight

## The Key Observation

Every edge in the graph has the **same weight (cost)**.

```text
Edge Weight = 1
```

This means that moving from one node to any adjacent node always increases the total distance by exactly **1**.

For example,

```text
A ---- B ---- C ---- D

Each edge = 1
```

Distance from `A`:

```text
A = 0

B = 1

C = 2

D = 3
```

Every move simply adds **+1**.

---

# Mathematical Relation

Suppose we are currently at node `u`.

The shortest distance to `u` is

```text
dist[u]
```

If there is an edge from `u` to `v`

```text
u ------ v
      weight = 1
```

Then the distance to `v` through `u` becomes

```text
dist[v] = dist[u] + 1
```

Since every edge contributes exactly **1**, we never need to add different weights.

This is the entire mathematics behind BFS on an unweighted graph.

---

# Why Does BFS Find the Shortest Path?

Imagine dropping a stone into water.

The water spreads in circles.

```text
Source

        0

      / | \

     1  1  1

    /|  |  |\

   2 2  2 2 2

  /          \

 3            3
```

The wave reaches

- all nodes at distance **1**
- then all nodes at distance **2**
- then all nodes at distance **3**

It **never skips a smaller distance**.

BFS behaves exactly like this.

---

# What Does the Queue Store?

Initially,

```text
Queue

[source]
```

After visiting the source,

```text
Queue

All nodes at distance 1
```

After processing those,

```text
Queue

All nodes at distance 2
```

Then,

```text
Queue

All nodes at distance 3
```

Notice something important.

The queue is automatically ordered by distance.

```text
Distance 0

↓

Distance 1

↓

Distance 2

↓

Distance 3
```

There is no possibility of processing a node at distance 3 before a node at distance 2.

This is exactly why BFS works.

---

# Why Don't We Need Dijkstra?

Dijkstra's algorithm is needed when edge weights are different.

Example:

```text
A ---5--- B

A ---1--- C ---1--- B
```

The shortest path is

```text
A → C → B = 2
```

not

```text
A → B = 5
```

Since edge weights are different,

we must always choose the smallest current distance.

For that, Dijkstra uses a **Priority Queue (Min Heap)**.

---

# But Here Every Edge Is 1

Suppose

```text
0

/ \

1  2

|   |

3   4
```

From node `0`

Distance becomes

```text
1 = 1

2 = 1
```

Next,

```text
3 = 2

4 = 2
```

Notice the order in which nodes are discovered.

```text
0

↓

1,2

↓

3,4
```

The queue is already sorted.

There is no need for a Priority Queue.

A normal FIFO queue is sufficient.

---

# Why Is the First Time We Reach a Node the Shortest Distance?

Suppose we reach node `X`.

```text
Source

↓

A

↓

X
```

Distance = 2

Later,

another path reaches

```text
Source

↓

B

↓

C

↓

X
```

Distance = 3

Which one is shorter?

Obviously,

```text
2 < 3
```

Since BFS explores all nodes level by level,

the **first time we reach a node is always through the shortest path**.

Any later visit can only be

- equal distance
- or longer distance

Never shorter.

Therefore,

we never need to revisit the node.

---

# The Core Formula

Whenever we visit a neighbor,

```text
newDistance = currentDistance + 1
```

If

```text
newDistance < dist[neighbor]
```

then

```text
dist[neighbor] = newDistance
```

and push the neighbor into the queue.

Otherwise,

ignore it.

---

# Level-by-Level Example

Consider this graph.

```text
        0
      /   \
     1     3
     |     |
     2     4
      \   /
        5
```

Initially,

```text
Queue

0
```

Distance array

```text
0 : 0
1 : ∞
2 : ∞
3 : ∞
4 : ∞
5 : ∞
```

---

### Step 1

Process `0`

New nodes

```text
1

3
```

Queue

```text
1
3
```

Distance

```text
0 = 0
1 = 1
3 = 1
```

---

### Step 2

Process `1`

Discover

```text
2
```

Queue

```text
3
2
```

Distance

```text
2 = 2
```

---

### Step 3

Process `3`

Discover

```text
4
```

Queue

```text
2
4
```

Distance

```text
4 = 2
```

---

### Step 4

Process `2`

Discover

```text
5
```

Queue

```text
4
5
```

Distance

```text
5 = 3
```

Even if node `4` later also reaches `5`,

that path would also have distance `3` (or more).

So we don't need to update it.

---

# Intuition to Remember

Whenever you see:

- Find the **shortest path**
- Every edge has the **same cost**
- Every move costs exactly **1**
- Find the **minimum number of moves/steps**

Your brain should immediately think:

```text
Shortest Path
        │
        ▼
All edges have equal weight
        │
        ▼
Explore level by level
        │
        ▼
Breadth First Search (BFS)
```

---

# One-Line Interview Intuition

> **BFS finds the shortest path in an unweighted graph because every edge has the same cost (1), so exploring nodes level by level guarantees that the first time a node is reached is through the minimum possible number of edges.**

<br/><br/><br/><br/><br/>

---

# Dijkstra's Algorithm (Priority Queue)

## When Should You Think of Dijkstra?

Whenever you see:

- Find the **shortest path**
- Graph has **positive edge weights**
- Find shortest distance from **one source** to **all nodes**

➡️ Think:

```text
Weighted Graph
        │
        ▼
Positive Edge Weights
        │
        ▼
Single Source Shortest Path
        │
        ▼
Dijkstra's Algorithm
```

---

# Problem Statement

You are given

- A weighted graph
- A source node

You need to find the **minimum distance** from the source to every other node.

Example

```text
        4
    0 ------- 1
    |         |
   4|         |2
    |         |
    2 ------- 3
        3
```

Source = 0

Output

```text
Distance to 0 = 0

Distance to 1 = 4

Distance to 2 = 4

Distance to 3 = 7
```

---

# Why Can't BFS Work?

Suppose

```text
A -----10----- B

A ------1------ C ------1------ B
```

BFS says

```text
A → B
```

because it has fewer edges.

Distance

```text
10
```

But actually

```text
A → C → B

1 + 1 = 2
```

which is much smaller.

So BFS **cannot** solve weighted shortest path problems.

---

# The Main Idea

Instead of exploring level by level,

we always explore

> **the node having the smallest current distance.**

That's the whole idea of Dijkstra.

---

# Why Do We Need a Priority Queue?

Imagine these nodes are waiting.

```text
Node 1 -> Distance 15

Node 2 -> Distance 4

Node 3 -> Distance 9

Node 4 -> Distance 20
```

Which node should be processed first?

Obviously

```text
Distance = 4
```

Because it is the closest node discovered so far.

A normal queue cannot do this.

A **Min Heap (Priority Queue)** always gives us

```text
Smallest Distance First
```

---

# What Does the Priority Queue Store?

It stores

```text
(distance, node)
```

Example

```text
(0,0)

(4,2)

(7,5)

(10,3)
```

The smallest distance is always removed first.

---

# Distance Array

Initially

```text
dist =

0 : 0

1 : ∞

2 : ∞

3 : ∞

4 : ∞
```

Infinity means

> We haven't found any path yet.

---

# Core Mathematics

Suppose

```text
u ----(weight)---- v
```

We already know

```text
dist[u]
```

Then

Possible distance to

```text
v
```

is

```text
newDistance = dist[u] + weight
```

Now compare

```text
newDistance

vs

dist[v]
```

If

```text
newDistance < dist[v]
```

then

we discovered a shorter path.

Update

```text
dist[v] = newDistance
```

and push

```text
(newDistance,v)
```

into the priority queue.

This process is called **Relaxation**.

---

# Relaxation

Current graph

```text
0 ----4---- 1
```

Current

```text
dist[0]=0

dist[1]=∞
```

Possible new distance

```text
0+4=4
```

Since

```text
4<∞
```

Update

```text
dist[1]=4
```

---

# Why Does Dijkstra Work?

Suppose

Priority Queue

```text
(2,A)

(5,B)

(8,C)
```

The smallest distance is

```text
2
```

No future path can reach A with a smaller value because

all remaining edges have **positive weights**.

Positive weights can only increase distance.

Therefore,

once we remove

```text
(2,A)
```

its shortest distance is finalized.

This is the greedy idea behind Dijkstra.

---

# Step-by-Step Dry Run

Graph

```text
        4
    0 ------- 1
    |         |
   4|         |2
    |         |
    2 ------- 3
        3
```

Source

```text
0
```

---

## Initial State

Priority Queue

```text
(0,0)
```

Distance

```text
0 = 0

1 = ∞

2 = ∞

3 = ∞
```

---

## Step 1

Pop

```text
(0,0)
```

Neighbors

```text
1

2
```

Distances

```text
0+4=4

0+4=4
```

Update

Priority Queue

```text
(4,1)

(4,2)
```

Distance

```text
0

4

4

∞
```

---

## Step 2

Pop

```text
(4,1)
```

Neighbor

```text
3
```

Possible distance

```text
4+2=6
```

Update

Priority Queue

```text
(4,2)

(6,3)
```

Distance

```text
0

4

4

6
```

---

## Step 3

Pop

```text
(4,2)
```

Neighbor

```text
3
```

Possible distance

```text
4+3=7
```

Already

```text
dist[3]=6
```

Since

```text
7>6
```

Ignore.

---

## Step 4

Pop

```text
(6,3)
```

Done.

Final Answer

```text
0

4

4

6
```

---

# Python Code

```python
import heapq


def dijkstra(n, graph, source):
    """
    n      -> Number of vertices
    graph  -> Adjacency List
              graph[u] = [(v, weight), ...]
    source -> Starting node
    """

    INF = float('inf')

    # Distance array
    dist = [INF] * n
    dist[source] = 0

    # Min Heap (distance, node)
    pq = [(0, source)]

    while pq:

        current_distance, node = heapq.heappop(pq)

        # Ignore outdated entries
        if current_distance > dist[node]:
            continue

        # Visit all neighbors
        for neighbor, weight in graph[node]:

            new_distance = current_distance + weight

            # Relaxation
            if new_distance < dist[neighbor]:

                dist[neighbor] = new_distance

                heapq.heappush(
                    pq,
                    (new_distance, neighbor)
                )

    return dist
```

---

# Example

```python
graph = [
    [(1,4), (2,4)],          # 0
    [(0,4), (3,2)],          # 1
    [(0,4), (3,3)],          # 2
    [(1,2), (2,3)]           # 3
]

print(dijkstra(4, graph, 0))
```

Output

```text
[0, 4, 4, 6]
```

---

# Code Walkthrough

## Step 1

```python
dist = [INF] * n
```

Initially

```text
∞ ∞ ∞ ∞
```

---

## Step 2

```python
dist[source]=0
```

Source distance is always

```text
0
```

---

## Step 3

```python
pq=[(0,source)]
```

Start with

```text
(distance,node)
```

---

## Step 4

```python
current_distance,node=heapq.heappop(pq)
```

Always removes

```text
Smallest Distance
```

---

## Step 5

Visit neighbors

```python
for neighbor,weight in graph[node]:
```

---

## Step 6

Compute

```python
new_distance=current_distance+weight
```

This is

```text
Current Distance

+

Edge Weight
```

---

## Step 7

Relaxation

```python
if new_distance<dist[neighbor]:
```

Found a shorter path.

Update

```python
dist[neighbor]=new_distance
```

---

## Step 8

Push back

```python
heapq.heappush(
    pq,
    (new_distance,neighbor)
)
```

Because the neighbor may help us discover shorter paths.

---

# Why Do We Ignore Outdated Entries?

Suppose

Priority Queue contains

```text
(10,5)

(7,5)
```

The node

```text
5
```

already has a better distance

```text
7
```

Later,

```text
10
```

comes out.

Should we process it?

No.

Hence

```python
if current_distance>dist[node]:
    continue
```

This skips old entries.

---

# Time Complexity

Each edge is relaxed once.

Each heap operation costs

```text
O(log V)
```

Total

```text
O((V+E) log V)
```

Usually written as

```text
O(E log V)
```

---

# Space Complexity

Distance Array

```text
O(V)
```

Priority Queue

```text
O(V)
```

Adjacency List

```text
O(V+E)
```

Overall

```text
O(V+E)
```

---

# Why Doesn't Dijkstra Work for Negative Weights?

Consider

```text
0 --(-2)--> 1
^           |
|           |
+----(-2)---+
```

Start

```text
0
```

Distance

```text
0
```

Go to

```text
1
```

Distance

```text
-2
```

Come back

```text
0
```

Distance

```text
-4
```

Again

```text
1
```

Distance

```text
-6
```

Again

```text
0
```

Distance

```text
-8
```

The distance keeps decreasing forever.

```text
0

↓

-2

↓

-4

↓

-6

↓

-8

↓

...
```

There is **no finite shortest path**.

Since Dijkstra assumes that once a node with the smallest distance is removed from the priority queue, its distance is final, this assumption breaks when negative-weight edges exist. This matches the explanation in your uploaded notes, which emphasize that Dijkstra is intended for graphs with non-negative edge weights. :contentReference[oaicite:0]{index=0}

---

# Pattern Recognition

Whenever you see

- Weighted Graph
- Positive Weights
- Shortest Distance
- Source → All Nodes

Immediately think

```text
Graph
    │
    ▼
Weighted
    │
    ▼
Positive Weights
    │
    ▼
Need Minimum Distance
    │
    ▼
Priority Queue
    │
    ▼
Dijkstra's Algorithm
```

---

# One-Line Interview Intuition

> **Dijkstra's algorithm is a greedy shortest-path algorithm that always expands the node with the smallest known distance. Because all edge weights are non-negative, once a node is removed from the min-heap, no shorter path to that node can ever be found later.**

<br/><br/><br/><br/><br/>

---

# Dijkstra's Algorithm (Set)

## Why do we use a `set` in Dijkstra's Algorithm?

The main goal of Dijkstra's algorithm is to **always process the node with the smallest distance first**.

When we implemented Dijkstra using a **priority queue (min-heap)**, we stored:

```python
(distance, node)
```

The priority queue automatically gave us the smallest distance first.

A `set` can also achieve the same goal.

---

# Important Property of `set`

In C++ (`std::set`):

- Stores **unique** values.
- Stores elements in **sorted order**.
- Therefore, the **smallest element is always at the beginning**.

Example:

```cpp
set<pair<int,int>> st;

st.insert({5,4});
st.insert({2,1});
st.insert({8,3});
```

Internally:

```
(2,1)
(5,4)
(8,3)
```

The first element is always the smallest.

---

# How does Python do this?

Python's built-in `set` is **NOT sorted**.

```python
s = {(5,4), (2,1), (8,3)}

print(s)
```

Output could be

```
{(5,4), (8,3), (2,1)}
```

Notice:

- Not sorted
- Cannot get the minimum efficiently

Therefore,

**Python's built-in set CANNOT be used like C++ std::set for Dijkstra.**

Instead, Python uses

- `heapq` (recommended)
- or a third-party package like `sortedcontainers.SortedSet`

---

# What is stored inside the set?

We store

```python
(distance, node)
```

Example

```
(0,0)
```

Meaning

```
Distance = 0
Node = 0
```

Later,

```
(4,1)
(4,2)
(7,3)
(5,4)
```

The set automatically sorts them as

```
(4,1)
(4,2)
(5,4)
(7,3)
```

The first element is always the minimum distance.

---

# Why does `(4,1)` come before `(4,2)`?

Pairs are compared lexicographically.

Python and C++ compare tuples/pairs like this:

```
(distance, node)
```

First compare

```
distance
```

If equal,

compare

```
node
```

Example

```
(4,1)
(4,2)
```

Distance is equal.

Compare nodes

```
1 < 2
```

Therefore

```
(4,1)
```

comes first.

---

# Example Graph

```
        4
    0 ------- 1
    |         |
 4  |         | 2
    |         |
    2---------+
      \
       \3
        \
         3
```

Initially

```
Distance array

0 : 0
1 : INF
2 : INF
3 : INF
```

Set

```
{(0,0)}
```

---

# Step 1

Remove

```
(0,0)
```

Visit neighbors.

Node 1

```
0 + 4 = 4
```

Update

```
dist[1]=4
```

Insert

```
(4,1)
```

Node 2

```
0+4=4
```

Insert

```
(4,2)
```

Now

```
{
(4,1),
(4,2)
}
```

---

# Step 2

Take the smallest

```
(4,1)
```

Visit its neighbors.

Suppose no shorter path is found.

Nothing changes.

Set

```
{
(4,2)
}
```

---

# Step 3

Remove

```
(4,2)
```

Now suppose

```
2 -> 5
```

cost

```
6
```

Current distance

```
4
```

New distance

```
10
```

Insert

```
(10,5)
```

Set becomes

```
{
(10,5)
}
```

---

# Later...

Suppose another node reaches node 5.

```
Current path

Distance = 5
```

Edge weight

```
3
```

New distance

```
8
```

Now

```
8 < 10
```

We found a better path.

---

# Priority Queue Behavior

Priority Queue contains

```
(10,5)
```

Insert

```
(8,5)
```

Now heap contains

```
(8,5)
(10,5)
```

Eventually,

```
(8,5)
```

is processed.

Later,

```
(10,5)
```

is also removed from the heap.

But

```
10
```

is already outdated.

So one unnecessary iteration occurs.

---

# Set Behavior

Set contains

```
(10,5)
```

We discover

```
(8,5)
```

Instead of inserting directly,

we first erase

```
(10,5)
```

Then insert

```
(8,5)
```

Final set

```
{
(8,5)
}
```

The outdated entry is removed immediately.

No extra processing later.

---

# Why erase first?

Because

```
10
```

is no longer the shortest distance.

Keeping it wastes time.

The set allows us to delete the old value.

Priority queue does not.

---

# Code (C++)

```cpp
if(dist[v] != INF)
{
    st.erase({dist[v], v});
}

dist[v] = newDistance;

st.insert({dist[v], v});
```

---

# Python Equivalent (using SortedSet)

```python
from sortedcontainers import SortedSet

st = SortedSet()

dist = [float('inf')] * n
dist[source] = 0

st.add((0, source))

while st:

    d, node = st.pop(0)

    for nxt, wt in graph[node]:

        if d + wt < dist[nxt]:

            if dist[nxt] != float('inf'):
                st.discard((dist[nxt], nxt))

            dist[nxt] = d + wt

            st.add((dist[nxt], nxt))
```

---

# Time Complexity

For a balanced BST (`std::set`):

| Operation | Complexity |
|-----------|------------|
| Insert | O(log V) |
| Erase | O(log V) |
| Find Minimum | O(1) (begin()) |
| Remove Minimum | O(log V) |

Overall Dijkstra complexity:

```
O((V + E) log V)
```

---

# Priority Queue vs Set

| Feature | Priority Queue | Set |
|----------|---------------|-----|
| Smallest element first | ✅ | ✅ |
| Stores duplicates | ✅ | ❌ (same pair) |
| Remove old distance | ❌ | ✅ |
| Extra outdated entries | Yes | No |
| Insert | O(log V) | O(log V) |
| Delete | Not possible directly | O(log V) |
| Recommended in Python | ✅ (`heapq`) | Only with `SortedSet` |

---

# Key Takeaways

- Dijkstra always processes the node with the **smallest current distance**.
- C++ `std::set` keeps `(distance, node)` pairs **sorted automatically**.
- The biggest advantage of a `set` over a priority queue is that **you can erase an outdated `(distance, node)` pair** before inserting the updated one.
- This avoids processing stale entries later, although both implementations have the same asymptotic time complexity: **O((V + E) log V)**.
- In Python, the built-in `set` is **not sorted**, so the usual implementation uses `heapq`. To mimic C++ `std::set`, you need a sorted data structure such as `sortedcontainers.SortedSet`.

<br/><br/><br/><br/><br/>

---

# Problems

# 1091. Shortest Path in Binary Matrix

## Problem Statement

You are given an `n × n` binary matrix called `grid`.

- `0` → Open (can walk)
- `1` → Blocked (cannot walk)

You start from the **top-left corner** `(0, 0)` and want to reach the **bottom-right corner** `(n - 1, n - 1)`.

You can move in **8 directions**:

```text
↖   ↑   ↗
←   •   →
↙   ↓   ↘
```

Your task is to return the **length of the shortest clear path**.

If no path exists, return `-1`.

---

# Example 1

Input

```text
grid = [
 [0,1],
 [1,0]
]
```

Visualization

```text
S  X
X  E
```

Since diagonal movement is allowed,

```text
S → E
```

Cells visited:

```text
(0,0)
(1,1)
```

Answer

```text
2
```

---

# Example 2

Input

```text
grid = [
 [0,0,0],
 [1,1,0],
 [1,1,0]
]
```

Visualization

```text
S  .  .
X  X  .
X  X  E
```

Shortest path

```text
S
↓

.
↓

.
↓

E
```

Length

```text
4
```

---

# Example 3

```text
grid = [
 [1,0,0],
 [1,1,0],
 [1,1,0]
]
```

The starting cell is blocked.

Answer

```text
-1
```

---

# Step 1: Understand the Problem Like a Common Man

Imagine you're inside a garden made of square tiles.

Some tiles are safe.

Some tiles are blocked.

```text
0 = Safe
1 = Blocked
```

You stand here

```text
S
```

and want to reach

```text
E
```

You may walk in all eight directions.

Your goal is **NOT** to find every possible path.

Your goal is **ONLY** to find the shortest one.

---

# Step 2: Identify the Important Keywords

Whenever solving coding problems, train your brain to search for keywords.

Here the important words are

```text
Shortest Path
Minimum
Fewest Moves
```

Whenever you see these words,

immediately think

> **Graph Problem**

Even though the input is a matrix.

---

# Step 3: Why is This a Graph?

Many beginners think

> "This is a matrix, not a graph."

Actually,

every matrix can be converted into a graph.

Each cell becomes a node.

For example,

```text
0 0

0 0
```

becomes

```text
A ----- B
| \   / |
|  \ /  |
|  / \  |
| /   \ |
C ----- D
```

Each neighboring cell has an edge.

Therefore,

this matrix is secretly a graph.

---

# Step 4: Which Graph Algorithm Should We Use?

There are many graph algorithms.

| Problem Type | Algorithm |
|-------------|-----------|
| Visit everything | DFS |
| Shortest path (equal cost) | BFS |
| Weighted shortest path | Dijkstra |
| Negative weights | Bellman Ford |

Now ask yourself

> Does every movement cost the same?

Yes.

Each movement costs exactly **1 step**.

Therefore,

the correct algorithm is

# Breadth First Search (BFS)

---

# Step 5: Why BFS?

Imagine throwing a stone into a pond.

The water spreads like this.

Distance 0

```text
S
```

Distance 1

```text
***
*S*
***
```

Distance 2

```text
*******
*******
***S***
*******
*******
```

BFS spreads exactly like these waves.

It first explores

```text
1 step away
```

then

```text
2 steps away
```

then

```text
3 steps away
```

and so on.

Because of this,

the **first time** BFS reaches the destination,

it is guaranteed to be the shortest path.

---

# Step 6: Why Not DFS?

DFS behaves differently.

It says

```text
I'll keep walking...

walking...

walking...

walking...
```

Maybe it finds a very long path.

Example

```text
S

↓

↓

↓

↓

↓

↓

↓

E
```

But another path might be

```text
S → E
```

DFS does not know that until much later.

BFS always checks the shorter paths first.

---

# Step 7: Building the Intuition

Whenever solving a new problem,

ask yourself these questions.

## Question 1

What is one state?

Answer

```text
(row, column)
```

---

## Question 2

From one state,

where can I move?

Answer

Eight neighboring cells.

---

## Question 3

When do I stop?

Answer

When

```python
row == n - 1

and

col == n - 1
```

---

## Question 4

Can I visit the same place again?

No.

Otherwise,

```text
A → B

↑   ↓

←---
```

Infinite loop.

Therefore,

we need

```text
visited
```

---

# Step 8: BFS Data Structure

BFS always uses a queue.

Initially,

the queue contains only the starting cell.

```text
Queue

[(0,0)]
```

But we also need distance.

So store

```text
(row, column, distance)
```

Initially,

```text
[(0,0,1)]
```

Distance starts from **1**

because the starting cell itself counts.

---

# Step 9: The Eight Directions

Instead of writing

```text
Go Left

Go Right

Go Up

Go Down

Go Diagonal
```

Use loops.

```python
for dx in (-1, 0, 1):
    for dy in (-1, 0, 1):
```

These generate

```text
(-1,-1)

(-1,0)

(-1,1)

(0,-1)

(0,0)

(0,1)

(1,-1)

(1,0)

(1,1)
```

Notice

```text
(0,0)
```

means

"Stay where you are."

That is not a move.

So skip it.

```python
if dx == 0 and dy == 0:
    continue
```

---

# Step 10: Valid Neighbor

Suppose we are at

```text
(x, y)
```

Neighbor

```text
(nx, ny)
```

is valid only if

### 1. Inside the grid

```python
0 <= nx < n
0 <= ny < n
```

---

### 2. Cell is open

```python
grid[nx][ny] == 0
```

---

### 3. Not already visited

```python
(nx, ny) not in visited
```

Only then push it into the queue.

---

# Step 11: Dry Run

Input

```text
grid = [
 [0,0,0],
 [1,1,0],
 [1,1,0]
]
```

Visualization

```text
S  .  .
X  X  .
X  X  E
```

---

## Initial State

Queue

```text
[(0,0,1)]
```

Visited

```text
{(0,0)}
```

---

## Pop

```text
(0,0,1)
```

Neighbors

```text
(-1,-1) ❌

(-1,0) ❌

(-1,1) ❌

(0,-1) ❌

(0,1) ✅

(1,-1) ❌

(1,0) Blocked

(1,1) Blocked
```

Queue

```text
[(0,1,2)]
```

Visited

```text
(0,0)

(0,1)
```

---

## Pop

```text
(0,1,2)
```

Valid neighbors

```text
(0,2)

(1,2)
```

Queue

```text
[(0,2,3),
 (1,2,3)]
```

---

## Pop

```text
(0,2,3)
```

Nothing useful.

Queue

```text
[(1,2,3)]
```

---

## Pop

```text
(1,2,3)
```

Neighbor

```text
(2,2)
```

Queue

```text
[(2,2,4)]
```

---

## Pop

```text
(2,2,4)
```

Destination reached.

Answer

```text
4
```

---

# Step 12: Why BFS Always Gives the Shortest Path

Suppose there are two paths.

```text
Path A = 4 steps

Path B = 8 steps
```

BFS explores

```text
Distance 1

↓

Distance 2

↓

Distance 3

↓

Distance 4
```

before it ever starts

```text
Distance 5

↓

Distance 6

↓

Distance 7

↓

Distance 8
```

Therefore,

the first time we reach the destination,

it is guaranteed to be the shortest path.

This property works because every edge has equal cost.

---

# My Python Code

```python
class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        n = len(grid)

        if grid[0][0] == 1 or grid[n-1][n-1] == 1:
            return -1

        def bfs():
            visited = set()
            q = deque([[0, 0, 1]])

            while q:
                x, y, path = q.popleft()

                if x == n-1 and y == n-1:
                    return path

                for dx in (-1, 0, 1):
                    for dy in (-1, 0, 1):
                        nx = x + dx
                        ny = y + dy

                        if (
                            0 <= nx < n and
                            0 <= ny < n and
                            grid[nx][ny] == 0 and
                            (nx, ny) not in visited
                        ):
                            q.append([nx, ny, path+1])
                            visited.add((nx, ny))

            return -1

        return bfs()
```

---

# Small Improvement

Mark the starting node as visited immediately.

```python
visited = {(0,0)}
```

Otherwise,

another node can push `(0,0)` into the queue again.

---

Also skip

```python
dx == 0 and dy == 0
```

because staying at the same position is not a move.

---

# Optimized Python Solution

```python
from collections import deque

class Solution:
    def shortestPathBinaryMatrix(self, grid):
        n = len(grid)

        # If start or destination is blocked
        if grid[0][0] == 1 or grid[n - 1][n - 1] == 1:
            return -1

        visited = {(0, 0)}
        queue = deque([(0, 0, 1)])

        while queue:
            row, col, distance = queue.popleft()

            # Destination reached
            if row == n - 1 and col == n - 1:
                return distance

            # Explore all 8 directions
            for dx in (-1, 0, 1):
                for dy in (-1, 0, 1):

                    # Skip staying in the same cell
                    if dx == 0 and dy == 0:
                        continue

                    nr = row + dx
                    nc = col + dy

                    if (
                        0 <= nr < n
                        and 0 <= nc < n
                        and grid[nr][nc] == 0
                        and (nr, nc) not in visited
                    ):
                        visited.add((nr, nc))
                        queue.append((nr, nc, distance + 1))

        return -1
```

---

# Time Complexity

There are at most

```text
n²
```

cells.

Each cell is visited only once.

Each visit checks at most

```text
8 neighbors
```

Therefore,

```text
Time Complexity = O(n²)
```

---

# Space Complexity

The queue and visited set can contain every cell.

```text
Space Complexity = O(n²)
```

---

# How to Build the Intuition for Future Problems

Whenever you see a new problem, ask yourself these questions:

### 1. Am I looking for the shortest path?

- Yes → Think Graph.

---

### 2. Can each state become a node?

- Here, each open cell is a node.

---

### 3. What are the edges?

- Moving to any of the 8 neighboring cells.

---

### 4. Does every move have the same cost?

- Yes → BFS
- No → Dijkstra

---

### 5. Can I revisit nodes?

- No → Use a `visited` set (or mark cells in the grid).

---

# Key Takeaways

- A matrix can often be treated as a graph.
- Each cell is a graph node.
- Neighboring cells form graph edges.
- Since every move has equal cost, BFS guarantees the shortest path.
- Always mark nodes as visited when you **enqueue** them.
- The first time BFS reaches the destination is always the shortest path.
- For grid shortest-path problems, remember the pattern:

```text
Shortest Path
        ↓
Graph
        ↓
Equal Edge Cost?
        ↓
      YES
        ↓
       BFS
```

<br/><br/><br/><br/><br/>

---

# 1631. Path With Minimum Effort

**Difficulty:** Medium

---

# Problem Statement

You are given a `rows × columns` grid called `heights`.

Each cell represents the height of that position.

You start at the **top-left corner `(0,0)`** and want to reach the **bottom-right corner `(rows-1, columns-1)`**.

You can move only in four directions:

- Up
- Down
- Left
- Right

The effort of moving between two adjacent cells is:

```python
abs(height1 - height2)
```

The effort of an entire path is **NOT the sum** of all efforts.

Instead,

> **The effort of a path is the maximum height difference between any two consecutive cells in that path.**

Your goal is to find a path whose effort is as small as possible.

---

# Understanding the Problem Like a Common Man

Imagine you're hiking on mountains.

Each number is the height of the land.

```
1 2 2
3 8 2
5 3 5
```

Suppose you walk like this:

```
1 → 3 → 5 → 3 → 5
```

The jump at every step is

```
|1-3| = 2
|3-5| = 2
|5-3| = 2
|3-5| = 2
```

The largest jump is

```
2
```

So the effort of this path is

```
2
```

---

Now consider another path.

```
1 → 2 → 2 → 2 → 5
```

Step differences

```
1
0
0
3
```

Largest jump

```
3
```

Even though most steps are easy,

there is one difficult jump of **3**.

So the effort becomes

```
3
```

We always care about

> **the biggest jump in the entire path.**

Not the total.

---

# Key Observation

We are trying to

```
Minimize

the Maximum

difference
```

This is called a **Minimax Problem**.

---

# How Should We Think?

Whenever you solve a graph problem, ask yourself:

### Question 1

What am I minimizing?

Is it

- Number of moves?
- Sum of costs?
- Maximum cost?
- Minimum cost?

Here,

we are minimizing

```
Maximum Edge Difference
```

---

### Question 2

Can BFS solve it?

No.

Because every edge has different cost.

Example

```
1 → 100

cost = 99
```

Another edge

```
100 → 101

cost = 1
```

Different edge weights mean BFS is not suitable.

---

### Question 3

Can DFS solve it?

Yes.

But DFS explores every possible path.

For a grid,

the number of paths grows exponentially.

Too slow.

---

# Why Dijkstra?

Normally Dijkstra is used to find

```
Minimum Sum
```

For example

```
2 + 5 + 3 = 10
```

But here

we don't care about the sum.

We care about

```
Maximum Edge
```

Can Dijkstra still work?

**Yes.**

We only need to modify how we calculate the path cost.

---

# The Main Intuition

Suppose you've already travelled.

Current maximum effort is

```
4
```

Now the next edge has effort

```
7
```

Then the whole path effort becomes

```
max(4,7)=7
```

Another example

Current effort

```
8
```

Next edge

```
2
```

Whole path

```
max(8,2)=8
```

Therefore,

every time we move,

our new effort is

```python
newEffort = max(currentEffort, edgeDifference)
```

This single line is the entire idea of this problem.

---

# Why Does Dijkstra Still Work?

Suppose there are two ways to reach the same cell.

First path

```
Effort = 2
```

Second path

```
Effort = 5
```

Clearly,

the first one is always better.

No matter where you go later,

starting with effort **2** can never become worse than starting with **5**.

This property makes Dijkstra work.

---

# Mathematical Logic

Suppose

```
Current Path Effort = E
```

Next edge difference is

```
D
```

Then

```
New Path Effort

=

max(E,D)
```

Why?

Because

```
Path Effort

=

Largest Edge Difference
```

If the largest difference so far is

```
6
```

and next edge is

```
2
```

Largest remains

```
6
```

If next edge is

```
10
```

Largest becomes

```
10
```

Hence

```python
newEffort = max(currentEffort, edgeDifference)
```

---

# Building the Intuition

Think of carrying a heavy backpack.

Initially,

```
Weight = 0
```

Every time you walk,

if the next jump is larger than every previous jump,

your backpack becomes heavier.

Otherwise,

its weight stays the same.

Example

Current backpack

```
5
```

Next jump

```
2
```

Backpack

```
5
```

Next jump

```
9
```

Backpack

```
9
```

The backpack never becomes lighter.

Exactly like

```python
max(previousMaximum, currentDifference)
```

---

# Step-by-Step Algorithm

## Step 1

Create an effort matrix.

```
effort[i][j]

=

Minimum effort needed to reach cell (i,j)
```

Initially

```
0 INF INF
INF INF INF
INF INF INF
```

---

## Step 2

Use a Min Heap.

The heap stores

```
(Current Effort, Row, Column)
```

Initially

```
(0,0,0)
```

---

## Step 3

Always remove the smallest effort cell.

This guarantees

we always process the currently best path first.

---

## Step 4

For every neighbour

calculate

```python
difference

=

abs(height[current]-height[next])
```

Then

```python
newEffort

=

max(currentEffort,difference)
```

---

## Step 5

If

```
newEffort

<

stored effort
```

update it

and push into heap.

---

## Step 6

When destination is removed from the heap,

that answer is guaranteed to be minimum.

---

# Dry Run

Grid

```
1 2 2
3 8 2
5 3 5
```

Initially

Heap

```
[(0,(0,0))]
```

Effort Matrix

```
0 INF INF
INF INF INF
INF INF INF
```

---

## Pop

```
(0,0)
```

Neighbours

Right

```
1 → 2

Difference

1

New Effort

max(0,1)=1
```

Update

Down

```
1 → 3

Difference

2

New Effort

2
```

Heap

```
(1,(0,1))

(2,(1,0))
```

---

## Pop

```
(1,(0,1))
```

Neighbours

Right

```
2 → 2

Difference

0

New Effort

max(1,0)=1
```

Update

Down

```
2 → 8

Difference

6

New Effort

6
```

Heap

```
(1,(0,2))

(2,(1,0))

(6,(1,1))
```

---

## Pop

```
(1,(0,2))
```

Neighbour

```
2 → 2

Difference

0

New Effort

1
```

Heap

```
(1,(1,2))

(2,(1,0))

(6,(1,1))
```

---

## Pop

```
(1,(1,2))
```

Neighbour

```
2 → 5

Difference

3

New Effort

3
```

Heap

```
(2,(1,0))

(3,(2,2))

(6,(1,1))
```

---

## Pop

```
(2,(1,0))
```

Neighbour

```
3 → 5

Difference

2

New Effort

2
```

Eventually

we reach

```
(2,2)
```

with effort

```
2
```

Answer

```
2
```

---

# What's Wrong With Your Code?

```python
class Solution:
    def minimumEffortPath(self, heights: List[List[int]]) -> int:
        m = len(heights)
        n = len(heights[0])
        if m == 1 and n == 1:
            return 0
        pq = []
        heapq.heappush(pq, (0, 0, 0, 0))
        distance = [
            (-1, 0),
            (1, 0),
            (0, -1),
            (0, 1)
        ]
        visited = set()
        visited.add((float('inf'),0,0))
        while pq:
            diff, x, y, ans = heapq.heappop(pq)
            ans = max(ans, diff)
            if x == m-1 and y == n-1:
                return ans
            for dx, dy in distance:
                nx = x + dx
                ny = y + dy
                if (
                    0 <= nx < m and
                    0 <= ny < n
                ):
                    newVal = heights[nx][ny]
                    oldVal = heights[x][y]
                    change = abs(newVal - oldVal)
                    if (change, nx, ny) not in visited:
                        heapq.heappush(pq, (change, nx, ny, ans))
                        visited.add((change, nx, ny))     
```

Your approach is very close.

The idea of using a priority queue is correct.

However, there are several mistakes.

---

## Mistake 1

You push

```python
(change,nx,ny,ans)
```

where

```
change

=

current edge difference
```

But Dijkstra should prioritize

```
Entire Path Effort
```

Instead,

push

```python
newEffort = max(ans,change)

heapq.heappush(
    pq,
    (newEffort,nx,ny)
)
```

---

## Mistake 2

Your visited set is

```python
(change,nx,ny)
```

This is incorrect.

The same cell can be reached with different efforts.

Example

```
Cell (2,2)

Reached with effort 8

Later reached with effort 3
```

The second path is better.

But your visited set blocks it.

---

## Mistake 3

You should maintain an effort matrix

```python
effort = [
    [INF]*n
    for _ in range(m)
]
```

Instead of

```python
visited
```

---

## Mistake 4

The variable

```python
ans
```

is unnecessary.

The heap itself already stores the best path effort.

Simply store

```
(Current Effort,Row,Column)
```

---

# Correct Python Solution

```python
import heapq

class Solution:
    def minimumEffortPath(self, heights):

        m = len(heights)
        n = len(heights[0])

        effort = [[float("inf")] * n for _ in range(m)]
        effort[0][0] = 0

        pq = [(0, 0, 0)]

        directions = [
            (-1, 0),
            (1, 0),
            (0, -1),
            (0, 1)
        ]

        while pq:

            currentEffort, x, y = heapq.heappop(pq)

            if currentEffort > effort[x][y]:
                continue

            if x == m - 1 and y == n - 1:
                return currentEffort

            for dx, dy in directions:

                nx = x + dx
                ny = y + dy

                if 0 <= nx < m and 0 <= ny < n:

                    difference = abs(
                        heights[nx][ny] -
                        heights[x][y]
                    )

                    newEffort = max(
                        currentEffort,
                        difference
                    )

                    if newEffort < effort[nx][ny]:

                        effort[nx][ny] = newEffort

                        heapq.heappush(
                            pq,
                            (newEffort, nx, ny)
                        )

        return 0
```

---

# Time Complexity

There are

```
V = m × n
```

vertices.

Each cell has at most

```
4
```

edges.

Dijkstra complexity

```
O(V log V)

=

O(mn log(mn))
```

---

# Space Complexity

```
O(mn)
```

for

- effort matrix
- priority queue

---

# Pattern Recognition for Interviews

Whenever you see

```
Minimum

Maximum

Minimum

Maximum
```

Think

> **Minimax Problem**

Whenever you see

```
Grid

+

Weighted Edges

+

Best Path
```

Think

> **Dijkstra**

Whenever the path cost is **not the sum**, ask yourself:

> **Can I redefine the path cost and replace `+` with another operation?**

Examples:

```
Shortest Path

newCost = oldCost + edge
```

```
Minimum Effort Path

newCost = max(oldCost, edge)
```

```
Maximum Probability Path

newCost = oldProbability × edgeProbability
```

The biggest intuition to learn is:

> **Dijkstra is not just for addition. It works whenever you can define a path cost that only depends on the previous path cost and the current edge, and where reaching a node with a smaller cost is always better than reaching it with a larger one.**

<br/><br/><br/><br/><br/>

---

# 787. Cheapest Flights Within K Stops

**Difficulty:** Medium

---

# Problem Statement

You are given:

- `n` cities numbered from `0` to `n-1`
- A list of flights

Each flight is represented as:

```python
[from, to, price]
```

which means

- You can travel from city `from`
- To city `to`
- By paying `price`

You are also given

- `src` → starting city
- `dst` → destination city
- `k` → maximum number of stops allowed

Your task is to return the **minimum cost** to reach the destination.

If no valid route exists, return **-1**.

---

# What is a Stop?

This is the most confusing part.

A **stop** means an intermediate city.

For example

```
0 → 1 → 3
```

Flights taken = 2

Intermediate cities =

```
1
```

Stops = **1**

This is allowed if

```text
k = 1
```

---

Now consider

```
0 → 1 → 2 → 3
```

Intermediate cities

```
1
2
```

Stops = **2**

This is NOT allowed if

```text
k = 1
```

---

## Important Observation

If maximum stops = `k`

Then maximum flights allowed are

```
k + 1
```

---

# Example

Input

```python
n = 4

flights = [
    [0,1,100],
    [1,2,100],
    [2,0,100],
    [1,3,600],
    [2,3,200]
]

src = 0
dst = 3
k = 1
```

Graph

```
          100
     0 --------->1
      ^          | \
      |100       |600
      |          |
      |          v
      2--------->3
        200
```

Possible routes

```
0 → 1 → 3

Cost = 700
Stops = 1
```

Valid ✅

---

Another route

```
0 → 1 → 2 → 3

Cost = 400
Stops = 2
```

Cheaper

BUT

Not valid ❌

---

Answer

```
700
```

---

# How Should a Normal Person Think?

Suppose someone asks

> Find the cheapest route.

What would you naturally do?

You would

- Start from the source
- Explore all flights
- Keep calculating costs
- Ignore routes having more than `k` stops
- Finally choose the cheapest one

This is exactly what the algorithm does.

---

# Step 1: Identify the Type of Problem

Whenever you see

- cities
- roads
- flights
- network
- minimum cost

Think

```
Graph Problem
```

---

# Step 2: Build the Graph

Flights

```python
[
[0,1,100],
[1,2,100],
[2,0,100],
[1,3,600],
[2,3,200]
]
```

Convert them into

```
0

→ (1,100)

1

→ (2,100)

→ (3,600)

2

→ (0,100)

→ (3,200)
```

Python

```python
graph = defaultdict(list)

for u, v, w in flights:
    graph[u].append((v, w))
```

Now every city knows where it can go.

---

# Step 3: What Information Do We Need?

Imagine you are currently traveling.

What do you need to remember?

Three things

```
Current city

Current cost

Number of flights taken
```

So one state becomes

```
(city, flights_taken, total_cost)
```

Example

```
(0,0,0)
```

means

```
At city 0

Taken 0 flights

Spent $0
```

---

# Step 4: Why BFS?

Normally BFS finds

```
Minimum number of edges
```

But here we are minimizing cost.

So why use BFS?

Because BFS explores

```
0 flights

1 flight

2 flights

3 flights
```

level by level.

Since we only care up to

```
k + 1 flights
```

BFS naturally limits our search.

---

# Step 5: Why Keep a Distance Array?

Suppose you already reached city 2

for

```
$200
```

Later another route reaches city 2

for

```
$500
```

Would you continue exploring from the $500 route?

No.

Because it is already worse.

So we store

```python
dist[node]
```

meaning

```
Cheapest cost found so far to reach this city
```

Initialize

```python
dist = [float("inf")] * n

dist[src] = 0
```

---

# Step 6: BFS Algorithm

Start

```
Queue

[(0,0,0)]
```

Meaning

```
(city,
flights_taken,
cost)
```

Remove it.

Explore every neighboring city.

Whenever

```
New Cost

<

Old Cost
```

Update the distance.

Push the new state into the queue.

Repeat.

---

# Step 7: Why This Condition?

```python
if cost + price < dist[neighbor] and flights_taken <= k:
```

Let's understand both parts.

---

## Part 1

```python
cost + price < dist[neighbor]
```

Means

```
Have we found a cheaper route?
```

If yes

Update it.

---

## Part 2

```python
flights_taken <= k
```

Means

```
Can we still take another flight?
```

If yes

Continue exploring.

Otherwise

Stop.

---

# Dry Run

Input

```python
n = 4

flights = [
    [0,1,100],
    [1,2,100],
    [2,0,100],
    [1,3,600],
    [2,3,200]
]

src = 0
dst = 3
k = 1
```

---

## Initial State

Distance

```
[0,∞,∞,∞]
```

Queue

```
[(0,0,0)]
```

---

## Pop

```
(0,0,0)
```

Neighbors

```
1

Cost

0+100=100
```

Update

```
dist

[0,100,∞,∞]
```

Queue

```
[(1,1,100)]
```

---

## Pop

```
(1,1,100)
```

Neighbor

```
2

Cost

200
```

Update

```
dist

[0,100,200,∞]
```

Queue

```
[(2,2,200)]
```

---

Second Neighbor

```
3

Cost

700
```

Update

```
dist

[0,100,200,700]
```

Queue

```
[(2,2,200),
 (3,2,700)]
```

---

## Pop

```
(2,2,200)
```

Flights taken

```
2
```

Allowed?

```
2 <= 1

False
```

Cannot continue.

---

## Pop

```
(3,2,700)
```

Destination reached.

Answer

```
700
```

---

# Time Complexity

Building Graph

```
O(E)
```

where

```
E = Number of Flights
```

Traversal

Approximately

```
O(E × K)
```

because every edge can be considered across the limited number of allowed flight levels.

---

# Space Complexity

Graph

```
O(E)
```

Distance Array

```
O(V)
```

Queue

```
O(V)
```

Overall

```
O(V + E)
```

---

# Python Solution (BFS)

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        graph = defaultdict(list)
        for u, v, w in flights:
            graph[u].append((v, w))

        def bfs():
            dist = [float('inf')] * n
            dist[src] = 0
            q = deque()
            q.append([src, 0, 0]) # node, level, cost

            while q:
                node, level, cost = q.popleft()
                for nei, neiWeight in graph[node]:
                    if cost + neiWeight < dist[nei] and level <= k:
                        q.append([nei, level + 1, cost + neiWeight])
                        dist[nei] = cost + neiWeight

            if dist[dst] == float('inf'):
                return -1
            return dist[dst]
        
        return bfs()
```

---

# Key Takeaways

- Convert flights into an adjacency list.
- Think in terms of **states**: `(city, flights_taken, total_cost)`.
- BFS explores routes level by level, which naturally fits the flight limit.
- Always track the cheapest cost found so far.
- Always ask yourself:
  1. What is the graph?
  2. What am I minimizing?
  3. What is the constraint?
  4. What information defines my current state?
  5. Can I safely ignore worse paths?
