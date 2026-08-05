# 🔗 Disjoint Set Union (DSU) / Union-Find

> **Graph Algorithms → Disjoint Set (Union-Find)**

---

# 📖 Introduction

Disjoint Set (DSU), also called **Union-Find**, is a special data structure used to maintain **multiple connected components** efficiently.

Instead of repeatedly running **DFS** or **BFS** to determine whether two nodes belong to the same connected component, DSU stores this information in a clever way so that queries become extremely fast.

It is one of the most important data structures used in Graph Algorithms.

Some of its major applications include:

- Kruskal's Algorithm (Minimum Spanning Tree)
- Number of Connected Components
- Dynamic Connectivity Problems
- Cycle Detection
- Network Connectivity
- Accounts Merge
- Friend Circle Problems

---

# ❓ Why Do We Need DSU?

Suppose we have a graph

```
1 ----- 2 ----- 3 ----- 4

5 ----- 6 ----- 7
```

There are **two connected components**.

Component 1

```
1 2 3 4
```

Component 2

```
5 6 7
```

Now suppose someone repeatedly asks questions like

```
Are 2 and 4 connected?

Are 1 and 7 connected?

Are 5 and 6 connected?

Are 3 and 5 connected?
```

How would you answer?

---

## Brute Force Solution

One way is

Start DFS/BFS from the first node.

Example

```
Is 2 connected to 4?
```

Run DFS from

```
2
```

Traverse

```
2

↓

1

↓

3

↓

4
```

Found node 4.

Answer

```
Yes
```

Now another question

```
Is 3 connected to 7?
```

Again run DFS.

Again traverse.

Again search.

Every query requires

```
O(V+E)
```

time.

If there are thousands of such queries,

this becomes extremely slow.

---

# 💡 Dynamic Graph Intuition

Now imagine the graph keeps changing.

Initially

```
1 ---- 2

3 ---- 4
```

Later,

someone adds an edge

```
2 ---- 3
```

Now

```
1 ----2----3----4
```

Everything became connected.

Again someone asks

```
Are 1 and 4 connected?
```

Running DFS every time is inefficient.

Instead,

we want a data structure that **remembers** which nodes belong to the same connected component.

That is exactly why **Disjoint Set** was created.

---

# 🌳 What is a Disjoint Set?

A Disjoint Set is simply a collection of **separate groups**.

Initially,

every node is considered its own group.

Example

```
1

2

3

4

5
```

There are

```
5 disjoint sets
```

Each node is its own parent.

As connections are added,

these sets merge together.

---

# ⚙️ Operations Supported

A Disjoint Set mainly supports two operations.

## 1. Find Parent

Finds the **ultimate parent** of a node.

## 2. Union

Merges two different sets together.

Everything in DSU revolves around these two operations.

---

# 👑 Ultimate Parent

The most important idea in DSU is the **Ultimate Parent**.

Suppose we have

```
1

↓

2

↓

3

↓

4
```

Here

```
Parent(4) = 3

Parent(3) = 2

Parent(2) = 1

Parent(1) = 1
```

Notice

```
1
```

is its own parent.

Therefore

```
Ultimate Parent of 4 = 1
```

Similarly

Ultimate Parent of

```
3

=

1
```

Ultimate Parent of

```
2

=

1
```

Every node inside the same connected component shares the **same ultimate parent**.

---

# 📦 Parent Array

DSU stores parent information inside an array.

Initially

```
Node

0 1 2 3 4
```

Parent

```
0 1 2 3 4
```

Every node is its own parent.

After merging

```
0 -----1
```

Parent becomes

```
0 0 2 3 4
```

Now

```
Parent(1)=0
```

Later

```
1 -----2
```

Parent

```
0 0 0 3 4
```

Now

```
Ultimate Parent of

0

1

2

is

0
```

---

# 📊 Rank Array

Along with parent,

we maintain another array called

```
Rank
```

Initially

```
Rank

0 0 0 0 0
```

Rank approximately represents

```
Height of the tree
```

It helps us keep the tree balanced during Union operations.

(We will study this in detail in Part 2.)

---

# 📝 Example

Initially

```
1

2

3

4
```

Parent Array

```
1 2 3 4
```

Every node belongs to its own set.

Now merge

```
1

2
```

Now

```
1

↓

2
```

Both belong to the same component.

Merge

```
3

4
```

Now we have

```
1

↓

2


3

↓

4
```

Still two components.

Finally merge

```
2

3
```

Everything becomes

```
1

↓

2

↓

3

↓

4
```

Now every node has the same ultimate parent.

---

# 📌 Summary

- DSU stores **connected components**.
- It avoids repeatedly running DFS/BFS.
- Every connected component has one **ultimate parent**.
- DSU mainly supports:
  - Find Parent
  - Union
- Parent Array stores the parent of every node.
- Rank Array helps keep trees balanced.

<br/><br/><br/>

# 🌲 Union by Rank

> One of the two optimizations used in Disjoint Set.

---

# ❓ Why Normal Union is Bad?

Suppose we always connect nodes randomly.

Example

```
1

↓

2

↓

3

↓

4

↓

5

↓

6
```

Now suppose we want the ultimate parent of

```
6
```

We must travel

```
6

↓

5

↓

4

↓

3

↓

2

↓

1
```

This takes

```
O(N)
```

time.

As the tree becomes taller,

Find operation becomes slower.

We need a way to keep the tree **short**.

---

# 🌳 What is Rank?

Rank approximately represents the **height** of the tree.

Initially

Every node has

```
Rank = 0
```

Example

```
1

2

3
```

Ranks

```
0 0 0
```

As trees merge,

rank may increase.

The goal is to **avoid increasing the height unnecessarily**.

---

# 💡 Main Idea

Whenever two trees are merged,

always attach the **smaller rank tree**

below the

**larger rank tree**.

This keeps the overall height as small as possible.

---

# 📜 Rules

## Case 1

```
Rank(A)

<

Rank(B)
```

Attach

```
A

↓

B
```

Rank does not change.

---

## Case 2

```
Rank(A)

>

Rank(B)
```

Attach

```
B

↓

A
```

Rank does not change.

---

## Case 3

Both ranks are equal.

Example

```
Rank

1

1
```

Attach either one.

Increase the new parent's rank by

```
1
```

because the height has increased.

---

# 📝 Dry Run

Initially

```
1

2
```

Ranks

```
0

0
```

Union(1,2)

Choose

```
1
```

as parent.

Now

```
1

↓

2
```

Rank becomes

```
1

0
```

---

Now

```
3

4
```

Union

```
3

↓

4
```

Rank

```
1

0
```

---

Now merge

```
1

↓

2


3

↓

4
```

Both roots have

```
Rank = 1
```

Attach one below the other.

Result

```
1

↓

2

↓

3

↓

4
```

Increase root's rank

```
Rank = 2
```

---

# 🐍 Python Implementation

```python
class DisjointSet:

    def __init__(self, n):

        self.parent = [i for i in range(n + 1)]
        self.rank = [0] * (n + 1)

    def find(self, node):

        if self.parent[node] == node:
            return node

        return self.find(self.parent[node])

    def union(self, u, v):

        pu = self.find(u)
        pv = self.find(v)

        if pu == pv:
            return

        if self.rank[pu] < self.rank[pv]:

            self.parent[pu] = pv

        elif self.rank[pv] < self.rank[pu]:

            self.parent[pv] = pu

        else:

            self.parent[pv] = pu
            self.rank[pu] += 1
```

---

# 🔍 Code Explanation

### Step 1

Initially

```
parent[i] = i
```

Every node is its own parent.

---

### Step 2

Find returns the ultimate parent.

```python
find(node)
```

---

### Step 3

Union first finds

```
Ultimate Parent
```

of both nodes.

---

### Step 4

Compare ranks.

Smaller rank tree goes below larger rank tree.

---

### Step 5

If both ranks are equal,

choose either one as parent.

Increase its rank by

```
1
```

because only then does the height increase.

---

# ⏱ Complexity

Without Path Compression

```
Find

O(log N)
```

Union

```
O(log N)
```

After adding Path Compression (next topic),

both operations become

```
Almost O(1)
```

using the inverse Ackermann function.

---

# 📌 Summary

- Rank approximately represents tree height.
- Always attach the smaller-rank tree below the larger-rank tree.
- Equal ranks increase the parent's rank by 1.
- Union by Rank keeps the tree balanced.
- Balanced trees make Find operations much faster.
- Union by Rank alone gives about **O(log N)** operations; combining it with Path Compression makes DSU extremely efficient.

<br/><br/><br/>

# 🚀 Path Compression

> **Graph Algorithms → Disjoint Set (DSU)**

---

# 📖 Introduction

In **Part 2**, we learned **Union by Rank**.

Union by Rank keeps the tree balanced.

However, even after using Union by Rank, finding the ultimate parent may still require traversing several nodes.

To make the **Find()** operation even faster, we use another optimization called **Path Compression**.

Most interview problems use **Union by Rank (or Size) + Path Compression** together.

---

# ❓ Why Do We Need Path Compression?

Suppose our tree looks like this.

```
1
|
2
|
3
|
4
|
5
```

Now suppose we call

```python
find(5)
```

To find the ultimate parent, we must travel

```
5

↓

4

↓

3

↓

2

↓

1
```

This takes multiple recursive calls.

Now imagine calling

```python
find(5)
```

again.

It will again travel through all these nodes.

This repeated work is unnecessary.

Can we somehow remember the answer?

Yes.

That is exactly what **Path Compression** does.

---

# 💡 What is Path Compression?

Whenever we perform a **Find()** operation,

instead of only returning the ultimate parent,

we also make **every node visited during the search point directly to the ultimate parent**.

This makes future Find operations much faster.

---

# 🌳 Before Path Compression

Suppose

```
1
|
2
|
3
|
4
|
5
```

Ultimate parent of

```
5
```

is

```
1
```

But to reach it,

we travel through

```
5 → 4 → 3 → 2 → 1
```

---

# 🌳 After Path Compression

After calling

```python
find(5)
```

the tree becomes

```
      1
   / /|\ \
  2 3 4 5
```

Now

```
Parent(2)=1

Parent(3)=1

Parent(4)=1

Parent(5)=1
```

Every node now directly points to the root.

The next Find operation takes only one step.

---

# 🧠 Intuition

Think of asking for directions.

Without Path Compression:

Every time you ask,

people send you through five different streets.

With Path Compression:

The first person writes the destination on your map.

Next time,

you go directly there.

The first search does the work.

Future searches become almost free.

---

# 📝 Dry Run

Initially

```
Parent

1→1

2→1

3→2

4→3

5→4
```

Tree

```
1
|
2
|
3
|
4
|
5
```

Now call

```python
find(5)
```

Recursive calls

```
find(5)

↓

find(4)

↓

find(3)

↓

find(2)

↓

find(1)
```

Node

```
1
```

is the root.

Now recursion starts returning.

While returning,

update every parent.

```
Parent(5)=1

Parent(4)=1

Parent(3)=1

Parent(2)=1
```

Final tree

```
      1
   / /|\ \
  2 3 4 5
```

---

# 🔄 How Recursion Helps

The magic happens while recursion is **returning**.

The statement

```python
parent[node] = find(parent[node])
```

means

1. Go to the ultimate parent.
2. Store that parent directly.
3. Return it.

So one recursive call both

- Finds the root.
- Compresses the path.

---

# 🐍 Python Implementation

```python
class DisjointSet:

    def __init__(self, n):
        self.parent = [i for i in range(n + 1)]

    def find(self, node):

        if self.parent[node] == node:
            return node

        self.parent[node] = self.find(self.parent[node])

        return self.parent[node]
```

---

# 🔍 Code Explanation

## Base Case

```python
if self.parent[node] == node:
    return node
```

If a node is its own parent,

it is the root.

Return it.

---

## Recursive Step

```python
self.parent[node] = self.find(self.parent[node])
```

This line does two things.

First,

find the ultimate parent.

Second,

store that parent directly.

This is the Path Compression step.

---

## Return

```python
return self.parent[node]
```

Return the root.

---

# ⚡ Path Compression + Union by Rank

In practice,

we always use

```
Union by Rank

+

Path Compression
```

or

```
Union by Size

+

Path Compression
```

Both give nearly constant-time operations.

---

# ⏱ Time Complexity

Without Path Compression

```
Find

O(log N)
```

With Path Compression

```
Find

≈ O(α(N))
```

where

```
α(N)
```

is the **Inverse Ackermann Function**.

This function grows extremely slowly.

For all practical input sizes,

it behaves almost like

```
O(1)
```

---

# 📌 Summary

- Path Compression is an optimization for the **Find()** operation.
- Every visited node is connected directly to the ultimate parent.
- Future Find operations become much faster.
- It works naturally with recursion.
- Path Compression is almost always combined with **Union by Rank** or **Union by Size**.
- Final Time Complexity becomes **O(α(N))**, which is practically constant.

---

# 🎯 Key Takeaways

- **Union by Rank** keeps the tree balanced.
- **Path Compression** flattens the tree.
- Together, they make DSU one of the fastest data structures for connectivity problems.
- The line

```python
parent[node] = find(parent[node])
```

is the heart of Path Compression.

<br/><br/><br/>

# 📦 Union by Size

> **Graph Algorithms → Disjoint Set (DSU)**

---

# 📖 Introduction

In the previous chapter, we learned **Union by Rank**.

Rank approximately represents the **height** of the tree.

However,

there is another popular optimization called

> **Union by Size**

Instead of storing the **height** of each tree,

we store the **number of nodes** present in each connected component.

Most programmers actually prefer **Union by Size** because it is simpler to understand.

---

# ❓ Why Another Optimization?

Suppose we have two components.

Component A

```
1
|
2
|
3
```

Size

```
3
```

Component B

```
4
|
5
```

Size

```
2
```

If we merge these two,

which tree should become the parent?

Obviously,

the **larger tree** should remain the parent.

Why?

Because attaching a small tree below a large tree keeps the overall tree balanced.

That is the main idea behind **Union by Size**.

---

# 💡 What is Union by Size?

Every connected component stores

```
Size
```

which represents

```
Number of nodes
```

Initially,

every node forms its own component.

So

```
Size = 1
```

for every node.

Example

```
1

2

3

4
```

Size Array

```
1 1 1 1
```

---

# 🌳 Main Idea

Whenever we merge two components,

always attach

```
Smaller Component

↓

Larger Component
```

Then update the size.

Suppose

```
Size(A)=5

Size(B)=2
```

After merging

```
Size(A)=7
```

---

# 📜 Rules

## Case 1

```
Size(A)

<

Size(B)
```

Attach

```
A

↓

B
```

Update

```
Size(B)

+=

Size(A)
```

---

## Case 2

```
Size(A)

>

Size(B)
```

Attach

```
B

↓

A
```

Update

```
Size(A)

+=

Size(B)
```

---

## Case 3

Equal sizes

You can attach either one.

Just update the new size.

---

# 📝 Dry Run

Initially

```
1

2

3

4
```

Parent

```
1 2 3 4
```

Size

```
1 1 1 1
```

---

## Union(1,2)

Both sizes are

```
1
```

Attach

```
2

↓

1
```

Now

Tree

```
1
|
2
```

Size

```
2
```

---

## Union(3,4)

Tree

```
3
|
4
```

Size

```
2
```

---

## Union(2,3)

Ultimate Parent

```
1

Size=2
```

Ultimate Parent

```
3

Size=2
```

Sizes are equal.

Attach

```
3

↓

1
```

Final Tree

```
      1
     / \
    2   3
         |
         4
```

Update

```
Size(1)

=

4
```

Now all four nodes belong to the same component.

---

# 🐍 Python Implementation

```python
class DisjointSet:

    def __init__(self, n):

        self.parent = [i for i in range(n + 1)]
        self.size = [1] * (n + 1)

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
```

---

# 🔍 Code Explanation

## Step 1

Initially

```python
parent[i] = i
```

Every node is its own parent.

---

## Step 2

Initially

```python
size[i] = 1
```

Every component contains only one node.

---

## Step 3

Find the ultimate parents.

```python
pu = find(u)

pv = find(v)
```

---

## Step 4

If both belong to the same component

```python
if pu == pv:
    return
```

Nothing to merge.

---

## Step 5

Compare component sizes.

```python
if size[pu] < size[pv]
```

Attach the smaller component below the larger component.

---

## Step 6

Update the size.

```python
size[parent] += size[child]
```

This maintains the correct number of nodes in each component.

---

# ⚖️ Union by Rank vs Union by Size

| Union by Rank | Union by Size |
|---------------|---------------|
| Stores approximate height | Stores number of nodes |
| Slightly harder to understand | Easier to understand |
| Uses Rank Array | Uses Size Array |
| Same Time Complexity | Same Time Complexity |

Both are equally efficient.

Most programmers use **Union by Size** because it is simpler.

---

# ⏱ Time Complexity

| Operation | Complexity |
|-----------|------------|
| Find | O(α(N)) |
| Union | O(α(N)) |

where

```
α(N)
```

is the **Inverse Ackermann Function**, which behaves almost like **O(1)** for all practical input sizes.

---

# 📌 Summary

- Union by Size stores the number of nodes in each component.
- Always attach the smaller component below the larger one.
- Update the component size after every merge.
- Combined with Path Compression, DSU operations become almost constant time.
- Union by Size is easier to understand than Union by Rank while providing the same performance.

---

# 🎯 Key Takeaways

- **Size** = Number of nodes in a connected component.
- Merge **smaller → larger**.
- Update the size after merging.
- Always use **Path Compression** with Union by Size.
- Final complexity is **O(α(N))**, which is practically **O(1)**.

<br/><br/><br/>

# 🏗️ Complete Disjoint Set (DSU) Template

> **Graph Algorithms → Disjoint Set (Union-Find)**

---

# 📖 Introduction

Now that we have learned

- Union by Rank
- Union by Size
- Path Compression

it's time to combine everything into a **complete DSU implementation**.

This is the exact template that is used in almost every interview problem involving Disjoint Set.

---

# 🐍 Complete DSU (Union by Rank)

```python
class DisjointSet:

    def __init__(self, n):

        self.parent = [i for i in range(n + 1)]
        self.rank = [0] * (n + 1)

    def find(self, node):

        if self.parent[node] != node:
            self.parent[node] = self.find(self.parent[node])

        return self.parent[node]

    def union(self, u, v):

        pu = self.find(u)
        pv = self.find(v)

        if pu == pv:
            return

        if self.rank[pu] < self.rank[pv]:

            self.parent[pu] = pv

        elif self.rank[pv] < self.rank[pu]:

            self.parent[pv] = pu

        else:

            self.parent[pv] = pu
            self.rank[pu] += 1
```

---

# 🐍 Complete DSU (Union by Size)

```python
class DisjointSet:

    def __init__(self, n):

        self.parent = [i for i in range(n + 1)]
        self.size = [1] * (n + 1)

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
```

---

# 🔄 How DSU Works

Suppose we have

```
1

2

3

4

5
```

Initially

Parent

```
1 2 3 4 5
```

Every node belongs to a different component.

---

## Union(1,2)

Tree

```
1
|
2
```

Parent

```
1 1 3 4 5
```

---

## Union(3,4)

Tree

```
3
|
4
```

Parent

```
1 1 3 3 5
```

---

## Union(2,3)

Find Parent

```
find(2)

↓

1
```

Find Parent

```
find(3)

↓

3
```

Merge

```
1

↓

2

↓

3

↓

4
```

Now

Parent

```
1 1 1 3 5
```

After Path Compression

```
1

├──2

├──3

└──4
```

Parent

```
1 1 1 1 5
```

---

# 📝 Example Usage

```python
ds = DisjointSet(7)

ds.union(1, 2)
ds.union(2, 3)

print(ds.find(1))
print(ds.find(3))
```

Output

```
1

1
```

Both belong to the same connected component.

---

Now

```python
ds.union(4,5)
```

Component

```
4

↓

5
```

Now

```python
ds.union(3,5)
```

Entire graph becomes

```
1

├──2

├──3

├──4

└──5
```

Now

```
find(5)

↓

1
```

Everything belongs to the same component.

---

# 💻 How DSU is Used in Problems

Most graph problems follow this pattern.

### Step 1

Create DSU

```python
ds = DisjointSet(n)
```

---

### Step 2

Process every edge

```python
for u, v in edges:

    ds.union(u, v)
```

---

### Step 3

Check connectivity

```python
if ds.find(u) == ds.find(v):
```

Then

```
Same Connected Component
```

Else

```
Different Components
```

---

# 🌍 Common Interview Problems

DSU is used in:

- Kruskal's Algorithm
- Number of Provinces
- Redundant Connection
- Accounts Merge
- Most Stones Removed
- Making A Large Island
- Graph Valid Tree
- Network Connectivity
- Dynamic Connectivity Problems

---

# ❌ Common Mistakes

## Mistake 1

Comparing parents directly

Wrong

```python
if parent[u] == parent[v]
```

Correct

```python
if find(u) == find(v)
```

Always compare **ultimate parents**.

---

## Mistake 2

Not using Path Compression

Without it,

Find becomes slower.

Always write

```python
parent[node] = find(parent[node])
```

---

## Mistake 3

Unioning nodes directly

Wrong

```python
parent[u] = v
```

Correct

```python
pu = find(u)
pv = find(v)

parent[pu] = pv
```

Always merge **ultimate parents**.

---

## Mistake 4

Forgetting to return when both parents are same.

```python
if pu == pv:
    return
```

Otherwise,

you may incorrectly increase rank or size.

---

# ⏱ Time Complexity

| Operation | Complexity |
|-----------|------------|
| Find | O(α(N)) |
| Union | O(α(N)) |

where

```
α(N)
```

is the **Inverse Ackermann Function**.

For all practical purposes,

```
O(α(N))

≈

O(1)
```

---

# 🎯 Pattern Recognition

Whenever a problem says

- Merge groups
- Connected Components
- Dynamic Graph
- Cycle Detection
- Connectivity Queries
- Friend Groups
- Network Connections
- Kruskal's Algorithm

Immediately think

```
Disjoint Set Union
```

instead of DFS/BFS.

---

# 📌 DSU Workflow

```
Initially

Every node is its own parent

↓

Process each edge

↓

Union both nodes

↓

Compress Paths

↓

Find Ultimate Parents

↓

Answer Connectivity Queries
```

---

# 📝 Revision Cheat Sheet

```
DSU

↓

Find Parent

↓

Union

↓

Union by Rank

OR

Union by Size

↓

Path Compression

↓

Nearly O(1)

↓

Used in Graph Connectivity Problems
```

---

# ✅ Key Takeaways

- DSU maintains multiple connected components efficiently.
- The two main operations are **Find** and **Union**.
- **Find** returns the ultimate parent of a node.
- **Union** merges two different components.
- **Union by Rank** keeps the tree balanced using height.
- **Union by Size** keeps the tree balanced using component size.
- **Path Compression** flattens the tree and makes future Find operations extremely fast.
- Always combine **Path Compression** with **Union by Rank** or **Union by Size**.
- DSU is one of the most frequently used data structures in graph interview problems.