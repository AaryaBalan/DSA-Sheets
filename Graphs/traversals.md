# Graph BFS Traversal in Python
# Complete Beginner to Interview Guide

> **Difficulty:** Easy  
> **Pattern:** Graph Traversal + Queue (FIFO)

---

# What is Graph Traversal?

Traversal means

> Visiting every node in the graph.

There are two major ways:

1. Breadth First Search (BFS)
2. Depth First Search (DFS)

---

# What is BFS?

BFS stands for

> **Breadth First Search**

Instead of going deep into one branch,

BFS explores **level by level**.

Imagine water spreading from a source.

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

Traversal order

```text
A

↓

B C

↓

D E F
```

Result

```text
A B C D E F
```

Notice

It visits every node on the current level before moving to the next level.

---

# Real Life Analogy

Imagine throwing a stone into a pond.

```text
      ●
```

Ripples spread like

```text
      ●

   ○ ○ ○

 ○ ○ ○ ○ ○
```

The water reaches

- Nearby points first
- Farther points later

This is exactly how BFS works.

---

# Why Does BFS Use a Queue?

A Queue follows

> **FIFO (First In, First Out)**

Example

```text
Queue

Front               Rear

A B C
```

Remove

```text
A
```

Queue becomes

```text
B C
```

New node

```text
D
```

Added at the rear

```text
B C D
```

The oldest node is always processed first.

This perfectly matches BFS.

---

# Data Structures Used in BFS

We need

## 1. Queue

Stores nodes waiting to be explored.

```python
from collections import deque

queue = deque()
```

---

## 2. Visited Set

Avoids visiting the same node again.

```python
visited = set()
```

---

## 3. Adjacency List

Represents the graph.

```python
graph = {
    "A": ["B", "C"],
    "B": ["A", "D", "E"],
    "C": ["A", "F"],
    "D": ["B"],
    "E": ["B"],
    "F": ["C"]
}
```

---

# How BFS Works Behind the Scenes

Consider

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

Initially

Queue

```text
A
```

Visited

```text
A
```

---

## Step 1

Remove

```text
A
```

Output

```text
A
```

Neighbors

```text
B
C
```

Add both

Queue

```text
B C
```

Visited

```text
A B C
```

---

## Step 2

Remove

```text
B
```

Output

```text
A B
```

Neighbors

```text
A
D
E
```

A already visited.

Add

```text
D
E
```

Queue

```text
C D E
```

Visited

```text
A B C D E
```

---

## Step 3

Remove

```text
C
```

Output

```text
A B C
```

Neighbor

```text
F
```

Queue

```text
D E F
```

Visited

```text
A B C D E F
```

---

## Step 4

Remove

```text
D
```

No new neighbors.

Queue

```text
E F
```

---

## Step 5

Remove

```text
E
```

Queue

```text
F
```

---

## Step 6

Remove

```text
F
```

Queue

```text
Empty
```

Traversal Complete

---

# Dry Run

Graph

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

Adjacency List

```python
graph = {
    "A": ["B", "C"],
    "B": ["A", "D", "E"],
    "C": ["A", "F"],
    "D": ["B"],
    "E": ["B"],
    "F": ["C"]
}
```

Start Node

```
A
```

---

## Initial State

Queue

```text
[A]
```

Visited

```text
{A}
```

Output

```text
[]
```

---

## Iteration 1

Pop

```
A
```

Output

```text
[A]
```

Push

```
B
C
```

Queue

```text
[B, C]
```

---

## Iteration 2

Pop

```
B
```

Output

```text
[A, B]
```

Neighbors

```
A D E
```

Skip

```
A
```

Push

```
D E
```

Queue

```text
[C, D, E]
```

---

## Iteration 3

Pop

```
C
```

Output

```text
[A, B, C]
```

Push

```
F
```

Queue

```text
[D, E, F]
```

---

## Iteration 4

Pop

```
D
```

Queue

```text
[E, F]
```

---

## Iteration 5

Pop

```
E
```

Queue

```text
[F]
```

---

## Iteration 6

Pop

```
F
```

Queue

```text
[]
```

Final Traversal

```text
A B C D E F
```

---

# BFS Algorithm

```text
Create Queue

Create Visited Set

Insert Starting Node

While Queue is not Empty

    Remove Front Node

    Visit It

    Visit Every Neighbor

        If Not Visited

            Mark Visited

            Push into Queue
```

---

# Python Implementation

```python
from collections import deque


def bfs(graph, start):

    visited = set()
    queue = deque()
    traversal = []

    # Start from the source node
    visited.add(start)
    queue.append(start)

    while queue:

        # Remove the front node
        node = queue.popleft()

        # Visit the node
        traversal.append(node)

        # Visit all neighbors
        for neighbor in graph[node]:

            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

    return traversal


graph = {
    "A": ["B", "C"],
    "B": ["A", "D", "E"],
    "C": ["A", "F"],
    "D": ["B"],
    "E": ["B"],
    "F": ["C"]
}

print(bfs(graph, "A"))
```

Output

```text
['A', 'B', 'C', 'D', 'E', 'F']
```

---

# Code Explanation

## Step 1

```python
visited = set()
```

Stores all visited nodes.

Without this,

the algorithm may revisit nodes forever.

---

## Step 2

```python
queue = deque()
```

Creates a queue.

Queue stores nodes waiting to be explored.

---

## Step 3

```python
visited.add(start)
queue.append(start)
```

Insert the starting node.

Initial

Queue

```text
[A]
```

---

## Step 4

```python
while queue:
```

Continue until every reachable node has been processed.

---

## Step 5

```python
node = queue.popleft()
```

Remove the front node.

FIFO

```text
A B C

↓

A removed

↓

B C
```

---

## Step 6

```python
traversal.append(node)
```

Visit the node.

---

## Step 7

```python
for neighbor in graph[node]:
```

Explore every neighbor.

---

## Step 8

```python
if neighbor not in visited:
```

Only visit unvisited nodes.

This prevents infinite loops in cyclic graphs.

---

## Step 9

```python
visited.add(neighbor)
queue.append(neighbor)
```

Mark first.

Then enqueue.

This ensures each node enters the queue only once.

---

# Why Do We Mark Visited Before Enqueuing?

Correct

```python
visited.add(neighbor)
queue.append(neighbor)
```

Suppose

```text
A

↓

B

↓

C

↑

A
```

If we enqueue before marking,

multiple parents could enqueue the same node.

Marking immediately guarantees uniqueness.

---

# Time Complexity

Each node is visited once.

```text
O(V)
```

Each edge is explored once.

```text
O(E)
```

Overall

```text
O(V + E)
```

---

# Space Complexity

Visited Set

```text
O(V)
```

Queue

Worst case

```text
O(V)
```

Overall

```text
O(V)
```

---

# Applications of BFS

BFS is commonly used for:

- Shortest path in an unweighted graph
- Level-order traversal in trees
- Finding connected components
- Social network friend suggestions
- Web crawling
- GPS navigation (unweighted roads)
- Network broadcasting
- Puzzle solving (Word Ladder, Open Lock)

---

# Common Mistakes

## Forgetting the Visited Set

Wrong

```python
queue.append(neighbor)
```

Without checking `visited`, cyclic graphs can cause infinite loops.

---

## Using a List Instead of deque

Wrong

```python
queue.pop(0)
```

Time Complexity

```text
O(n)
```

Correct

```python
queue.popleft()
```

Time Complexity

```text
O(1)
```

---

## Marking Visited Too Late

Wrong

```python
queue.append(neighbor)

visited.add(neighbor)
```

A node could be added multiple times before being marked.

Correct

```python
visited.add(neighbor)
queue.append(neighbor)
```

---

# Interview Tips

When you hear:

- Shortest path in an unweighted graph
- Minimum number of moves
- Level by level traversal
- Distance from a source
- Nearest node

Think immediately:

```text
BFS
```

---

# Mental Model

```text
Start Node
     │
     ▼
Insert into Queue
     │
     ▼
Remove Front Node
     │
     ▼
Visit It
     │
     ▼
Explore All Neighbors
     │
     ▼
Unvisited Neighbor?
     │
     ├── No → Ignore
     │
     ▼
Mark Visited
     │
     ▼
Push into Queue
     │
     ▼
Repeat Until Queue is Empty
```

---

# Key Takeaways

- BFS explores a graph **level by level**.
- It always uses a **Queue (FIFO)**.
- Maintain a **Visited Set** to avoid revisiting nodes.
- Mark nodes as visited **before enqueuing** them.
- Time Complexity is **O(V + E)**.
- Space Complexity is **O(V)**.
- BFS is the standard algorithm for finding the **shortest path in an unweighted graph**.

---

# Mental Formula

```text
Need to Traverse a Graph?
           │
           ▼
Need Level-by-Level Traversal?
           │
           ▼
Need Shortest Path (Unweighted)?
           │
           ▼
Use Queue (FIFO)
           │
           ▼
Visited Set
           │
           ▼
Pop Front
           │
           ▼
Visit Node
           │
           ▼
Push Unvisited Neighbors
           │
           ▼
Repeat Until Queue is Empty
```

<br/><br/><br/>

---

# Graph DFS Traversal in Python
# Complete Beginner to Interview Guide

> **Difficulty:** Easy  
> **Pattern:** Graph Traversal + Recursion (Call Stack) / Stack (LIFO)

---

# Before Learning DFS

Before writing any code, understand **how DFS thinks**.

Suppose we have the following graph.

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

Adjacency List

```python
graph = {
    "A": ["B", "C"],
    "B": ["A", "D", "E"],
    "C": ["A", "F"],
    "D": ["B"],
    "E": ["B"],
    "F": ["C"]
}
```

---

# What is DFS?

DFS stands for

> **Depth First Search**

Instead of exploring level by level,

DFS keeps moving **as deep as possible** before coming back.

Imagine walking inside a maze.

Whenever you see a path,

you keep walking until

- you reach the end, or
- you cannot move further.

Only then do you return to explore another path.

---

# BFS vs DFS

Graph

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

### BFS

Visits level by level.

```text
A

↓

B C

↓

D E F
```

Traversal

```text
A B C D E F
```

---

### DFS

Keeps going deeper.

```text
A

↓

B

↓

D

↓

Back

↓

E

↓

Back

↓

Back

↓

C

↓

F
```

Traversal

```text
A B D E C F
```

Notice

DFS explores one complete branch before moving to another.

---

# Why Does DFS Use a Stack?

DFS follows

> **LIFO (Last In, First Out)**

Example

Stack

```text
Top

C
B
A
```

Remove

```text
C
```

Stack becomes

```text
Top

B
A
```

The most recently added node is processed first.

This naturally creates the "go deep first" behavior.

---

# Two Ways to Implement DFS

There are two common implementations.

## 1. Recursive DFS

Uses Python's **Call Stack** automatically.

```python
dfs(A)
```

↓

calls

```python
dfs(B)
```

↓

calls

```python
dfs(D)
```

No explicit stack is needed.

---

## 2. Iterative DFS

Uses your own stack.

```python
stack = []
```

You manually push and pop nodes.

---

# How Recursion Works Behind the Scenes

This is the most important concept.

Suppose we call

```python
dfs("A")
```

Python creates a function frame.

```text
Stack

dfs(A)
```

Inside it,

we call

```python
dfs("B")
```

Now

```text
Stack

dfs(B)

dfs(A)
```

Inside B,

we call

```python
dfs("D")
```

Now

```text
Stack

dfs(D)

dfs(B)

dfs(A)
```

Notice

Python remembers every unfinished function.

This is called the **Call Stack**.

---

# What Happens at a Dead End?

Suppose

```text
D
```

has no unvisited neighbors.

Function

```python
dfs(D)
```

ends.

Python removes it.

Stack

```text
dfs(B)

dfs(A)
```

Execution continues exactly where it stopped inside

```python
dfs(B)
```

This process is called

> **Backtracking**

---

# DFS Step-by-Step

Graph

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

---

## Step 1

Visit

```text
A
```

Visited

```text
A
```

Go to

```text
B
```

---

## Step 2

Visit

```text
B
```

Visited

```text
A B
```

Go to

```text
D
```

---

## Step 3

Visit

```text
D
```

Visited

```text
A B D
```

No neighbors.

Backtrack.

---

## Step 4

Return to

```text
B
```

Next neighbor

```text
E
```

Visit

```text
E
```

Visited

```text
A B D E
```

No neighbors.

Backtrack.

---

## Step 5

Return to

```text
A
```

Next neighbor

```text
C
```

Visit

```text
C
```

Visited

```text
A B D E C
```

Go to

```text
F
```

---

## Step 6

Visit

```text
F
```

Visited

```text
A B D E C F
```

Done.

Traversal Complete.

---

# Complete Dry Run

Initial

Visited

```text
{}
```

Call

```python
dfs(A)
```

---

### Call Stack

```text
dfs(A)
```

Visit

```text
A
```

Visited

```text
A
```

---

Go to

```text
B
```

Stack

```text
dfs(B)

dfs(A)
```

Visited

```text
A B
```

---

Go to

```text
D
```

Stack

```text
dfs(D)

dfs(B)

dfs(A)
```

Visited

```text
A B D
```

No neighbors.

Return.

---

Stack

```text
dfs(B)

dfs(A)
```

Go to

```text
E
```

Stack

```text
dfs(E)

dfs(B)

dfs(A)
```

Visited

```text
A B D E
```

Return.

---

Stack

```text
dfs(B)

dfs(A)
```

Return.

---

Stack

```text
dfs(A)
```

Go to

```text
C
```

Stack

```text
dfs(C)

dfs(A)
```

Visited

```text
A B D E C
```

---

Go to

```text
F
```

Stack

```text
dfs(F)

dfs(C)

dfs(A)
```

Visited

```text
A B D E C F
```

Done.

Return.

Traversal

```text
A B D E C F
```

---

# Recursive DFS Algorithm

```text
Visit Current Node

Mark Visited

For Every Neighbor

    If Not Visited

        DFS(Neighbor)
```

---

# Recursive Python Solution

```python
def dfs(graph, node, visited, traversal):

    visited.add(node)
    traversal.append(node)

    for neighbor in graph[node]:

        if neighbor not in visited:
            dfs(graph, neighbor, visited, traversal)


graph = {
    "A": ["B", "C"],
    "B": ["A", "D", "E"],
    "C": ["A", "F"],
    "D": ["B"],
    "E": ["B"],
    "F": ["C"]
}

visited = set()
traversal = []

dfs(graph, "A", visited, traversal)

print(traversal)
```

Output

```text
['A', 'B', 'D', 'E', 'C', 'F']
```

---

# Code Explanation

## Step 1

```python
visited.add(node)
```

Mark the current node.

This prevents cycles.

---

## Step 2

```python
traversal.append(node)
```

Visit the node.

---

## Step 3

```python
for neighbor in graph[node]:
```

Look at every adjacent node.

---

## Step 4

```python
if neighbor not in visited:
```

Only visit nodes that have not been explored.

---

## Step 5

```python
dfs(graph, neighbor, visited, traversal)
```

Recursive call.

Python automatically pushes this function onto the call stack.

When it finishes,

execution returns to the previous function.

---

# Iterative DFS

Instead of recursion,

we can use our own stack.

---

## Algorithm

```text
Push Start Node

While Stack Not Empty

    Pop Top Node

    Visit It

    Push All Unvisited Neighbors
```

---

# Python Implementation

```python
def dfs(graph, start):

    visited = set()
    stack = [start]
    traversal = []

    while stack:

        node = stack.pop()

        if node in visited:
            continue

        visited.add(node)
        traversal.append(node)

        # Reverse keeps traversal similar to recursive DFS
        for neighbor in reversed(graph[node]):

            if neighbor not in visited:
                stack.append(neighbor)

    return traversal


graph = {
    "A": ["B", "C"],
    "B": ["A", "D", "E"],
    "C": ["A", "F"],
    "D": ["B"],
    "E": ["B"],
    "F": ["C"]
}

print(dfs(graph, "A"))
```

Output

```text
['A', 'B', 'D', 'E', 'C', 'F']
```

---

# Why Reverse the Neighbor List?

Stack is LIFO.

Suppose

```python
["B", "C"]
```

If we push

```text
B

then

C
```

Stack

```text
Top

C

B
```

C comes out first.

To preserve the recursive order,

we push

```python
reversed(graph[node])
```

so

```text
C

then

B
```

Stack

```text
Top

B

C
```

Now

```
B
```

is processed first,

matching recursive DFS.

---

# Time Complexity

Every vertex is visited once.

```text
O(V)
```

Every edge is explored once.

```text
O(E)
```

Overall

```text
O(V + E)
```

---

# Space Complexity

Visited Set

```text
O(V)
```

Call Stack (Recursive)

Worst case

```text
O(V)
```

Stack (Iterative)

Worst case

```text
O(V)
```

Overall

```text
O(V)
```

---

# Applications of DFS

DFS is widely used for:

- Detecting cycles
- Topological sorting
- Connected components
- Strongly connected components
- Maze solving
- Backtracking problems
- Graph coloring
- Finding bridges
- Finding articulation points

---

# Common Mistakes

## Forgetting the Visited Set

Wrong

```python
dfs(neighbor)
```

Without checking `visited`, DFS may recurse forever in cyclic graphs.

---

## Marking Visited Too Late

Wrong

```python
dfs(neighbor)

visited.add(neighbor)
```

Always mark before making the recursive call.

Correct

```python
visited.add(neighbor)
dfs(neighbor)
```

---

## Forgetting Base Condition

If you don't check

```python
if neighbor not in visited
```

the recursion never stops in graphs with cycles.

---

# BFS vs DFS

| Feature | BFS | DFS |
|---------|-----|-----|
| Data Structure | Queue | Stack / Recursion |
| Traversal | Level by Level | Go Deep First |
| Shortest Path (Unweighted) | ✅ Yes | ❌ No |
| Memory | Usually More | Usually Less |
| Backtracking | No | Yes |
| Recursive | Rare | Very Common |

---

# Interview Tips

Whenever you hear:

- Explore every possible path
- Backtracking
- Connected components
- Detect cycle
- Topological sort
- Islands problem
- Maze exploration

Think immediately:

```text
DFS
```

If you hear

- Minimum moves
- Shortest path (unweighted)
- Level order traversal

Think

```text
BFS
```

---

# Mental Model

```text
Start Node
     │
     ▼
Visit Current Node
     │
     ▼
Mark Visited
     │
     ▼
Choose First Unvisited Neighbor
     │
     ▼
Go Deeper
     │
     ▼
Dead End?
     │
     ├── No
     │      │
     │      ▼
     │   Continue Deeper
     │
     ▼
Yes
     │
     ▼
Backtrack
     │
     ▼
Explore Remaining Neighbors
     │
     ▼
Repeat Until Every Node Is Visited
```

---

# Key Takeaways

- DFS explores a graph **as deep as possible before backtracking**.
- It is naturally implemented using **recursion**, which relies on Python's **call stack**.
- An iterative version uses an explicit **stack (LIFO)**.
- Always maintain a **visited set** to avoid infinite loops in graphs with cycles.
- Mark nodes as visited **before** exploring their neighbors.
- Time Complexity is **O(V + E)**.
- Space Complexity is **O(V)**.
- DFS is the foundation for many advanced graph algorithms such as cycle detection, topological sorting, SCCs, bridges, articulation points, and backtracking problems.

---

# Mental Formula

```text
Need to Traverse a Graph?
           │
           ▼
Need to Explore Every Path?
           │
           ▼
Need Backtracking?
           │
           ▼
Use DFS
           │
           ▼
Visit Current Node
           │
           ▼
Mark Visited
           │
           ▼
Go to First Unvisited Neighbor
           │
           ▼
Can't Go Further?
           │
           ▼
Backtrack
           │
           ▼
Explore Remaining Neighbors
           │
           ▼
Repeat Until Every Node Is Visited
```