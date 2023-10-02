---

---
---
[LC Link](https://leetcode.com/problems/clone-graph/)

---
---

```python
class Solution:
    def cloneGraph(self, node: "Node") -> "Node":
        oldToNew = {}

        def dfs(node):
            if node in oldToNew:
                return oldToNew[node]

            copy = Node(node.val)
            oldToNew[node] = copy
            for nei in node.neighbors:
                copy.neighbors.append(dfs(nei))
            return copy

        return dfs(node) if node else None

```

### Understanding the Problem:

The problem asks to clone a given undirected graph. An undirected graph can be represented as nodes connected to each other through neighbors.

### 1st Principles of Problem-Solving:

**What is a Clone?** - A clone of a graph is a new graph that is structurally identical to the original but is a completely independent copy.

### Solving Intuition:

The intuitive approach to solve the cloning problem is to traverse the graph. During this traversal, for each node encountered, we'd create its copy. Two effective traversal methods are DFS (Depth First Search) and BFS (Breadth First Search). Here, a DFS-based approach is used.

### Raw Algorithm:

1. Start from the given node.
2. For each node encountered:
   - If its copy doesn't exist, create a new node with the same value.
   - Recursively (DFS) create copies for its neighbors.
   - Attach these copied neighbors to the copied node.
3. Return the copied start node.

### Why The Code Is Written The Way It Is:

**1. `oldToNew` Dictionary**:
- **Purpose**: Maps original nodes to their corresponding copies. This serves two purposes:
    1. Helps in avoiding cloning a node more than once.
    2. Helps in connecting already cloned neighbors to newer nodes.
- **Why Dictionary**: Lookup, insertion, and checking existence in a dictionary is O(1). It's efficient for our use-case.

**2. `dfs(node)` function**:
- **Recursion**: This function uses recursion, which is characteristic of DFS. The idea is to explore as far as possible along each branch before backtracking.
- **Base Case**: If the node is already in the `oldToNew` dictionary, we've already cloned it. So, we return its clone.
- **Recursive Call**: For every neighbor of the current node, the function calls itself. This is the depth-first search in action.

**3. `copy.neighbors.append(dfs(nei))`**:
- Here, for every neighbor of the original node, we're appending its copy to the neighbors of our current copied node. This ensures the structural integrity of the cloned graph remains the same as the original.

### Time Complexity:

**O(N)** - We're visiting each node and its neighbors exactly once. Here, N is the total number of nodes.

### Space Complexity:

**O(N)** - In the worst case, the space used by the `oldToNew` dictionary and the recursive call stack would be O(N).

### Base Cases and Edge Cases:

**Base Case**: 
- When we encounter a node that we've already cloned (it's in our `oldToNew` dictionary), we simply return its cloned copy.

**Edge Case**:
- If the given node is `None` (empty graph), we return `None`.

### Optimizing Clues:

- BFS can also be used for cloning. Instead of a recursive approach, you'd use a queue to handle the nodes.
- Another optimization, if node values are unique, we could use the node value as a key in our dictionary instead of the node itself.

### Mnemonics to Remember:

- **"Dive Deep, Clone Deep"**: Dive deep into the graph with DFS, and as you explore, you clone.
- **"Old to New, Map the Zoo"**: Use the `oldToNew` dictionary to map original nodes to their clones, ensuring you don't recreate any node and maintain graph structure.

In essence, the solution works by diving deep into the graph using DFS and mapping original nodes to their clones to ensure an identical structure in the cloned graph.