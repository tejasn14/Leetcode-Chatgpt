---

---
---
[LC Link](https://leetcode.com/problems/graph-valid-tree/)

---
---

```python
# Problem is free on Lintcode
class Solution:
    """
    @param n: An integer
    @param edges: a list of undirected edges
    @return: true if it's a valid tree, or false
    """

    def validTree(self, n, edges):
        if not n:
            return True
        adj = {i: [] for i in range(n)}
        for n1, n2 in edges:
            adj[n1].append(n2)
            adj[n2].append(n1)

        visit = set()

        def dfs(i, prev):
            if i in visit:
                return False

            visit.add(i)
            for j in adj[i]:
                if j == prev:
                    continue
                if not dfs(j, i):
                    return False
            return True

        return dfs(0, -1) and n == len(visit)
    
    
    
    # alternative solution via DSU O(ElogV) time complexity and 
    # save some space as we don't recreate graph\tree into adjacency list prior dfs and loop over the edge list directly
    class Solution:
    """
    @param n: An integer
    @param edges: a list of undirected edges
    @return: true if it's a valid tree, or false
    """
    def __find(self, n: int) -> int:
        while n != self.parents.get(n, n):
            n = self.parents.get(n, n)
        return n

    def __connect(self, n: int, m: int) -> None:
        pn = self.__find(n)
        pm = self.__find(m)
        if pn == pm:
            return
        if self.heights.get(pn, 1) > self.heights.get(pm, 1):
            self.parents[pn] = pm
        else:
            self.parents[pm] = pn
            self.heights[pm] = self.heights.get(pn, 1) + 1
        self.components -= 1

    def valid_tree(self, n: int, edges: List[List[int]]) -> bool:
        # init here as not sure that ctor will be re-invoked in different tests
        self.parents = {}
        self.heights = {}
        self.components = n

        for e1, e2 in edges:
            if self.__find(e1) == self.__find(e2):  # 'redundant' edge
                return False
            self.__connect(e1, e2)

        return self.components == 1  # forest contains one tree


```

This problem requires us to determine if a given graph with `n` nodes and some edges form a valid tree.

### First Principles of Problem-Solving:

**Core Principle**: A valid tree is defined by two properties:
1. There are no cycles.
2. All nodes are connected.

Using these principles, we can deduce that:
1. For `n` nodes to form a valid tree, there must be exactly `n-1` edges.
2. Using Depth-First Search (DFS) on a node, if we encounter any already-visited node that isn't the direct parent of the current node, then a cycle exists.
3. After DFS from one node, all nodes must have been visited if it's a valid tree.

### Solving Intuition:

Given the core principles, we can intuitively decide to employ DFS to traverse the tree to verify if a cycle exists and if all nodes are connected. Additionally, by simply comparing the number of edges and nodes, we can quickly check the valid tree condition.

### Raw Algorithm for DFS Solution:

1. Construct an adjacency list representation of the graph.
2. Start a DFS from the first node, marking nodes as visited along the way.
3. If, during DFS, an already visited node is encountered that's not the parent of the current node, a cycle exists.
4. After DFS, check if all nodes were visited. If not, the nodes aren't fully connected.
5. Validate that the number of edges is `n-1`.

### Why the DFS Solution Code is Written This Way:

1. **Adjacency List (`adj`)**: For each edge, add nodes to each other's list to denote an undirected graph.
2. **DFS Function**: A recursive function that visits each node and its neighbors.
   - `i` is the current node and `prev` is its parent. `prev` is used to allow backtracking to the parent without flagging it as a cycle.
   - If a visited node is encountered that's not the parent, return `False`.
   - Recursively check all neighbors.
3. The main function initiates the DFS from the first node and checks if the number of visited nodes is `n`.

### Time Complexity:

For the DFS solution:
- **O(n + e)**, where `n` is the number of nodes and `e` is the number of edges, due to DFS and graph construction.

### Raw Algorithm for DSU Solution:

1. Implement helper functions `__find` and `__connect` for the Disjoint Set Union.
2. For each edge, check if the nodes are already connected. If yes, a cycle is detected.
3. If not, connect the nodes.
4. By the end, there should only be one component (or tree) for a valid tree.

### Why the DSU Solution Code is Written This Way:

1. **`__find` Function**: Returns the root or parent of a node. Implements path compression for efficiency.
2. **`__connect` Function**: Connects two nodes. If they have different parents, they are merged under a single parent.
3. In `valid_tree`, iterate over each edge and check for cycles by seeing if two nodes have the same parent. If not, connect them. At the end, if there's more than one component, it's not a valid tree.

### Time Complexity:

For the DSU solution:
- **O(e * log(n))** where `e` is the number of edges (due to union/find operations) and `n` is the number of nodes.

### Base Cases and Edge Cases:

**Base Cases**:
- A graph with 1 node and no edges is a valid tree.
- For `n` nodes, if there aren't `n-1` edges, it can't be a valid tree.

**Edge Cases**:
- If the graph forms multiple disconnected components, it's not a valid tree.
- A graph where a node connects to itself isn't a valid tree.

### Optimizing Clues:

- Path compression and union-by-rank in DSU improve efficiency.
- Early exit if the number of edges isn't `n-1`.

### Mnemonics to Remember:

- **"Two Checks for Trees"**: Remember that we're looking for 1) no cycles and 2) full connectivity.
- **"DFS Detects Cycles"**: When doing DFS, visiting an already-visited node (that's not the immediate parent) means there's a cycle.
- **"DSU Connects Components"**: DSU helps in finding and connecting components, and at the end, a valid tree will only have one component.

In summary, this problem can be approached in two main ways: DFS and DSU. Both methods rely on understanding the properties of a tree, and each has its benefits. DFS provides a more intuitive approach, while DSU offers a more optimized solution for larger graphs.