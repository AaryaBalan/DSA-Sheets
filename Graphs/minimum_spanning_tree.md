# Minimum Spanning Tree (MST)

> **Graph Topic:** Minimum Spanning Tree (MST)  
> **Prerequisites:** Graph Representation, Trees, Connected Graphs  
> **Algorithms Used:** Prim's Algorithm, Kruskal's Algorithm

---

# 📑 Table of Contents

## Concepts
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [What is a Spanning Tree?](#what-is-a-spanning-tree)
- [Properties of a Spanning Tree](#properties-of-a-spanning-tree)
- [Example of a Spanning Tree](#example-of-a-spanning-tree)
- [What is a Minimum Spanning Tree (MST)?](#what-is-a-minimum-spanning-tree-mst)
- [Difference Between Spanning Tree and MST](#difference-between-spanning-tree-and-mst)
- [Applications of MST](#applications-of-mst)
- [Prim's Algorithm](#1-prims-algorithm)
- [Kruskal's Algorithm](#2-kruskals-algorithm)
- [Pattern Recognition](#pattern-recognition)
- [Time Complexity](#time-complexity)
- [Revision Cheat Sheet](#revision-cheat-sheet)
- [Key Takeaways](#key-takeaways)

<br/><br/>

## Problems
- [1584. Min Cost to Connect All Points](#1584-min-cost-to-connect-all-points)

---

# Introduction

Many real-world problems involve connecting different locations while spending the **least possible cost**.

Examples include:

- Connecting cities using roads.
- Laying internet cables between buildings.
- Connecting computers in a network.
- Supplying electricity to different villages.

In all these problems, we want to:

- Connect every location.
- Avoid unnecessary connections.
- Minimize the total cost.

This is exactly what a **Minimum Spanning Tree (MST)** does.

---

# What is a Spanning Tree?

Before learning Minimum Spanning Tree, we must first understand **Spanning Tree**.

Suppose you are given a **connected, undirected weighted graph**.

Example

```
       2
   0 ------- 1
   |\       /|
 6 | \5   3/ |7
   |  \   /  |
   |   \ /   |
   3----2----4
       8
```

This graph contains

```
Vertices = 5

Edges = 6
```

Our goal is to create a **tree** using these vertices.

---

# Definition of a Spanning Tree

A **Spanning Tree** is a tree that satisfies the following conditions:

- It contains **all the vertices** of the graph.
- It contains exactly **N - 1 edges**, where **N** is the number of vertices.
- Every vertex is reachable from every other vertex.
- It contains **no cycles**.

These four conditions are mandatory.

---

# Why Exactly N - 1 Edges?

Suppose we have

```
5 vertices
```

A tree with

```
5 vertices
```

must always have

```
4 edges
```

because

```
Edges = Vertices - 1
```

If we add one more edge,

a cycle will be formed.

If we remove one edge,

the graph becomes disconnected.

Therefore,

a tree always contains exactly

```
N - 1
```

edges.

---

# Properties of a Spanning Tree

A valid spanning tree must satisfy:

- Contains every vertex.
- Contains exactly `N - 1` edges.
- Every node is connected.
- No cycles.

---

# Example of a Spanning Tree

Suppose we select the following edges

```
0 ----- 1

|

|

4

|

|

2 ----- 3
```

Vertices

```
0
1
2
3
4
```

Edges

```
4
```

Since

```
Vertices = 5

Edges = 4
```

and every node is connected,

this is a **Spanning Tree**.

---

# Another Spanning Tree

We could also choose different edges.

```
1

|

|

3

|

|

0

|

|

2

|

|

4
```

Again,

```
Vertices = 5

Edges = 4
```

Still a valid spanning tree.

---

# Important Observation

A graph can have **multiple spanning trees**.

There is **no fixed number**.

Some graphs may have

```
1
```

spanning tree.

Others may have

```
5
```

or

```
100
```

or even thousands.

The number of spanning trees depends entirely on the graph.

---

# Weighted Graph

Until now,

we ignored edge weights.

Now suppose every edge has a cost.

Example

```
0 ----6----1

|

2

|

2----5----3

 \

  3

   \

    4
```

Each edge has a weight.

Our objective now changes.

Instead of selecting **any** spanning tree,

we want the one whose **total edge weight is minimum**.

---

# What is a Minimum Spanning Tree (MST)?

A **Minimum Spanning Tree (MST)** is simply a spanning tree whose total edge weight is the smallest among all possible spanning trees.

In simple words,

> Connect all the vertices using exactly **N−1 edges** while keeping the total cost as small as possible.

---

# Example

Suppose one spanning tree has edge weights

```
6

2

5

3
```

Total

```
6 + 2 + 5 + 3

=

16
```

Another spanning tree has

```
2

8

3

7
```

Total

```
20
```

Since

```
16 < 20
```

The first spanning tree is the **Minimum Spanning Tree**.

---

# Difference Between Spanning Tree and MST

| Spanning Tree | Minimum Spanning Tree |
|---------------|-----------------------|
| Any valid tree | Tree with minimum total weight |
| May not have minimum cost | Has the minimum possible cost |
| Can be many | One or more (if minimum cost is the same) |

---

# Can There Be Multiple MSTs?

Yes.

If multiple spanning trees have the **same minimum total weight**,

all of them are considered valid **Minimum Spanning Trees**.

The important condition is

```
Minimum Total Cost
```

not

```
Unique Tree
```

---

# Real-Life Analogy

Imagine five villages.

You need to connect them using roads.

Every road costs money.

Possible options:

```
Road Plan A

Cost = ₹16 Lakhs
```

```
Road Plan B

Cost = ₹20 Lakhs
```

```
Road Plan C

Cost = ₹18 Lakhs
```

Which one would you choose?

Obviously,

```
Road Plan A
```

because it connects every village while spending the least money.

That is exactly what an **MST** does.

---

# Example Problem

Consider the graph

```
        1
    1 ------- 4
    |\
  2 | \5
    |  \
    2---3
      3
       \
        5
         \
          6
```

Choose edges

```
1

2

3

4

7
```

Total

```
1 + 2 + 3 + 4 + 7

=

17
```

This is one possible Minimum Spanning Tree.

---

# How Do We Find an MST?

Instead of checking every possible spanning tree,

we use efficient algorithms.

The two most popular algorithms are:

## 1. Prim's Algorithm

- Starts from any node.
- Expands the tree one edge at a time.
- Always picks the minimum-weight edge connecting a new vertex.

Time Complexity

```
O(E log V)
```

---

## 2. Kruskal's Algorithm

- Sorts all edges by weight.
- Adds the smallest edge first.
- Avoids cycles using the **Disjoint Set (Union-Find)** data structure.

Time Complexity

```
O(E log E)
```

---

# Applications of MST

Minimum Spanning Trees are widely used in:

- Network Design
- Road Construction
- Electrical Wiring
- Water Pipeline Distribution
- Internet Cable Routing
- Railway Planning
- Computer Network Optimization
- Cluster Analysis

---

# Pattern Recognition

Whenever a problem says:

- Connect all vertices.
- Minimum cost.
- No unnecessary edges.
- No cycles.
- Network design.

Immediately think

```
Minimum Spanning Tree (MST)
```

Then decide whether to use

```
Prim's Algorithm
```

or

```
Kruskal's Algorithm
```

---

# Time Complexity

Finding an MST depends on the algorithm used.

| Algorithm | Time Complexity |
|-----------|-----------------|
| Prim's Algorithm | O(E log V) |
| Kruskal's Algorithm | O(E log E) |

---

# Revision Cheat Sheet

```
Connected Graph

↓

Need a Tree

↓

Tree = N Vertices + (N-1) Edges

↓

Many Spanning Trees Possible

↓

Choose One With Minimum Weight

↓

Minimum Spanning Tree (MST)
```

---

# Key Takeaways

- A **Spanning Tree** connects all vertices without forming any cycles.
- Every spanning tree contains exactly **N − 1 edges**.
- A graph can have multiple spanning trees.
- A **Minimum Spanning Tree (MST)** is the spanning tree with the smallest total edge weight.
- Multiple MSTs can exist if they all have the same minimum cost.
- The two standard algorithms to find an MST are:
  - **Prim's Algorithm**
  - **Kruskal's Algorithm**
- MST is commonly used in network design, transportation, wiring, and infrastructure planning.

<br/><br/><br/><br/><br/>

---

# Prim's Algorithm (Indepth)

> **Graph Algorithms → Minimum Spanning Tree (MST)**  
> Prim's Algorithm is a greedy algorithm used to find the **Minimum Spanning Tree (MST)** of a connected, weighted, undirected graph. 
---

# 📖 Introduction

Prim's Algorithm builds the **Minimum Spanning Tree (MST)** by starting from any node and repeatedly adding the **smallest edge** that connects a new vertex to the current tree. It follows a **Greedy** approach. :contentReference[oaicite:1]{index=1}

---

# 📚 Prerequisites

- Graph Representation
- Priority Queue (Min Heap)
- Minimum Spanning Tree (MST)

---

# 💡 Idea

Start from any node.

At every step:

- Pick the **minimum-weight edge**.
- If its destination is already in the MST, ignore it.
- Otherwise:
  - Add the edge to the MST.
  - Add its weight to the answer.
  - Push all adjacent edges into the priority queue.

Continue until every node is included in the MST. :contentReference[oaicite:2]{index=2}

---

# ⚙️ Algorithm

1. Create a **Min Heap** storing `(weight, node)`.
2. Create a `visited` array.
3. Push the starting node as `(0,0)`.
4. While the heap is not empty:
   - Pop the minimum edge.
   - If node is already visited, continue.
   - Mark it visited.
   - Add its weight to the answer.
   - Push all unvisited neighbors into the heap.
5. Return the total weight.

---

# 📝 Dry Run

Graph

```
0 --2-- 1
|      /|
1     3 |
|   /   |
2 --2-- 3
 \
 1
  \
   4
```

Start

```
Heap = [(0,0)]
```

Visit

```
0
```

Push

```
(2,1)
(1,2)
```

Pick minimum

```
(1,2)
```

Visit 2

Push

```
(1,1)
(2,3)
(2,4)
```

Next minimum

```
(1,1)
```

Visit 1

Next minimum

```
(2,3)
```

Visit 3

Next minimum

```
(1,4)
```

Visit 4

All vertices are visited.

MST is complete. :contentReference[oaicite:3]{index=3}

---

# 🐍 Python Code

```python
import heapq
from collections import defaultdict

def prim(n, edges):

    graph = defaultdict(list)

    for u, v, w in edges:
        graph[u].append((v, w))
        graph[v].append((u, w))

    visited = [False] * n
    pq = [(0, 0)]          # (weight, node)
    mst_weight = 0

    while pq:

        weight, node = heapq.heappop(pq)

        if visited[node]:
            continue

        visited[node] = True
        mst_weight += weight

        for nei, wt in graph[node]:
            if not visited[nei]:
                heapq.heappush(pq, (wt, nei))

    return mst_weight
```

---

# ⏱ Complexity

| Operation | Complexity |
|-----------|------------|
| Building Graph | O(E) |
| Heap Operations | O(E log E) |
| Overall | **O(E log E)** |
| Space | **O(V + E)** |

*(As explained in the source, each edge may be pushed into the priority queue, leading to approximately `O(E log E)` time.)* :contentReference[oaicite:4]{index=4}

---

# 🎯 Pattern Recognition

Use **Prim's Algorithm** when the problem says:

- Minimum Spanning Tree
- Connect all nodes
- Minimum total cost
- Weighted Undirected Graph
- No cycles

---

# 📌 Key Takeaways

- Prim's Algorithm is a **Greedy Algorithm**.
- It grows the MST one vertex at a time.
- Always picks the **minimum available edge**.
- Uses a **Priority Queue (Min Heap)**.
- Skip nodes that are already visited.
- Time Complexity: **O(E log E)** (as presented in the source). :contentReference[oaicite:5]{index=5}

<br/><br/><br/>

# 🌳 Kruskal's Algorithm

> **Graph Algorithms → Minimum Spanning Tree (MST)**

Kruskal's Algorithm is a **Greedy Algorithm** used to find the **Minimum Spanning Tree (MST)** of a **connected, weighted, undirected graph**. It works by always selecting the **smallest available edge** while ensuring that **no cycle is formed**. To efficiently detect cycles, it uses the **Disjoint Set Union (DSU)** data structure. :contentReference[oaicite:0]{index=0}

---

# 📖 Introduction

Suppose you are given a connected weighted graph.

Your task is to connect **all the vertices** such that

- Every node is reachable.
- No cycles are formed.
- The total weight is minimum.

This is exactly the **Minimum Spanning Tree (MST)** problem.

Previously, we solved this using **Prim's Algorithm**.

Now we will solve the same problem using **Kruskal's Algorithm**.

Unlike Prim's Algorithm, which grows one tree from a starting node, **Kruskal's Algorithm starts with no edges and keeps adding the smallest edge one by one until the MST is complete.** :contentReference[oaicite:1]{index=1}

---

# 📚 Prerequisites

Before learning Kruskal's Algorithm, you should know:

- Graph Representation
- Minimum Spanning Tree (MST)
- Disjoint Set Union (DSU)

---

# 💡 Intuition

Imagine every vertex is standing alone.

Initially

```
1

2

3

4

5

6
```

No node is connected.

Now start picking the **smallest edge**.

Every time you choose an edge,

ask only one question:

> "Will adding this edge create a cycle?"

If

```
NO
```

Take the edge.

If

```
YES
```

Ignore the edge.

Continue until

```
Number of Edges = N - 1
```

That becomes the Minimum Spanning Tree.

---

# ❓ Why Do We Need DSU?

Suppose we already selected

```
1 -----2

|

|

3
```

Now suppose another edge arrives

```
2 -----3
```

Should we take it?

No.

Because

```
1

↓

2

↓

3
```

already connects them.

Adding

```
2-----3
```

creates a cycle.

How do we quickly detect this?

We use **Disjoint Set Union (DSU)**.

DSU tells us

```
Do these two vertices already belong to the same connected component?
```

If yes,

skip the edge.

Otherwise,

take it.

---

# 🌳 Main Idea

Kruskal's Algorithm is based on two simple steps.

## Step 1

Sort all edges according to their weight.

Smallest edge comes first.

Example

```
(1,4,1)

(1,2,2)

(2,3,3)

(2,4,4)

(1,5,5)

...
```

---

## Step 2

Process edges one by one.

For every edge

```
(u,v,w)
```

Check

```
find(u)

==

find(v)
```

If both have the same ultimate parent,

Ignore the edge.

Otherwise

- Add the edge to MST.
- Add its weight.
- Union both components.

Repeat until all edges are processed.

---

# 📝 Dry Run

Suppose we have the graph

```
1 ---2

|\

| \

4  3

|

5

|

6
```

Sorted edges

```
(1,4,1)

(1,2,2)

(2,3,3)

(2,4,4)

(1,5,5)

(3,4,6)

(2,6,7)
```

Initially

Every node is its own component.

```
1

2

3

4

5

6
```

---

## Edge (1,4)

Different components.

Take it.

```
Weight = 1
```

---

## Edge (1,2)

Different components.

Take it.

```
Weight = 1+2=3
```

---

## Edge (2,3)

Different components.

Take it.

```
Weight = 6
```

---

## Edge (2,4)

Now

```
2
```

and

```
4
```

already belong to the same component.

Adding this edge forms a cycle.

Skip it.

---

## Edge (1,5)

Different components.

Take it.

```
Weight = 11
```

---

## Edge (3,4)

Already connected.

Skip.

---

## Edge (2,6)

Different components.

Take it.

Final Weight

```
18
```

MST completed.

---

# ⚙️ Algorithm

1. Store all edges.
2. Sort edges by weight.
3. Create a Disjoint Set.
4. Traverse every edge.
5. If endpoints belong to different components:
   - Add weight.
   - Union both vertices.
6. Return the total MST weight.

---

# 🐍 Python Implementation

```python
class DisjointSet:

    def __init__(self, n):
        self.parent = [i for i in range(n)]
        self.size = [1] * n

    def find(self, node):

        if self.parent[node] != node:
            self.parent[node] = self.find(self.parent[node])

        return self.parent[node]

    def union(self, u, v):

        pu = self.find(u)
        pv = self.find(v)

        if pu == pv:
            return

        if self.size[pu] < self.size[pv]:

            self.parent[pu] = pv
            self.size[pv] += self.size[pu]

        else:

            self.parent[pv] = pu
            self.size[pu] += self.size[pv]


def kruskal(n, edges):

    edges.sort(key=lambda x: x[2])

    ds = DisjointSet(n)

    mst_weight = 0

    mst_edges = []

    for u, v, w in edges:

        if ds.find(u) != ds.find(v):

            ds.union(u, v)

            mst_weight += w

            mst_edges.append((u, v))

    return mst_weight, mst_edges
```

---

# 🔍 Code Explanation

## Step 1

Sort all edges.

```python
edges.sort(key=lambda x: x[2])
```

Edges are processed from the smallest weight to the largest.

---

## Step 2

Create DSU.

```python
ds = DisjointSet(n)
```

Initially,

every node is its own component.

---

## Step 3

Traverse every edge.

```python
for u,v,w in edges:
```

---

## Step 4

Check whether

```
u

and

v
```

already belong to the same component.

```python
if ds.find(u) != ds.find(v):
```

---

## Step 5

If not,

take the edge.

```python
mst_weight += w
```

---

## Step 6

Merge both components.

```python
ds.union(u,v)
```

Continue until every edge is processed.

---

# ⏱ Time Complexity

Sorting edges

```
O(E log E)
```

DSU operations

```
O(E × α(V))
```

where

```
α(V)
```

is the **Inverse Ackermann Function**, which is almost constant.

Overall Time Complexity

```
O(E log E)
```

Space Complexity

```
O(V + E)
```

---

# ⚖️ Prim's vs Kruskal's

| Prim's Algorithm | Kruskal's Algorithm |
|------------------|---------------------|
| Starts from one node | Starts from smallest edge |
| Uses Priority Queue | Uses Sorting |
| Grows one tree | Builds forest and merges components |
| Detects visited nodes | Detects cycles using DSU |
| Time: O(E log V) | Time: O(E log E) |

---

# 🎯 Pattern Recognition

Think of **Kruskal's Algorithm** whenever a problem says:

- Minimum Spanning Tree
- Weighted Undirected Graph
- Minimum Cost Network
- Avoid Cycles
- Connect all vertices
- Use Disjoint Set

---

# 📌 Revision Cheat Sheet

```
Weighted Graph

↓

Sort All Edges

↓

Take Smallest Edge

↓

Same Component?

↓

YES → Ignore

NO → Take Edge

↓

Union Components

↓

Repeat

↓

Minimum Spanning Tree
```

---

# ✅ Key Takeaways

- Kruskal's Algorithm is a **Greedy Algorithm**.
- Always process edges in **ascending order of weight**.
- Use **Disjoint Set (DSU)** to detect cycles efficiently.
- Add an edge only if it connects two different components.
- Ignore edges that create cycles.
- Continue until the MST contains **N − 1 edges**.
- Time Complexity: **O(E log E)**.
- Kruskal's Algorithm is one of the most common interview algorithms for Minimum Spanning Tree problems.

<br/><br/><br/><br/><br/>

---


# Problems

<br/>

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