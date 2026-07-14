# Graph Representations in Python
## Complete Guide to Implementing Graphs

Graphs can be represented in multiple ways depending on the problem.

The three most common representations are:

1. **Edge List**
2. **Adjacency List**
3. **Adjacency Matrix**

Each representation has its own advantages, disadvantages, and use cases.

---

# Sample Graph

We'll use the same graph throughout the document.

## Undirected Graph

```text
      A
     / \
    B---C
     \
      D
```

Edges

```text
A-B
A-C
B-C
B-D
```

---

## Directed Graph

```text
A → B
↓
C → D
↑
B
```

Edges

```text
A → B
A → C
B → C
C → D
```

---

# 1. Edge List

## Idea

Store every edge as a pair of vertices.

Nothing more.

---

## Undirected Graph

Edges

```text
A-B
A-C
B-C
B-D
```

Python

```python
edges = [
    ("A", "B"),
    ("A", "C"),
    ("B", "C"),
    ("B", "D")
]
```

---

## Directed Graph

```text
A → B
A → C
B → C
C → D
```

Python

```python
edges = [
    ("A", "B"),
    ("A", "C"),
    ("B", "C"),
    ("C", "D")
]
```

---

## Traversing Edge List

```python
for u, v in edges:
    print(u, "->", v)
```

Output

```text
A -> B
A -> C
B -> C
B -> D
```

---

## Checking if an Edge Exists

```python
def edge_exists(edges, u, v):
    return (u, v) in edges
```

Time Complexity

```text
O(E)
```

---

## Advantages

- Very simple
- Easy to read
- Uses very little memory
- Perfect when only the list of edges is needed

---

## Disadvantages

- Slow edge lookup
- Slow neighbor traversal
- Not suitable for BFS or DFS

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Add Edge | O(1) |
| Remove Edge | O(E) |
| Search Edge | O(E) |
| Find Neighbors | O(E) |
| Space | O(E) |

---

# 2. Adjacency List

## Idea

Each node stores a list of all its neighbors.

This is the **most commonly used graph representation**.

---

# Undirected Graph

Graph

```text
A
|\
| \
B--C
|
D
```

Adjacency List

```text
A → B C

B → A C D

C → A B

D → B
```

Python

```python
graph = {
    "A": ["B", "C"],
    "B": ["A", "C", "D"],
    "C": ["A", "B"],
    "D": ["B"]
}
```

---

# Directed Graph

Graph

```text
A → B
↓

C → D
↑
B
```

Adjacency List

```text
A → B C

B → C

C → D

D → []
```

Python

```python
graph = {
    "A": ["B", "C"],
    "B": ["C"],
    "C": ["D"],
    "D": []
}
```

---

# Traversing Neighbors

```python
for neighbor in graph["A"]:
    print(neighbor)
```

Output

```text
B
C
```

---

# Adding an Edge

## Undirected

```python
graph[u].append(v)
graph[v].append(u)
```

---

## Directed

```python
graph[u].append(v)
```

---

# Removing an Edge

Undirected

```python
graph[u].remove(v)
graph[v].remove(u)
```

Directed

```python
graph[u].remove(v)
```

---

# Using defaultdict

Very common in interviews.

```python
from collections import defaultdict

graph = defaultdict(list)

graph["A"].append("B")
graph["A"].append("C")
graph["B"].append("C")
```

Output

```python
{
    "A": ["B", "C"],
    "B": ["C"]
}
```

---

# Advantages

- Fast neighbor traversal
- Easy to build
- Memory efficient
- Ideal for sparse graphs
- Preferred in BFS, DFS, Dijkstra, Topological Sort

---

# Disadvantages

- Edge lookup is slower than adjacency matrix
- Slightly more complex than edge list

---

# Time Complexity

| Operation | Complexity |
|-----------|------------|
| Add Edge | O(1) |
| Remove Edge | O(V) |
| Search Edge | O(V) (neighbor list) |
| Traverse Neighbors | O(Degree) |
| Space | O(V+E) |

---

# 3. Adjacency Matrix

## Idea

Store every possible edge in a matrix.

Rows represent the source node.

Columns represent the destination node.

---

Assume

```text
A = 0

B = 1

C = 2

D = 3
```

---

# Undirected Graph

Graph

```text
A-B

A-C

B-C

B-D
```

Matrix

```text
      A B C D

A     0 1 1 0

B     1 0 1 1

C     1 1 0 0

D     0 1 0 0
```

Python

```python
graph = [
    [0,1,1,0],
    [1,0,1,1],
    [1,1,0,0],
    [0,1,0,0]
]
```

Notice

```
Matrix is symmetric.
```

because

```
A-B

=

B-A
```

---

# Directed Graph

Edges

```text
A → B

A → C

B → C

C → D
```

Matrix

```text
      A B C D

A     0 1 1 0

B     0 0 1 0

C     0 0 0 1

D     0 0 0 0
```

Python

```python
graph = [
    [0,1,1,0],
    [0,0,1,0],
    [0,0,0,1],
    [0,0,0,0]
]
```

---

# Checking an Edge

```python
if graph[u][v] == 1:
    print("Edge Exists")
```

Time Complexity

```text
O(1)
```

---

# Adding an Edge

Undirected

```python
graph[u][v] = 1
graph[v][u] = 1
```

Directed

```python
graph[u][v] = 1
```

---

# Removing an Edge

Undirected

```python
graph[u][v] = 0
graph[v][u] = 0
```

Directed

```python
graph[u][v] = 0
```

---

# Advantages

- Fastest edge lookup
- Very easy implementation
- Good for dense graphs
- Perfect for algorithms like Floyd-Warshall

---

# Disadvantages

- Wastes memory
- Neighbor traversal takes O(V)
- Not suitable for sparse graphs

---

# Time Complexity

| Operation | Complexity |
|-----------|------------|
| Add Edge | O(1) |
| Remove Edge | O(1) |
| Search Edge | O(1) |
| Traverse Neighbors | O(V) |
| Space | O(V²) |

---

# Weighted Graphs

Edge weights can also be stored.

---

## Edge List

```python
edges = [
    ("A", "B", 4),
    ("A", "C", 2),
    ("B", "D", 5)
]
```

---

## Adjacency List

```python
graph = {
    "A": [("B",4), ("C",2)],
    "B": [("D",5)],
    "C": [],
    "D": []
}
```

---

## Adjacency Matrix

```python
INF = float("inf")

graph = [
    [0,4,2,INF],
    [INF,0,INF,5],
    [INF,INF,0,INF],
    [INF,INF,INF,0]
]
```

---

# Comparison Table

| Feature | Edge List | Adjacency List | Adjacency Matrix |
|----------|-----------|----------------|------------------|
| Space | O(E) | O(V+E) | O(V²) |
| Add Edge | O(1) | O(1) | O(1) |
| Remove Edge | O(E) | O(V) | O(1) |
| Search Edge | O(E) | O(V) | O(1) |
| Traverse Neighbors | O(E) | O(Degree) | O(V) |
| Sparse Graph | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐ |
| Dense Graph | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| BFS | ❌ | ✅ | ✅ |
| DFS | ❌ | ✅ | ✅ |
| Dijkstra | ❌ | ✅ | ⚠️ |
| Floyd-Warshall | ❌ | ❌ | ✅ |

---

# Which Representation Is Used in Interviews?

| Problem Type | Recommended Representation |
|--------------|----------------------------|
| BFS | Adjacency List |
| DFS | Adjacency List |
| Topological Sort | Adjacency List |
| Dijkstra | Adjacency List |
| Bellman-Ford | Edge List |
| Kruskal | Edge List |
| Prim | Adjacency List |
| Floyd-Warshall | Adjacency Matrix |
| Dense Graph | Adjacency Matrix |
| Sparse Graph | Adjacency List |

---

# Decision Flowchart

```text
Need to represent a graph?
          │
          ▼
Need fast edge lookup?
          │
     Yes ─────────────► Adjacency Matrix
          │
          No
          │
          ▼
Need BFS / DFS / Dijkstra?
          │
     Yes ─────────────► Adjacency List
          │
          No
          │
          ▼
Only storing edges?
          │
     Yes ─────────────► Edge List
```

---

# Interview Tips

- **Adjacency List** is the default choice for almost every graph problem.
- **Edge List** is mainly used when algorithms process edges directly (e.g., Kruskal's or Bellman-Ford).
- **Adjacency Matrix** is best when the graph is dense or when constant-time edge lookup is required.

---

# Key Takeaways

- **Edge List**
  - Simplest representation.
  - Best when only edges matter.
  - Poor for traversal.

- **Adjacency List**
  - Most common representation.
  - Memory efficient.
  - Best for BFS, DFS, shortest paths, and most interview problems.

- **Adjacency Matrix**
  - Fastest edge existence check.
  - Best for dense graphs.
  - Uses the most memory.

> **Rule of Thumb:**  
> If you're solving a graph problem in an interview and the interviewer doesn't specify the representation, start with an **Adjacency List**.