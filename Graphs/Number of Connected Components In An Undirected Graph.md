---

---
---
[LC Link](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

---
---

```python
class UnionFind:
    def __init__(self):
        self.f = {}

    def findParent(self, x):
        y = self.f.get(x, x)
        if x != y:
            y = self.f[x] = self.findParent(y)
        return y

    def union(self, x, y):
        self.f[self.findParent(x)] = self.findParent(y)


class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        dsu = UnionFind()
        for a, b in edges:
            dsu.union(a, b)
        return len(set(dsu.findParent(x) for x in range(n)))

```

Alright, let's delve deep into this problem.

### Problem Understanding:

Given `n` nodes labeled from `0` to `n-1` and a list of undirected edges, write a function to find the number of connected components in an undirected graph. 

### 1st Principles of Problem-Solving:

**Core Principle**: The problem leverages the concept of Disjoint Set Union (DSU) or Union-Find. It's used to determine the connected components in a graph.

### Solving Intuition:

Each connected component has a unique parent or representative. By iterating through the graph edges, we can merge nodes into components. By the end of this process, each distinct parent represents a connected component. Thus, counting the number of unique parents will give the number of components.

### Raw Algorithm:

1. Define a UnionFind class that can perform union and find operations.
2. Initialize an array to keep track of the parent of each node.
3. For each edge, apply union operation to merge the components.
4. Iterate over all nodes and find their representative/parent.
5. Count the number of unique parents.

### Why The Code Is Written The Way It Is:

**UnionFind Class**:

1. **`__init__`**:
   - Initializes a dictionary `f` that acts as a disjoint set. Each node is its parent initially.

2. **`findParent`**:
   - If the node is its parent, it returns the node.
   - Otherwise, it recursively finds the parent of the node until a node is its parent. This is path compression.

3. **`union`**:
   - It unites two nodes by setting the parent of one node as the parent of the other.

**Solution Class**:

1. **`countComponents`**:
   - Initializes an instance of the UnionFind class.
   - For each edge, it unites the two nodes.
   - It finds the parent of each node from 0 to n-1 and counts the number of unique parents.

### Time Complexity:

- Union and Find operations are almost **O(1)** with path compression. 
- Thus, for `n` nodes and `e` edges, the time complexity will be **O(n + e)**.

### Space Complexity:

- **O(n)** due to the disjoint set.

### Base Cases and Edge Cases:

**Base Cases**:
- If there are no edges, then each node is its component. So, the result is `n`.

**Edge Cases**:
- If all nodes are connected directly or indirectly, there is only one connected component.

### Optimizing Clues:

- Path Compression in `findParent` makes subsequent operations faster.
- Instead of counting the number of unique parents every time, we could decrement a counter every time two separate components are merged.

### Mnemonics to Remember:

- **"Find and Compress"**: In the `findParent` function, the process involves finding the parent and compressing the path.
- **"Union to Connect"**: Using the `union` function to connect nodes in the graph.
- **"Unique Parents for Components"**: Counting unique parents to determine the number of components.

Essentially, the problem revolves around using the DSU to detect and count the number of connected components in a graph. The idea is to repeatedly merge nodes based on edges, and by the end of the process, each distinct parent or root represents a connected component. Counting these unique roots gives the number of components.