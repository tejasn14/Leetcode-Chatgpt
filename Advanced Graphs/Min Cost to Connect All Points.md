---

---
---
[LC Link](https://leetcode.com/problems/min-cost-to-connect-all-points/)

---
---

```python
class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        N = len(points)
        adj = {i: [] for i in range(N)}  # i : list of [cost, node]
        for i in range(N):
            x1, y1 = points[i]
            for j in range(i + 1, N):
                x2, y2 = points[j]
                dist = abs(x1 - x2) + abs(y1 - y2)
                adj[i].append([dist, j])
                adj[j].append([dist, i])

        # Prim's
        res = 0
        visit = set()
        minH = [[0, 0]]  # [cost, point]
        while len(visit) < N:
            cost, i = heapq.heappop(minH)
            if i in visit:
                continue
            res += cost
            visit.add(i)
            for neiCost, nei in adj[i]:
                if nei not in visit:
                    heapq.heappush(minH, [neiCost, nei])
        return res

```

The problem described is about connecting all the points using the minimum cost, where the cost is the Manhattan distance between two points. This is a classic example of the Minimum Spanning Tree (MST) problem.

### First Principles of Problem-Solving:

**Core Principle**: We need to connect all the points with the minimum possible cost.

### Solving Intuition:

1. **Graph Representation**: Each point can be considered as a node in a graph. The cost to connect two nodes is the Manhattan distance between the respective points. 

2. **Minimum Spanning Tree**: We aim to find the MST for the graph because we want to connect all the points with minimum total cost. 

3. **Prim's Algorithm**: One of the classic algorithms to find the MST is Prim's algorithm. It builds the MST one vertex at a time, starting from an arbitrary vertex, and always chooses the smallest edge to add next.

### Raw Algorithm:

1. **Graph Creation**:
   - For each point, calculate the Manhattan distance to every other point and store it as an adjacency list.
   
2. **Prim's Algorithm**:
   - Initialize the MST with a single vertex, chosen arbitrarily from the graph.
   - Grow the MST by one edge: Of the edges that connect the vertices in the MST to the vertices outside it, find a minimum-weight edge.
   - Repeat step 2 until we have covered all vertices.

### Why the Code is Written This Way:

1. **Adjacency List (`adj`)**: This is a dictionary where keys are point indices and values are lists of [distance, index of another point]. We need this structure to get the neighbors of a particular point and the cost to reach them.

2. **Prim's Implementation**:
   - `res` keeps track of the total minimum cost.
   - `visit` is a set to keep track of points/nodes that are already included in the MST.
   - `minH` is a min-heap (implemented using Python's `heapq`) that keeps the edges with the smallest distance on top. This ensures that we always pick the shortest edge next.
   - The `while` loop ensures that we continue the process until all points are covered.
   
3. **Calculating Distance**: The Manhattan distance between two points `(x1, y1)` and `(x2, y2)` is `abs(x1 - x2) + abs(y1 - y2)`.

### Time Complexity:

- Constructing the graph will take O(N^2), where N is the number of points, because we calculate the distance of each point from every other point.
- Prim's algorithm will, in the worst case, examine all edges which are O(N^2) and for each edge, it might perform a logN operation due to the heap operation. Hence, the time complexity is O(N^2 logN).

### Space Complexity:

- The adjacency list, set, and heap will take space proportional to the number of points and edges. So, the space complexity is O(N^2).

### Base Cases and Edge Cases:

**Base Cases**:
- If there's only one point, the cost is 0 because there are no other points to connect to.

**Edge Cases**:
- Multiple points might lie on the same horizontal or vertical line.

### Optimizing Clues:

- A potential optimization can be using Kruskal’s algorithm for MST. Though the worst-case complexity remains the same, in practice, and especially with sparse graphs, Kruskal's might outperform Prim’s.
- Using a Fibonacci heap can improve the time complexity of Prim's algorithm to O(E + V log V), but implementing it is non-trivial.

### Mnemonics to Remember:

- **"Points to Graph"**: Remind yourself that the problem involves converting a set of points to a graph representation.
- **"Prim's for Minimum"**: Use Prim's algorithm to find the MST and get the minimum cost to connect all points.

In essence, the solution leverages graph theory to model the problem as a Minimum Spanning Tree challenge and then employs Prim's algorithm to find the desired connections at minimum cost.