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
- [1976. Number of Ways to Arrive at Destination](#1976-number-of-ways-to-arrive-at-destination)
- [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance](#1334-find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance)
- [2359. Find Closest Node to Given Two Nodes](#2359-find-closest-node-to-given-two-nodes)

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

<br/><br/><br/><br/><br/>

---

# 1976. Number of Ways to Arrive at Destination

**Difficulty:** Medium

---

# Problem Statement

You are given:

- `n` intersections (cities) numbered from `0` to `n-1`.
- Roads connecting these intersections.
- Each road has a travel time.

Each road is represented as

```python
[u, v, time]
```

meaning

- There is a road between `u` and `v`.
- It takes `time` minutes to travel.
- Roads are **bidirectional**, so you can travel both ways.

Your task is:

> Find **how many different shortest paths** exist from intersection **0** to intersection **n-1**.

Since the answer can become very large, return it modulo

```text
10^9 + 7
```

---

# Let's Understand the Problem Like a Common Person

Imagine you're using Google Maps.

You want to travel from your home to your office.

Google tells you

> Fastest time = **20 minutes**

Now you ask another question:

> "How many different routes also take exactly 20 minutes?"

Not

- How many total routes?
- How many possible paths?

Only

> **How many routes have the minimum travel time?**

That is exactly this problem.

---

# Example

Input

```python
n = 2

roads = [
    [1,0,10]
]
```

Graph

```
0 --------10-------- 1
```

Only one path

```
0 → 1
```

Shortest time

```
10
```

Number of shortest paths

```
1
```

Answer

```
1
```

---

# Example 2

```
          2
     0 --------1
      \         \
       \         3
        5         \
         \         \
          4---------6
```

There are several different routes.

Some take

```
7 minutes
```

Others take

```
9 minutes
```

Others take

```
12 minutes
```

We only count the routes whose total time equals the minimum possible time.

---

# Step 1 : Identify the Problem Type

Whenever you see

- cities
- roads
- minimum time
- minimum distance
- cheapest route

Think immediately

```
Shortest Path Problem
```

---

# Step 2 : Which Algorithm?

There are many shortest path algorithms.

| Algorithm | Used For |
|------------|----------|
| BFS | Equal edge weights |
| DFS | Explore everything |
| Dijkstra | Positive weights ✅ |
| Bellman Ford | Negative weights |
| Floyd Warshall | Every pair shortest path |

Here

Road times are always positive.

```
1 <= time <= 10^9
```

Therefore

> **Dijkstra's Algorithm** is the correct choice.

---

# Step 3 : But There Is One Extra Requirement

Normally Dijkstra finds

```
Shortest Distance
```

This problem asks

```
Shortest Distance

+

Number of shortest paths
```

So we need one extra array.

---

# Step 4 : Build the Graph

Input

```python
roads = [
    [0,6,7],
    [0,1,2],
    [1,2,3]
]
```

Convert into adjacency list.

```
0

→ (6,7)

→ (1,2)

1

→ (0,2)

→ (2,3)

6

→ (0,7)
```

Python

```python
graph = defaultdict(list)

for u, v, w in roads:
    graph[u].append((v, w))
    graph[v].append((u, w))
```

Because roads are bidirectional.

---

# Step 5 : What Information Should We Store?

Normally Dijkstra stores

```
Shortest distance
```

Now we also need

```
How many shortest paths reach this node?
```

So we maintain two arrays.

---

## Distance Array

```
dist[i]
```

means

> Shortest distance from source to node i.

Initially

```
dist

[0,∞,∞,∞...]
```

because

Source distance is

```
0
```

Everything else

```
Unknown
```

---

## Ways Array

```
ways[i]
```

means

> Number of shortest paths reaching node i.

Initially

```
ways

[1,0,0,0...]
```

Why?

Because

There is exactly one way to stand at the source.

Do nothing.

---

# Step 6 : Why Use a Min Heap?

Imagine you have many cities waiting.

```
City A

Distance = 4

City B

Distance = 10

City C

Distance = 2
```

Which city should we explore first?

Obviously

```
City C
```

Because it has the smallest known distance.

A Min Heap automatically gives us

```
Smallest distance first
```

---

# Step 7 : The Main Logic

Suppose we're at

```
Node u
```

Current shortest distance

```
dist[u]
```

Road

```
u ----5----> v
```

New distance

```
newDistance

=

dist[u] + weight
```

Now compare.

---

## Case 1

```
newDistance

<

dist[v]
```

Amazing!

We found a shorter route.

Update

```
dist[v]
```

Also

All shortest paths to

```
v
```

must now come from

```
u
```

Therefore

```
ways[v]

=

ways[u]
```

---

## Case 2

Suppose

```
newDistance

==

dist[v]
```

Interesting.

We found another shortest path.

Old shortest path

```
0 → A → V
```

New shortest path

```
0 → B → V
```

Both take the same time.

Therefore

```
ways[v]

+=

ways[u]
```

Don't replace.

Add.

---

## Case 3

```
newDistance

>

dist[v]
```

Ignore it.

It is longer.

---

# The Mathematics Behind It

Suppose

```
ways[A] = 3
```

Meaning

There are

```
3 shortest ways
```

to reach

```
A
```

Now suppose

```
A → B
```

is part of a shortest route.

Every shortest path reaching

```
A
```

can continue to

```
B
```

So

```
ways[B]

=

ways[A]
```

If another shortest path reaches

```
B
```

from somewhere else

Then

```
ways[B]

=

ways[B]

+

ways[other]
```

This is just counting combinations.

---

# Visual Example

Suppose

```
      A
     /
0---<
     \
      B
```

Both

```
0→A
```

and

```
0→B
```

take

```
5
```

Now

```
A→C

B→C
```

both take

```
2
```

Then

```
0→A→C

7
```

and

```
0→B→C

7
```

Both are shortest.

Therefore

```
ways[C]

=

2
```

---

# Dry Run

Example

```python
n = 2

roads = [
    [1,0,10]
]
```

---

Initial

Distance

```
[0,∞]
```

Ways

```
[1,0]
```

Heap

```
[(0,0)]
```

Meaning

```
(distance,node)
```

---

## Pop Heap

```
(0,0)
```

Neighbor

```
1

Weight = 10
```

New Distance

```
0+10=10
```

Compare

```
10<∞
```

Yes.

Update

Distance

```
[0,10]
```

Ways

```
ways[1]

=

ways[0]

=

1
```

Push

```
(10,1)
```

---

## Pop

```
(10,1)
```

Nothing better found.

Done.

Answer

```
ways[1]

=

1
```

---

# Dry Run of the Larger Example

Input

```python
roads = [
[0,6,7],
[0,1,2],
[1,2,3],
[1,3,3],
[6,3,3],
[3,5,1],
[6,5,1],
[2,5,1],
[0,4,5],
[4,6,2]
]
```

Initially

```
dist = [0,∞,∞,∞,∞,∞,∞]

ways = [1,0,0,0,0,0,0]
```

Explore from node `0`:

- `0 → 6` gives distance `7`
- `0 → 1` gives distance `2`
- `0 → 4` gives distance `5`

```
dist = [0,2,∞,∞,5,∞,7]

ways = [1,1,0,0,1,0,1]
```

Next, process node `1` (distance `2`):

- `1 → 2` gives `5`
- `1 → 3` gives `5`

```
dist = [0,2,5,5,5,∞,7]

ways = [1,1,1,1,1,0,1]
```

Next, process nodes with distance `5` (`2`, `3`, `4`):

From `2`:

```
2 → 5 = 6

ways[5] = ways[2] = 1
```

From `3`:

```
3 → 5 = 6

Equal shortest distance

ways[5] = 1 + 1 = 2
```

From `4`:

```
4 → 6 = 7

Equal shortest distance

ways[6] = 1 + 1 = 2
```

Current state

```
dist = [0,2,5,5,5,6,7]

ways = [1,1,1,1,1,2,2]
```

Now process node `5` (distance `6`):

```
5 → 6 = 7

Equal shortest distance

ways[6]

=

2 + ways[5]

=

2 + 2

=

4
```

Final

```
dist[6] = 7

ways[6] = 4
```

There are exactly **4 shortest paths**.

---

# Time Complexity

Building Graph

```
O(E)
```

Dijkstra

```
O((V+E) log V)
```

where

- `V` = Number of intersections
- `E` = Number of roads

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

Ways Array

```
O(V)
```

Heap

```
O(V)
```

Overall

```
O(V + E)
```

---

# Python Solution

```python
from collections import defaultdict
import heapq

class Solution:
    def countPaths(self, n, roads):

        MOD = 10**9 + 7

        graph = defaultdict(list)

        # Build graph
        for u, v, w in roads:
            graph[u].append((v, w))
            graph[v].append((u, w))

        dist = [float("inf")] * n
        ways = [0] * n

        dist[0] = 0
        ways[0] = 1

        heap = [(0, 0)]   # (distance, node)

        while heap:

            currentDist, node = heapq.heappop(heap)

            # Skip outdated entries
            if currentDist > dist[node]:
                continue

            for neighbor, weight in graph[node]:

                newDist = currentDist + weight

                # Found a shorter path
                if newDist < dist[neighbor]:

                    dist[neighbor] = newDist
                    ways[neighbor] = ways[node]

                    heapq.heappush(
                        heap,
                        (newDist, neighbor)
                    )

                # Found another shortest path
                elif newDist == dist[neighbor]:

                    ways[neighbor] = (
                        ways[neighbor] + ways[node]
                    ) % MOD

        return ways[n - 1]
```

---

# Why This Works

The algorithm combines **Dijkstra's shortest path** with **dynamic counting**.

- `dist[]` always stores the minimum time needed to reach each node.
- `ways[]` stores **how many different shortest paths** achieve that minimum time.
- Whenever a strictly shorter path is found, we replace both the distance and the number of ways.
- Whenever another path with the **same shortest distance** is found, we add its count to the existing count.
- Dijkstra guarantees that when a node is processed with its minimum distance, no future path can improve that distance, making the counting correct.

---

# How to Build the Intuition

Whenever you encounter a graph problem, ask yourself these questions:

### 1. What is the graph?

```
Intersections

Roads
```

---

### 2. What am I optimizing?

```
Minimum travel time
```

---

### 3. Are edge weights positive?

```
Yes

→ Dijkstra
```

---

### 4. Do I need only the shortest distance?

No.

I also need

```
How many shortest paths exist?
```

---

### 5. What extra information should I maintain?

```
dist[]

+

ways[]
```

---

### 6. What happens when I discover a path?

- **Shorter path?** Replace the distance and copy the number of ways.
- **Equal shortest path?** Add the number of ways.
- **Longer path?** Ignore it.

---

# Key Takeaways

- Recognize **positive weighted graph → Dijkstra**.
- Build the graph using an adjacency list.
- Maintain two arrays:
  - `dist[]` → shortest distance.
  - `ways[]` → number of shortest paths.
- If a **shorter** path is found, **replace** the distance and **copy** the path count.
- If an **equally short** path is found, **add** the path count.
- Always use a **min-heap** so the closest node is processed first.
- Return `ways[n-1] % (10^9 + 7)`.

<br/><br/><br/><br/><br/>

---

# 1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance

---

# Problem Statement

You are given:

- `n` cities numbered from `0` to `n - 1`
- A list of weighted, bidirectional roads:
  ```python
  edges[i] = [from, to, weight]
  ```
- A maximum allowed travel distance called `distanceThreshold`.

For **every city**, find how many other cities can be reached using **any path** whose **total distance** is less than or equal to `distanceThreshold`.

Finally,

- Return the city that can reach the **fewest** cities.
- If multiple cities have the same minimum count, return the **largest city number**.

---

# Example

```text
Input:

n = 4

edges =
[
 [0,1,3],
 [1,2,1],
 [1,3,4],
 [2,3,1]
]

distanceThreshold = 4
```

Graph

```text
       3
0 ------------ 1
              / \
            1/   \4
            /     \
           2-------3
              1
```

Output

```text
3
```

---

# Understanding the Problem Like a Common Man

Imagine there are several cities connected by roads.

Every road has a distance.

Suppose someone asks:

> "Starting from City 0, how many cities can I visit if I can travel at most 4 km?"

You would naturally:

- Walk through the roads.
- Keep adding distances.
- Stop whenever the total distance becomes greater than 4.

Now repeat this process for **every city**.

Finally,

Choose the city that can reach the smallest number of cities.

---

# What is the Problem Really Asking?

Many beginners misunderstand this question.

It is **NOT** asking:

> Which city has the fewest direct neighbors?

Instead it asks:

> Which city can reach the fewest cities through **any path** whose total distance is within the threshold?

Notice the words:

> through some path

That means this is a **Shortest Path Problem**.

---

# First Intuition

Suppose you start at city 0.

There are many possible routes.

```text
0 → 1 → 2

or

0 → 3 → 5

or

0 → 4 → 6
```

Which one is shortest?

You don't know.

So we need an algorithm that always finds the shortest path.

---

# Which Algorithm Should We Use?

Let's think.

### Can we use BFS?

No.

BFS assumes every edge has equal weight.

Example

```text
0 ----10---- 1

0 ----1---- 2 ----1---- 1
```

BFS chooses

```text
0 → 1
```

because it uses only one edge.

But the shortest distance is

```text
0 → 2 → 1

1 + 1 = 2
```

Therefore,

**BFS fails on weighted graphs.**

---

### Dijkstra Algorithm

Dijkstra works perfectly because:

- All edge weights are positive.
- We need shortest distances.

Exactly what Dijkstra is designed for.

---

# Building the Graph

Input

```python
edges = [
    [0,1,3],
    [1,2,1],
    [1,3,4],
    [2,3,1]
]
```

Adjacency List

```text
0
↓
[(1,3)]

--------------------

1
↓
[(0,3), (2,1), (3,4)]

--------------------

2
↓
[(1,1), (3,1)]

--------------------

3
↓
[(1,4), (2,1)]
```

---

# Overall Idea

For every city:

1. Run Dijkstra.
2. Compute shortest distance to every city.
3. Count how many cities have distance ≤ threshold.
4. Keep the city with the smallest count.

---

# Dijkstra Algorithm Explained

Suppose we start from city **0**.

Initially

```text
Distance

0 = 0

1 = ∞

2 = ∞

3 = ∞
```

Priority Queue

```text
[(0,0)]
```

Meaning

```text
(distance, city)
```

---

## Step 1

Pop

```text
(0,0)
```

Visit neighbors.

Neighbor

```text
1

new distance = 0 + 3 = 3
```

Update

```text
dist[1] = 3
```

Push

```text
(3,1)
```

Queue

```text
[(3,1)]
```

---

## Step 2

Pop

```text
(3,1)
```

Neighbors

### City 0

```text
3 + 3 = 6

Current distance = 0

Ignore
```

### City 2

```text
3 + 1 = 4

Update

dist[2] = 4
```

Push

```text
(4,2)
```

### City 3

```text
3 + 4 = 7

Update

dist[3] = 7
```

Queue

```text
[(4,2), (7,3)]
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

4 + 1 = 5
```

Current

```text
7
```

Better path found.

Update

```text
dist[3] = 5
```

Push

```text
(5,3)
```

Queue

```text
[(5,3), (7,3)]
```

---

## Step 4

Pop

```text
(5,3)
```

Nothing improves.

Done.

Final distances

```text
City 0 → 0

City 1 → 3

City 2 → 4

City 3 → 5
```

Threshold

```text
4
```

Reachable cities

```text
1

2
```

Count

```text
2
```

---

# Dry Run of Complete Example

Threshold = 4

## City 0

Shortest distances

```text
0 → 0

1 → 3

2 → 4

3 → 5
```

Reachable

```text
1

2
```

Count = 2

---

## City 1

Distances

```text
0 → 3

1 → 0

2 → 1

3 → 2
```

Reachable

```text
0

2

3
```

Count = 3

---

## City 2

Distances

```text
0 → 4

1 → 1

2 → 0

3 → 1
```

Reachable

```text
0

1

3
```

Count = 3

---

## City 3

Distances

```text
0 → 5

1 → 2

2 → 1

3 → 0
```

Reachable

```text
1

2
```

Count = 2

---

Summary

| City | Reachable Cities | Count |
|------|-----------------|------|
| 0 | 1,2 | 2 |
| 1 | 0,2,3 | 3 |
| 2 | 0,1,3 | 3 |
| 3 | 1,2 | 2 |

Minimum count

```text
2
```

Cities

```text
0

3
```

Tie-break rule

Choose larger index.

Answer

```text
3
```

---

# Why Dijkstra Works

Dijkstra always expands the city with the smallest known distance.

Why?

Suppose a city is removed from the priority queue.

Since it has the minimum distance among all remaining cities, no shorter path can exist.

Therefore,

its distance is final.

This property makes Dijkstra correct for graphs with **positive edge weights**.

---

# Time Complexity

Let

- V = Number of cities
- E = Number of roads

One Dijkstra

```text
O(E log V)
```

We run Dijkstra for every city.

Total

```text
O(V × E log V)
```

Given

```text
n ≤ 100
```

This is completely acceptable.

---

# Improving Your Code

## My python Solution

```python
class Solution:
    def findTheCity(self, n: int, edges: List[List[int]], distanceThreshold: int) -> int:
        graph = defaultdict(list)
        inf = float('inf')
        for u, v, w in edges:
            graph[u].append((v, w))
            graph[v].append((u, w))
        
        ansDict = defaultdict(set)

        def count(node):
            nonlocal n
            pq = [(0, node)]
            dist = [inf] * n
            dist[node] = 0
            while pq:
                cost, city = heapq.heappop(pq)
                if cost > distanceThreshold:
                    break
                if cost > dist[city]:
                    continue
                if cost <= distanceThreshold:
                    ansDict[node].add(city)
                for nei, neiCost in graph[city]:
                    newCost = neiCost + cost
                    if newCost < dist[nei]:
                        dist[nei] = newCost
                        heapq.heappush(pq, (newCost, nei))
            
        for i in range(n):
            count(i)

        for i in range(n):
            if i not in ansDict:
                ansDict[i] = {}

        ans = []
        for node, nei in ansDict.items():
            heapq.heappush(ans, (len(nei), -node))
        
        return -ans[0][1]
```

## Good Parts

✔ Correct graph representation

✔ Correct use of priority queue

✔ Correct shortest path logic

---

## Issue 1

You count the starting city itself.

```python
ansDict[node].add(city)
```

But the problem asks for **neighboring cities**.

Instead

```python
if city != node:
    ansDict[node].add(city)
```

---

## Issue 2

Using a `set` is unnecessary.

You only need a count.

Instead of storing every city,

just count them after Dijkstra.

This saves memory.

---

## Issue 3

This part

```python
if cost > distanceThreshold:
    break
```

is actually safe.

Since the priority queue always gives the smallest distance first,

once the smallest distance exceeds the threshold,

all remaining paths will also exceed it.

---

# Optimized Python Solution

```python
from collections import defaultdict
import heapq


class Solution:
    def findTheCity(self, n, edges, distanceThreshold):

        graph = defaultdict(list)

        for u, v, w in edges:
            graph[u].append((v, w))
            graph[v].append((u, w))

        INF = float("inf")

        answer = -1
        minimumReachable = float("inf")

        for start in range(n):

            dist = [INF] * n
            dist[start] = 0

            pq = [(0, start)]

            while pq:

                currentDistance, city = heapq.heappop(pq)

                if currentDistance > dist[city]:
                    continue

                for neighbor, weight in graph[city]:

                    newDistance = currentDistance + weight

                    if newDistance < dist[neighbor]:
                        dist[neighbor] = newDistance
                        heapq.heappush(pq, (newDistance, neighbor))

            reachable = 0

            for city in range(n):
                if city != start and dist[city] <= distanceThreshold:
                    reachable += 1

            if reachable <= minimumReachable:
                minimumReachable = reachable
                answer = start

        return answer
```

---

# Can We Solve This Using Floyd–Warshall?

Yes.

Since

```text
n ≤ 100
```

Floyd–Warshall runs in

```text
O(n³)
```

For

```text
100³ = 1,000,000
```

Only one million operations.

Very fast.

This problem is actually one of the classic applications of Floyd–Warshall because we need the shortest distance between **every pair of cities**.

---

# How to Build the Intuition

Whenever you see a graph problem, ask these questions.

## Step 1

Is this a graph?

```text
Cities

Roads

Connections

Network
```

If yes,

think Graph.

---

## Step 2

Are there weights?

Words like

```text
Distance

Cost

Time

Price

Weight
```

Mean

**Weighted Graph**

---

## Step 3

Do we need the minimum distance?

Words like

```text
Shortest

Minimum

Cheapest

Least Cost

Within Threshold
```

Mean

**Shortest Path Problem**

---

## Step 4

Choose the correct algorithm

| Situation | Algorithm |
|-----------|-----------|
| Unweighted Graph | BFS |
| Positive Weights | Dijkstra |
| Negative Weights | Bellman-Ford |
| All-Pairs Shortest Path (small `n`) | Floyd-Warshall |

---

## Step 5

How many source nodes?

If the question says

```text
For every city...

For each node...

Count reachable nodes from every vertex...
```

Then

Run

- Dijkstra from every node, **or**
- Floyd-Warshall.

---

# Mental Checklist for Similar Problems

```text
Graph?
        ↓
Weighted?
        ↓
Need shortest path?
        ↓
Positive weights?
        ↓
Dijkstra
        ↓
Need answer for every node?
        ↓
Run Dijkstra for every node
(or Floyd-Warshall if n is small)
        ↓
Process the shortest distances
        ↓
Return the required result
```

---

# Key Takeaways

- The problem is fundamentally a **shortest path problem**.
- We need shortest distances from **every city**.
- Dijkstra is suitable because all edge weights are **positive**.
- After computing distances, simply count how many cities are within the threshold.
- Apply the tie-break rule by choosing the **largest city index** when counts are equal.
- Always separate the problem into two phases:
  1. **Compute shortest distances**
  2. **Use those distances to answer the question**

This way of thinking applies to many graph problems and helps build strong intuition for choosing the right algorithm.

<br/><br/><br/><br/><br/>

---

# 2359. Find Closest Node to Given Two Nodes

---

# Problem Statement

You are given:

- A **directed graph**.
- The graph is represented using an array called `edges`.
- Every node has **at most one outgoing edge**.

```python
edges[i] = j
```

means

```text
i ─────► j
```

If

```python
edges[i] = -1
```

then

```text
i

(no outgoing edge)
```

You are also given two starting nodes:

- `node1`
- `node2`

Your task is to find a node that

- can be reached from **both** starting nodes.
- minimizes

```text
max(distance from node1,
    distance from node2)
```

If multiple nodes satisfy this condition,

return the **smallest index**.

If no such node exists,

return **-1**.

---

# Understanding the Graph

Unlike most graph problems,

this graph is special.

Every node has

```text
0 or 1 outgoing edge.
```

Example

```text
edges = [2,2,3,-1]
```

means

```text
0 ─────► 2
1 ─────► 2
2 ─────► 3
3
```

Graph

```text
0 ----\
       \
        ► 2 ► 3

1 ----/
```

Notice something important.

A node can never branch into multiple paths.

Each node has only one road going out.

This observation makes the problem much easier.

---

# Understanding the Question Like a Common Man

Imagine two friends start walking.

Friend A starts from

```text
node1
```

Friend B starts from

```text
node2
```

Both follow the arrows.

They cannot choose different roads because every city has only one outgoing road.

Eventually

- they may meet
- or they may never meet.

Among all possible meeting places,

choose the one that is fairest.

Fair means

> Neither person should travel too much.

Mathematically,

we minimize

```text
maximum(distanceA, distanceB)
```

---

# Why Do We Minimize the Maximum?

Suppose there are two meeting points.

Meeting Point A

```text
Friend 1 travels = 2

Friend 2 travels = 10
```

Maximum distance

```text
10
```

Meeting Point B

```text
Friend 1 travels = 6

Friend 2 travels = 6
```

Maximum distance

```text
6
```

Which is fairer?

Obviously,

Meeting Point B.

Although Friend 1 walks more,

Friend 2 walks much less.

So we minimize the **maximum** distance.

---

# First Intuition

Suppose

```text
node1 = 0

node2 = 1
```

Friend A walks

```text
0

↓

2

↓

3
```

Friend B walks

```text
1

↓

2

↓

3
```

Friend A distances

```text
0 → 0 = 0

2 = 1

3 = 2
```

Friend B distances

```text
1 → 1 = 0

2 = 1

3 = 2
```

Now compare.

Node

```text
2

max(1,1)=1
```

Node

```text
3

max(2,2)=2
```

Smaller maximum is

```text
1
```

Answer

```text
2
```

---

# How Should We Think?

Whenever you see

```text
Distance from node A

Distance from node B
```

Think

> We need the shortest distance from both sources.

Then compare.

This becomes

```text
Source 1
↓

Distances

+

Source 2

↓

Distances

↓

Compare
```

---

# Which Algorithm Should We Use?

Normally,

for shortest path,

we think of

- BFS
- Dijkstra

Which one?

Notice

Every edge has weight

```text
1
```

So

BFS gives shortest distance.

Even better,

because every node has only one outgoing edge,

our BFS is almost just walking along a single chain.

---

# Why BFS Works

Every edge costs exactly

```text
1
```

BFS explores

```text
Distance 0

↓

Distance 1

↓

Distance 2
```

Therefore,

the first time we visit a node,

we have already found its shortest distance.

---

# Overall Algorithm

## Step 1

Start BFS from

```text
node1
```

Store

```text
distance from node1
```

Example

```text
{
0:0,
2:1,
3:2
}
```

---

## Step 2

Run BFS again

from

```text
node2
```

Store

```text
distance from node2
```

Example

```text
{
1:0,
2:1,
3:2
}
```

---

## Step 3

Visit every node.

If it exists in both maps,

compute

```python
max(distance1,
    distance2)
```

Keep the smallest one.

---

# Dry Run

Input

```python
edges = [2,2,3,-1]

node1 = 0

node2 = 1
```

Graph

```text
0

 \
  ►2►3

 /

1
```

---

## BFS from node1

Initially

```text
Queue

[(0,0)]
```

Map

```text
{
0:0
}
```

---

Pop

```text
0
```

Neighbor

```text
2
```

Distance

```text
1
```

Queue

```text
[(2,1)]
```

Map

```text
{
0:0,

2:1
}
```

---

Pop

```text
2
```

Neighbor

```text
3
```

Distance

```text
2
```

Queue

```text
[(3,2)]
```

Map

```text
{
0:0,

2:1,

3:2
}
```

---

Pop

```text
3
```

Neighbor

```text
-1
```

Stop.

Final

```text
node1Map

0 →0

2 →1

3 →2
```

---

## BFS from node2

Queue

```text
[(1,0)]
```

Map

```text
{
1:0
}
```

---

Pop

```text
1
```

Neighbor

```text
2
```

Distance

```text
1
```

Queue

```text
[(2,1)]
```

Map

```text
{
1:0,

2:1
}
```

---

Pop

```text
2
```

Neighbor

```text
3
```

Distance

```text
2
```

Queue

```text
[(3,2)]
```

Map

```text
{
1:0,

2:1,

3:2
}
```

Stop.

---

Now compare every node.

Node 0

```text
Only in node1Map

Ignore
```

---

Node 1

```text
Only in node2Map

Ignore
```

---

Node 2

```text
distance1 =1

distance2 =1

max=1
```

Current answer

```text
2
```

---

Node 3

```text
distance1 =2

distance2 =2

max=2
```

Current minimum

```text
1
```

No improvement.

Answer

```text
2
```

---

# Another Example

Input

```python
edges = [1,2,-1]

node1 = 0

node2 = 2
```

Graph

```text
0►1►2
```

Distances

Friend A

```text
0

1

2
```

Distances

```text
0

1

2
```

Friend B

```text
2
```

Distances

```text
0
```

Common node

```text
2
```

Maximum

```text
max(2,0)=2
```

Answer

```text
2
```

---

# Why Does This Algorithm Work?

Suppose

```text
dist1[node]
```

is the shortest distance from

```text
node1
```

and

```text
dist2[node]
```

is the shortest distance from

```text
node2
```

If a node is reachable from both,

its meeting cost is

```text
max(dist1,node,
    dist2,node)
```

Why maximum?

Because both people must arrive.

The slower person determines the meeting time.

Therefore,

meeting cost

```text
=

maximum distance
```

We simply choose

```text
minimum

of

maximums
```

This is a classic

```text
Min-Max Optimization
```

---

# Time Complexity

Each BFS visits every node at most once.

Since each node has only one outgoing edge,

Time

```text
O(n)
```

Two BFS

```text
O(2n)
```

Comparison

```text
O(n)
```

Overall

```text
O(n)
```

Space

```text
O(n)
```

---

# Your Solution Explained

```python
def bfs(src, nodeMap):
```

Runs BFS from one source.

---

Queue

```python
q = deque([[src,0]])
```

Stores

```text
(node,distance)
```

---

Visited map

```python
nodeMap[src] = 0
```

Stores shortest distance.

---

Move to neighbor

```python
nei = edges[node]
```

Remember

There is only one neighbor.

---

Visit if unvisited

```python
if nei != -1 and nei not in nodeMap:
```

Update distance

```python
nodeMap[nei] = dist+1
```

Continue.

---

Finally

```python
for i in range(n):
```

Check every node.

If

```python
i in node1Map

and

i in node2Map
```

Calculate

```python
max(node1Map[i],
    node2Map[i])
```

Keep the minimum.

Exactly matches the problem statement.

---

# Python Solution

```python
class Solution:
    def closestMeetingNode(self, edges: List[int], node1: int, node2: int) -> int:

        n = len(edges)
        def bfs(src, nodeMap):
            q = deque([[src, 0]])
            nodeMap[src] = 0
            while q:
                node, dist = q.popleft()
                nei = edges[node]
                if nei != -1 and nei not in nodeMap:
                    nodeMap[nei] = dist + 1
                    q.append([nei, dist + 1])
        
        node1Map = {}
        node2Map = {}

        bfs(node1, node1Map)
        bfs(node2, node2Map)

        res = -1
        minDist = float('inf')

        for i in range(n):
            if i in node1Map and i in node2Map:
                maxDist = max(node1Map[i], node2Map[i])
                if maxDist < minDist:
                    minDist = maxDist
                    res = i
        return res
```

---

# Even Simpler Observation (No Queue Needed)

Because every node has **at most one outgoing edge**, there is never a branching choice.

From any starting node, there is only one path to follow.

So instead of BFS, you can simply keep walking until:

- you reach `-1`, or
- you revisit a node (cycle).

```python
def getDistance(start):
    dist = {}
    d = 0

    while start != -1 and start not in dist:
        dist[start] = d
        d += 1
        start = edges[start]

    return dist
```

This is still **O(n)** and is arguably even more intuitive for this special graph.

---

# How to Build the Intuition

Whenever you solve graph problems, ask these questions.

## Step 1

Is it a graph?

```text
Nodes

Edges

Connections
```

↓

Graph.

---

## Step 2

Directed or Undirected?

Here

```text
A → B
```

Only one direction.

↓

Directed Graph.

---

## Step 3

Weighted or Unweighted?

Every edge costs

```text
1
```

↓

Unweighted Graph.

---

## Step 4

Need shortest distance?

Words like

```text
Closest

Nearest

Minimum distance

Reach
```

↓

Think Shortest Path.

---

## Step 5

How many sources?

Two.

↓

Compute distances from both.

---

## Step 6

How do we compare?

Problem explicitly says

```text
minimize

max(distance1,
    distance2)
```

So compute

```python
max(dist1[node], dist2[node])
```

for every common node and choose the smallest.

---

# Mental Checklist

```text
Graph?
      ↓
Directed?
      ↓
Weighted?
      ↓
No (all weights = 1)
      ↓
Shortest Path
      ↓
BFS (or simple walk because out-degree ≤ 1)
      ↓
Distance from node1
+
Distance from node2
      ↓
Common nodes
      ↓
Compute max(distance1, distance2)
      ↓
Choose minimum
      ↓
If tie → smallest index
```

---

# Key Takeaways

- This is a **shortest path** problem on a **special directed graph**.
- Since each node has **at most one outgoing edge**, every start node follows a **single chain**.
- Compute the shortest distance from both starting nodes independently.
- Compare only the nodes reachable from both.
- For each common node, the meeting cost is `max(distance1, distance2)`.
- Return the node with the **minimum meeting cost**, breaking ties by choosing the **smallest index**.
- Recognizing the graph's structure (out-degree ≤ 1) allows an even simpler implementation than general BFS.