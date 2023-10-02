---

---
---
[LC Link](https://leetcode.com/problems/redundant-connection/)

---
---

```python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        par = [i for i in range(len(edges) + 1)]
        rank = [1] * (len(edges) + 1)

        def find(n):
            p = par[n]
            while p != par[p]:
                par[p] = par[par[p]]
                p = par[p]
            return p

        # return False if already unioned
        def union(n1, n2):
            p1, p2 = find(n1), find(n2)

            if p1 == p2:
                return False
            if rank[p1] > rank[p2]:
                par[p2] = p1
                rank[p1] += rank[p2]
            else:
                par[p1] = p2
                rank[p2] += rank[p1]
            return True

        for n1, n2 in edges:
            if not union(n1, n2):
                return [n1, n2]

```

### Problem Understanding:

Given an undirected graph with `n` nodes labeled `1` to `n`, and a list of edges where `edges[i] = [ai, bi]` indicates there's an undirected edge between `ai` and `bi`, determine the last edge that causes the graph to become cyclic. If the graph remains acyclic, return an empty list.

### 1st Principles of Problem-Solving:

**Core Principle**: This problem leverages the concept of Disjoint Set Union (DSU) or Union-Find. It's essentially about finding cycles in an undirected graph.

### Solving Intuition:

If two nodes in the edge share the same parent or root, then adding this edge will form a cycle. Hence, to find the redundant connection, iterate over the edges and unite their parents. If they're already united (i.e., have the same parent), then the current edge is redundant.

### Raw Algorithm:

1. Initialize the parent array (`par`) where each node is its parent.
2. Initialize the rank array to keep track of the size of the set each node belongs to.
3. Define the `find` function to return the parent/root of a node. It uses path compression for optimization.
4. Define the `union` function to unite two nodes. It uses union by rank to optimize the tree structure.
5. For each edge, apply union. If they're already united, return the edge as it's the redundant connection.

### Why The Code Is Written The Way It Is:

**1. `par` and `rank` arrays**:
- `par`: Represents the parent of each node.
- `rank`: Represents the size of the set each node belongs to.

**2. `find` function**:
- Gets the root/parent of a node using path compression for faster subsequent access.

**3. `union` function**:
- Unites two nodes if they aren't already united.
- Uses union by rank to ensure the tree remains as flat as possible for optimization.

**4. Main Loop**:
- For each edge, if union detects they're already united, it returns the edge as the redundant connection.

### Time Complexity:

The union and find operations have a complexity of nearly **O(1)** with path compression and union by rank. Thus, for `n` edges, the time complexity will be **O(n)**.

### Space Complexity:

The space complexity is **O(n)** due to the parent and rank arrays, where `n` is the number of nodes.

### Base Cases and Edge Cases:

**Base Cases**:
- For a single edge in the graph, it won't be redundant as no cycle will form.

**Edge Cases**:
- Multiple cycles in the graph: The algorithm will return the first redundant connection it encounters.

### Optimizing Clues:

- Path Compression in the `find` function optimizes future accesses.
- Union by Rank ensures that the tree remains as flat as possible, which aids in faster operations.

### Mnemonics to Remember:

- **"Parent's Parent for Path Compression"**: Remember to use `par[p] = par[par[p]]` for path compression in the `find` function.
- **"Unite and Check the Root"**: The main idea is to unite the nodes and see if they were already united.

In essence, the problem is about using the Union-Find data structure to detect cycles in an undirected graph. If during the union operation, two nodes are found to have the same parent, then a cycle has been detected, and the edge causing it is returned. The use of path compression and union by rank optimizes the operations to be almost constant time.