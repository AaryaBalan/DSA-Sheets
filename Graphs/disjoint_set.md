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

<br/><br/>

# Expample Problems

- [721. Accounts Merge](#721-accounts-merge)
- [Number Of Islands](#number-of-islands-ii-online-queries)
- [827. Making A Large Island](#827-making-a-large-island)

<br/><br/>

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

<br/><br/><br/><br/><br/>

---

# 721. Accounts Merge

**Difficulty:** Medium  
**Topic:** Graph, Disjoint Set Union (DSU), HashMap

---

# 📖 Problem Understanding

You are given several user accounts.

Each account contains

```
[Name, Email1, Email2, Email3...]
```

Example

```python
[
["John","a@mail","b@mail"],
["John","b@mail","c@mail"],
["Mary","d@mail"],
["John","e@mail"]
]
```

Notice something important.

The name

```
John
```

appears multiple times.

Does that automatically mean all John's belong to the same person?

**No.**

The problem clearly says

> Two accounts belong to the same person **only if they share at least one email.**

That means

Names are **not enough**.

Emails determine the identity.

---

# 🌎 Real-Life Analogy

Imagine Gmail accounts.

Person A owns

```
a@gmail
b@gmail
```

Later they create another account

```
b@gmail
c@gmail
```

Since both accounts contain

```
b@gmail
```

they obviously belong to the same person.

So they should be merged.

---

# 📥 Understanding the Input

Example

```python
[
["John","johnsmith@mail.com","john_newyork@mail.com"],
["John","johnsmith@mail.com","john00@mail.com"],
["Mary","mary@mail.com"],
["John","johnnybravo@mail.com"]
]
```

Visualize it.

Account 0

```
John

johnsmith

john_newyork
```

Account 1

```
John

johnsmith

john00
```

Common email

```
johnsmith
```

Therefore

```
Account0

↓

Same Person

↓

Account1
```

---

Mary

```
mary@mail
```

No common email.

Different person.

---

Third John

```
johnnybravo
```

No common email.

Different person.

---

# 💭 First Thought (Brute Force)

A beginner might think

Compare every account with every other account.

```
Account0 vs Account1

Account0 vs Account2

Account0 vs Account3

Account1 vs Account2

...
```

If they share an email,

merge them.

---

# ❌ Why Brute Force is Bad?

Suppose

```
1000 accounts
```

Comparing every pair

```
1000 × 1000
```

Too slow.

Time complexity becomes roughly

```
O(N² × Emails)
```

Not acceptable.

We need something smarter.

---

# 💡 Building the Intuition

Instead of comparing accounts,

think about **connections**.

Suppose

Account 0

```
a

b
```

Account 1

```
b

c
```

Common email

```
b
```

That means

```
0 -------- 1
```

They are connected.

Now think

What data structure is used to maintain connected groups?

**Disjoint Set Union (DSU).**

---

# 🧠 Recognizing This as a DSU Problem

Whenever you see words like

- Merge
- Connected
- Same Group
- Common Element
- Belong Together

Think

```
Connected Components

↓

DSU
```

---

# 🔑 Key Observation

We DO NOT merge emails.

We merge

```
Account Indexes
```

Suppose

```
Account0

↓

John

a

b
```

```
Account1

↓

John

b

c
```

Email

```
b
```

appears in both.

Therefore

```
Union(0,1)
```

Done.

---

# 📌 Main Idea

We need a way to know

"Have I seen this email before?"

Use a HashMap.

```
email

↓

first account index
```

Example

Initially

```
{}
```

Read

```
a
```

Store

```
a → Account0
```

Read

```
b
```

Store

```
b → Account0
```

Now move to Account1.

Read

```
b
```

Already exists.

```
b → Account0
```

Current account

```
Account1
```

Therefore

```
Union(Account0, Account1)
```

Easy!

---

# 📝 Algorithm

## Step 1

Create DSU.

Each account is initially its own parent.

```
0

1

2

3
```

---

## Step 2

Traverse every account.

For every email

If email is new

Store

```
email → account
```

Otherwise

```
Union(previous account,current account)
```

---

## Step 3

After all unions,

every connected account has the same parent.

Now traverse every email again.

Find its parent.

Store

```
Parent

↓

Emails
```

---

## Step 4

Sort emails.

Insert owner's name.

Return answer.

---

# 🌳 Dry Run

Input

```python
[
["John","johnsmith@mail.com","john_newyork@mail.com"],
["John","johnsmith@mail.com","john00@mail.com"],
["Mary","mary@mail.com"],
["John","johnnybravo@mail.com"]
]
```

---

### Step 1

DSU

```
0

1

2

3
```

---

### Step 2

Process Account0

```
johnsmith

↓

0
```

```
john_newyork

↓

0
```

HashMap

```
johnsmith → 0

john_newyork → 0
```

---

Process Account1

Email

```
johnsmith
```

Already exists.

```
Union(0,1)
```

Store

```
john00 → 1
```

Now

DSU

```
0 -----1

2

3
```

---

Process Account2

```
mary

↓

2
```

---

Process Account3

```
johnnybravo

↓

3
```

---

### Step 3

Group emails

Parent

```
0

↓

johnsmith

john_newyork

john00
```

Parent

```
2

↓

mary
```

Parent

```
3

↓

johnnybravo
```

---

### Step 4

Sort

```
john00

john_newyork

johnsmith
```

Final Answer

```python
[
["John",
"john00@mail.com",
"john_newyork@mail.com",
"johnsmith@mail.com"],

["Mary",
"mary@mail.com"],

["John",
"johnnybravo@mail.com"]
]
```

---

# 🐍 Python Solution

```python
from collections import defaultdict

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


class Solution:
    def accountsMerge(self, accounts):

        n = len(accounts)

        ds = DisjointSet(n)

        emailToAccount = {}

        # Step 1: Merge accounts
        for i in range(n):

            for email in accounts[i][1:]:

                if email not in emailToAccount:
                    emailToAccount[email] = i

                else:
                    ds.union(i, emailToAccount[email])

        # Step 2: Group emails by parent
        mergedEmails = defaultdict(list)

        for email, account in emailToAccount.items():

            parent = ds.find(account)

            mergedEmails[parent].append(email)

        # Step 3: Build answer
        ans = []

        for parent, emails in mergedEmails.items():

            ans.append(
                [accounts[parent][0]] + sorted(emails)
            )

        return ans
```

---

# 🔍 Code Explanation

### Create DSU

Every account is initially separate.

---

### emailToAccount

Stores

```
Email

↓

First Account Index
```

---

### Union

Whenever an email repeats

```
Union(previous,current)
```

Now both accounts belong to the same person.

---

### Find Parent

Later,

every email is grouped using

```
Ultimate Parent
```

---

### Sort

Emails must be returned in

```
Lexicographical Order
```

So we use

```python
sorted(emails)
```

---

# ⏱ Complexity

Let

```
N = Number of Accounts

E = Total Emails
```

Building HashMap

```
O(E)
```

DSU

```
O(E × α(N))
```

Sorting

```
O(E log E)
```

Overall

```
O(E log E)
```

Space

```
O(E)
```

---

# ❌ Common Mistakes

### Mistake 1

Merging based on names.

Wrong.

Always merge using

```
Emails
```

---

### Mistake 2

Unioning emails instead of accounts.

Wrong.

DSU nodes are

```
Account Indexes
```

---

### Mistake 3

Forgetting to call

```python
find()
```

before grouping.

Always use the ultimate parent.

---

# 🎯 Pattern Recognition

Whenever you see

- Merge Groups
- Common Element
- Same Person
- Connected Components
- Belong Together

Immediately think

```
Disjoint Set Union
```

---

# 📌 Key Takeaways

- Treat each **account as a node**.
- Emails act as **connections** between accounts.
- A repeated email means two accounts belong to the same connected component.
- Use a **HashMap** to quickly detect repeated emails.
- Use **DSU** to merge connected accounts efficiently.
- After all merges, group emails by the **ultimate parent**, sort them, and build the final answer.
- This is a classic example of converting a real-world merging problem into a **connected components** problem solved with DSU.

<br/><br/><br/><br/><br/>

---

# Number of Islands II (Online Queries)

> **Prerequisites**
>
> - Graph Basics
> - Connected Components
> - Disjoint Set Union (DSU)
>
> This problem is one of the most important applications of **Disjoint Set Union (DSU)** in graphs. Unlike the classic Number of Islands problem where the entire grid is given at once, here the grid changes dynamically after each operation.

# Introduction

This problem belongs to the category of **Dynamic Connectivity Problems**.

Initially,

- Every cell is **water (0)**.
- One by one, some cells become **land (1)**.
- After **each operation**, we must immediately report the current number of islands.

Since the answer is required **after every update**, this is called an **Online Query Problem**. :contentReference[oaicite:1]{index=1}

---

# What are Online Queries?

An **online query** means:

```
Receive a query

↓

Update the data

↓

Immediately return the answer

↓

Receive next query

↓

Update

↓

Return answer
```

Unlike offline problems, we **cannot wait until all operations finish**.

We must answer after every operation.

---

# Problem Statement

Initially the grid is

```text
0 0 0
0 0 0
0 0 0
```

Every operation converts

```
0 → 1
```

For example

```
Operation 1

(1,1)
```

Grid

```text
0 0 0
0 1 0
0 0 0
```

Answer

```
1 Island
```

---

Next

```
Operation

(0,1)
```

Grid

```text
0 1 0
0 1 0
0 0 0
```

Now

```
Both cells share a side.

↓

One Island
```

Answer

```
1
```

---

# Important Rule

Cells are connected only through

- Up
- Down
- Left
- Right

Not diagonally.

Example

```text
1 0
0 1
```

These are

```
NOT Connected
```

because they touch only diagonally.

---

# Key Observation

Whenever a new land appears,

only **four neighbouring cells** can affect the answer.

```
      Up

Left Current Right

    Down
```

No other cell can directly change the connectivity.

---

# First Intuition

Suppose

```
(1,1)
```

becomes land.

Obviously

```
Island Count++

↓

1
```

Now

```
(1,3)
```

becomes land.

```
Island Count++

↓

2
```

Now

```
(1,2)
```

becomes land.

Initially,

```
Island Count++

↓

3
```

But

```
(1,2)

connects

↓

(1,1)

↓

count--

↓

2
```

Again

```
(1,2)

connects

↓

(1,3)

↓

count--

↓

1
```

Final answer

```
One Island
```

This is exactly what DSU helps us maintain efficiently.

---

# Why Not DFS/BFS?

Suppose

```
100 × 100 Grid
```

After every operation,

run DFS again.

```
1000 Operations

×

10000 Cells

=

10 Million Visits
```

Most of this work is repeated.

We only changed **one cell**, but DFS explores the entire grid again.

This is inefficient.

---

# Why DSU?

DSU is built for

```
Dynamic Connectivity
```

Whenever two neighbouring lands meet,

we simply

```
Union(Current,Neighbour)
```

No need to recompute everything.

This makes the solution much faster.

---

# Thinking in Terms of Graph

Instead of viewing the matrix as a grid,

think of it as a graph.

Every land cell

↓

becomes a graph node.

If two land cells are adjacent,

↓

create an edge between them.

Example

```text
1 1 1
```

becomes

```
● —— ● —— ●
```

Now DSU merges these connected nodes into one connected component.

---

# Representing Grid Cells as DSU Nodes

DSU works with

```
0

1

2

3

...
```

It does **not understand**

```
(row,col)
```

So every grid cell must be converted into a unique node number.

---

# Formula

Suppose

```
rows = 4

cols = 5
```

Grid

```text
(0,0) (0,1) (0,2) (0,3) (0,4)

(1,0) (1,1) (1,2) (1,3) (1,4)

(2,0) (2,1) (2,2) (2,3) (2,4)

(3,0) (3,1) (3,2) (3,3) (3,4)
```

Node numbering

```text
0  1  2  3  4

5  6  7  8  9

10 11 12 13 14

15 16 17 18 19
```

Formula

```python
node = row * cols + col
```

Examples

```
(0,0)

↓

0
```

```
(1,2)

↓

1 × 5 + 2

↓

7
```

```
(3,4)

↓

3 × 5 + 4

↓

19
```

This gives every cell a unique DSU node.

---

# Complete Algorithm

For every operation

## Step 1

If already land,

```
Append current answer.

Continue.
```

---

## Step 2

Convert water into land.

```
Visited = True

Island Count++
```

Initially every new land is treated as a new island.

---

## Step 3

Check all four neighbours.

```
Up

Down

Left

Right
```

---

## Step 4

If neighbour

- is inside the grid
- is already land

then

```
Union(Current,Neighbour)
```

---

## Step 5

If Union succeeds,

that means

```
Two Different Islands

↓

Merged

↓

Island Count--
```

If they already belonged to the same component,

do nothing.

---

## Step 6

Store the current island count.

Repeat for the next operation.

---

# Dry Run

Operations

```
(1,1)

(1,3)

(1,2)
```

---

## Operation 1

Grid

```text
0 0 0 0

0 1 0 0

0 0 0 0
```

```
Count = 1
```

Answer

```
[1]
```

---

## Operation 2

Grid

```text
0 0 0 0

0 1 0 1

0 0 0 0
```

```
Count = 2
```

Answer

```
[1,2]
```

---

## Operation 3

Initially

```
Count++

↓

3
```

Neighbours

```
Left

↓

Land

↓

Union

↓

Count--

↓

2
```

Neighbour

```
Right

↓

Land

↓

Union

↓

Count--

↓

1
```

Final Grid

```text
0 0 0 0

0 1 1 1

0 0 0 0
```

Answer

```
[1,2,1]
```

---

# Python Solution

```python
from typing import List

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
            return False

        if self.size[pu] < self.size[pv]:
            self.parent[pu] = pv
            self.size[pv] += self.size[pu]
        else:
            self.parent[pv] = pu
            self.size[pu] += self.size[pv]

        return True


class Solution:

    def numOfIslands(self, rows, cols, operators):

        ds = DisjointSet(rows * cols)

        visited = [[0] * cols for _ in range(rows)]

        directions = [
            (-1,0),
            (1,0),
            (0,-1),
            (0,1)
        ]

        count = 0
        ans = []

        for row, col in operators:

            if visited[row][col]:
                ans.append(count)
                continue

            visited[row][col] = 1
            count += 1

            curr = row * cols + col

            for dx, dy in directions:

                nr = row + dx
                nc = col + dy

                if (
                    0 <= nr < rows and
                    0 <= nc < cols and
                    visited[nr][nc]
                ):

                    adj = nr * cols + nc

                    if ds.union(curr, adj):
                        count -= 1

            ans.append(count)

        return ans
```

---

# Time Complexity

For every operation

- Check 4 neighbours → **O(1)**
- DSU Find/Union → **O(α(N))**

Overall

```
O(K × α(rows × cols))
```

where

- **K** = number of operations
- **α** = Inverse Ackermann Function (almost constant)

Practically,

```
O(K)
```

---

# Space Complexity

```
Visited Matrix

↓

O(rows × cols)
```

DSU Arrays

```
Parent

Size

↓

O(rows × cols)
```

Total

```
O(rows × cols)
```

---

# Key Takeaways

- This is a **dynamic connectivity** problem.
- Each grid cell is treated as a DSU node.
- Convert `(row, col)` into a unique node using:
  ```python
  node = row * cols + col
  ```
- Every new land initially creates a new island.
- Only the four neighbouring cells can change connectivity.
- Every successful `union()` merges two islands, so decrement the island count.
- DSU avoids recomputing all islands after every operation, making it the optimal solution for online queries.

<br/><br/><br/><br/><br/>

---

# 827. Making A Large Island

**Difficulty:** Hard  
**Topics:** Graph, Matrix, DFS, Disjoint Set Union (DSU)

---

# 📖 Problem Understanding

You are given a grid containing only

```
0 → Water

1 → Land
```

You are allowed to change **at most one** water cell into land.

Your goal is to obtain the **largest possible island**.

An island means all connected 1's using

- Up
- Down
- Left
- Right

connections only.

---

# Example

Input

```text
1 0

0 1
```

If we convert

```
(0,1)
```

Grid becomes

```text
1 1

0 1
```

Now

```
(0,0)

↓

(0,1)

↓

(1,1)
```

Everything becomes connected.

Largest island

```
3
```

Answer

```
3
```

---

# 🌎 Real-Life Analogy

Imagine several small islands in the ocean.

```
🏝️      🏝️
```

You are allowed to build **one bridge**.

Where should you build it?

Obviously,

connect the islands that produce the largest land mass.

That is exactly this problem.

The bridge is

```
Changing one 0 into 1
```

---

# First Thought (Brute Force)

A beginner usually thinks

For every zero

```
Convert it into 1

↓

Run DFS

↓

Find island size

↓

Restore 0
```

Repeat for every zero.

---

# Example

Grid

```text
1 1

0 1
```

Convert

```
0 → 1
```

Run DFS

Island size

```
4
```

Restore

Repeat for every zero.

---

# Why is Brute Force Slow?

Suppose

```
500 × 500 Grid
```

Total cells

```
250000
```

Suppose

```
125000 zeros
```

For every zero

Run DFS

```
250000

×

125000
```

Way too slow.

Time complexity becomes

```
O((N²)²)

=

O(N⁴)
```

Impossible.

---

# Building the Intuition

Instead of asking

```
If I flip this zero,

how big is the island?
```

Ask

```
What islands already exist?
```

Suppose

```text
1 1 0

1 0 1

0 1 1
```

Current islands

```
Island A

size = 3
```

```
Island B

size = 3
```

Now flip

```
Center 0
```

It connects

```
Island A

+

Island B
```

Final size

```
1

+

3

+

3

=

7
```

Notice something.

We never needed to rebuild islands.

We only needed to know

```
Existing island sizes.
```

This is the key insight.

---

# Why DSU?

DSU can answer

```
Which island does this cell belong to?

↓

find()
```

and

```
How big is that island?

↓

size[parent]
```

So instead of rebuilding islands,

we build them once.

---

# The Biggest Idea

DSU is **NOT**

```
Temporary Calculator
```

It is

```
A Database

of

Connected Components
```

Build it once.

Never destroy it.

Later,

just query it.

---

# Step 1

Union every existing land.

Example

```text
1 1

1 0
```

DSU builds

```
Parent

↓

One Component

Size = 3
```

Done.

---

# Step 2

Now visit every zero.

Suppose

```
(1,1)
```

Neighbours

```
Up

↓

Island A
```

Left

```
↓

Island A
```

Both are actually the SAME island.

Important.

---

# Why Set is Needed?

Suppose

```text
1 1

0 1
```

Neighbours

```
Up

↓

Parent = 0
```

Right

```
↓

Parent = 0
```

If we simply add

```
3

+

3
```

Answer becomes

```
7
```

Wrong.

Actual answer

```
4
```

So we store

```python
parents = set()
```

Now

```
Parent 0

already counted.
```

Only one copy is used.

---

# Key Observation

When we flip a zero,

we DO NOT perform unions.

Instead

```
Look around.

↓

Find neighbouring islands.

↓

Add their sizes.
```

That is all.

---

# Complete Algorithm

## Phase 1

Build all islands.

Traverse grid.

Whenever

```
Current = 1

Neighbour = 1
```

Perform

```
Union(Current,Neighbour)
```

After this,

every island knows

```
Parent

Size
```

---

## Phase 2

Traverse grid again.

For every zero

```
Area = 1
```

because we are flipping it.

Now

collect unique parents.

For every neighbour

```
Find Parent

↓

Not counted before?

↓

Area += size[parent]
```

Maximum answer.

---

## Phase 3

Special Case

Suppose

```text
1 1

1 1
```

No zero exists.

Answer

```
N × N
```

---

# Dry Run

Input

```text
1 1

0 1
```

---

## Step 1

Build DSU

All existing ones

```
(0,0)

↓

(0,1)

↓

(1,1)
```

One component

```
Size = 3
```

---

## Step 2

Visit

```
(1,0)
```

Area

```
1
```

Neighbours

```
Up

↓

Parent = A

Size = 3
```

Right

```
↓

Parent = A
```

Already counted.

Ignore.

Area

```
1

+

3

=

4
```

Answer

```
4
```

---

# Another Example

Input

```text
1 0

0 1
```

Initially

Two islands

```
Size 1

Size 1
```

Flip

```
(0,1)
```

Neighbours

Left

```
Size 1
```

Down

```
Size 1
```

Area

```
1

+

1

+

1

=

3
```

Answer

```
3
```

---

# Python Solution

```python
from typing import List

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


class Solution:

    def largestIsland(self, grid: List[List[int]]) -> int:

        n = len(grid)

        ds = DisjointSet(n * n)

        directions = [
            (-1,0),
            (1,0),
            (0,-1),
            (0,1)
        ]

        # Step 1: Build all islands
        for r in range(n):
            for c in range(n):

                if grid[r][c] == 0:
                    continue

                curr = r * n + c

                for dx, dy in directions:

                    nr = r + dx
                    nc = c + dy

                    if (
                        0 <= nr < n and
                        0 <= nc < n and
                        grid[nr][nc] == 1
                    ):

                        ds.union(curr, nr * n + nc)

        ans = 0
        hasZero = False

        # Step 2: Try flipping every zero
        for r in range(n):
            for c in range(n):

                if grid[r][c] == 1:
                    continue

                hasZero = True

                area = 1

                parents = set()

                for dx, dy in directions:

                    nr = r + dx
                    nc = c + dy

                    if (
                        0 <= nr < n and
                        0 <= nc < n and
                        grid[nr][nc] == 1
                    ):

                        parent = ds.find(nr * n + nc)

                        if parent not in parents:

                            parents.add(parent)

                            area += ds.size[parent]

                ans = max(ans, area)

        if not hasZero:
            return n * n

        return ans
```

---

# Code Explanation

## Build Islands

```python
ds.union(curr, neighbour)
```

Merges every connected land.

Finally

```
Each island

↓

One Parent

↓

One Size
```

---

## Visiting Every Zero

Start

```python
area = 1
```

because we convert

```
0

↓

1
```

Then

collect neighbouring islands.

---

## Why Set?

Suppose

```text
1 1

0 1
```

Neighbours

```
Up

↓

Parent 5
```

Right

```
↓

Parent 5
```

Without set

```
1

+

3

+

3

=

7
```

Wrong.

Set stores

```
{5}
```

Only once.

Answer

```
4
```

---

# Complexity Analysis

Let

```
N = Grid Size
```

Building DSU

```
O(N² × α(N²))
```

Checking every zero

```
O(N² × 4 × α(N²))
```

Overall

```
O(N²)
```

because

```
α(N)

≈ Constant
```

Space

```
Parent Array

+

Size Array

↓

O(N²)
```

---

# Common Mistakes

## ❌ Mistake 1

Building DSU separately for every zero.

Wrong.

Build it only once.

---

## ❌ Mistake 2

Calling

```python
clearUnion()
```

Never do this.

You lose all island information.

---

## ❌ Mistake 3

Performing union after flipping zero.

Not needed.

Only query neighbouring island sizes.

---

## ❌ Mistake 4

Not using a set.

Same island may touch the zero from multiple directions.

Without a set,

you count it multiple times.

---

# Pattern Recognition

Whenever you see

- Largest Connected Component
- Merge Components
- Flip One Cell
- Add One Edge
- Query Existing Components

Think

```
Build Components Once

↓

Store Their Sizes

↓

Answer Queries
```

This is one of the most common DSU interview patterns.

---

# Key Takeaways

- Treat every land cell as a DSU node.
- Build all connected islands **once**.
- Store the size of every connected component.
- For every `0`, inspect its four neighbours.
- Collect **unique island parents** using a set.
- The possible island size is:
  ```
  1 + sum(size of unique neighbouring islands)
  ```
- Never rebuild or reset the DSU for each `0`; use it as a permanent representation of the current connected components.

<br/><br/><br/><br/><br/>

---

