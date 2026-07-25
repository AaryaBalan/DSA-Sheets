# Introduction

This is a beginner-friendly guide to shortest path problems in graphs. It explains how to solve shortest paths in a DAG using topological ordering and relaxation, why BFS works for unweighted graphs, and when Dijkstra is the right choice for weighted graphs with positive edge weights.

## Intro Links

- [Shortest Path in Directed Acyclic Graph](#shortest-path-in-directed-acyclic-graph-dag)
- [Shortest Path in Undirected Graph with Unit Weights](#shortest-path-in-undirected-graph-with-unit-weights)
- [Dijkstra's Algorithm (Priority Queue)](#dijkstras-algorithm-priority-queue)

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
