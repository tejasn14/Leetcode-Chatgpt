---

---
---
[LC Link](https://leetcode.com/problems/number-of-islands/)

---
---

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid or not grid[0]:
            return 0

        islands = 0
        visit = set()
        rows, cols = len(grid), len(grid[0])

        def dfs(r, c):
            if (
                r not in range(rows)
                or c not in range(cols)
                or grid[r][c] == "0"
                or (r, c) in visit
            ):
                return

            visit.add((r, c))
            directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]
            for dr, dc in directions:
                dfs(r + dr, c + dc)

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1" and (r, c) not in visit:
                    islands += 1
                    dfs(r, c)
        return islands


# BFS Version From Video
class SolutionBFS:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        visited=set()
        islands=0

         def bfs(r,c):
             q = deque()
             visited.add((r,c))
             q.append((r,c))
           
             while q:
                 row,col = q.popleft()
                 directions= [[1,0],[-1,0],[0,1],[0,-1]]
               
                 for dr,dc in directions:
                     r,c = row + dr, col + dc
                     if (r) in range(rows) and (c) in range(cols) and grid[r][c] == '1' and (r ,c) not in visited:
                       
                         q.append((r , c ))
                         visited.add((r, c ))

         for r in range(rows):
             for c in range(cols):
               
                 if grid[r][c] == "1" and (r,c) not in visited:
                     bfs(r,c)
                     islands +=1 

         return islands


```

Alright, let's dive into this problem, which is about counting the number of distinct islands on a given grid.

### Problem-Solving Approach:

**Understanding the Problem**:
You're given a 2D grid where "1" represents land and "0" represents water. Groups of connected "1"s form islands. Your task is to count these islands.

### Solving Intuition:

**DFS (Depth First Search)**: Start from a land cell ("1") and mark it as visited. Then, traverse all its neighbors that are land. Continue this process until there's no neighboring land. Each DFS invocation finds one distinct island.

**BFS (Breadth First Search)**: Start from a land cell ("1") and add it to a queue. Mark it as visited. Then, for each cell in the queue, explore its neighboring land cells. Each BFS invocation finds one distinct island.

### Raw Algorithm:

1. **DFS**:
   - For each cell in the grid:
     - If it's a "1" and not visited, initiate a DFS traversal from that cell.
     - Increment island count for every DFS initiation.

2. **BFS**:
   - For each cell in the grid:
     - If it's a "1" and not visited, initiate a BFS traversal from that cell.
     - Increment island count for every BFS initiation.

### Why The Code Is Written The Way It Is:

**DFS Version**:
1. **`visit` set**: It's a set used to keep track of the cells we've visited. Using sets gives O(1) complexity for insertion and lookup.
2. **`dfs(r, c)` function**: Initiates a depth-first traversal from cell `(r, c)`. If a cell is not land or already visited, the function returns. Otherwise, it marks the cell as visited and explores its neighbors.
3. **Why `directions`?**: To generalize the movement to neighbors (up, down, left, right), we use this array of directions.

**BFS Version**:
1. **`visited` set**: Similar to the DFS version, it keeps track of visited cells.
2. **`bfs(r, c)` function**: Initiates a breadth-first traversal. It starts with a cell, adds it to a queue, and explores its neighbors. As cells are explored, they're dequeued.
3. **`q` (queue)**: It's used to keep track of cells to be explored. BFS works in "layers", expanding outwards, and a queue is the ideal data structure for this.

### Time Complexity:

Both approaches run in **O(M x N)** where M and N are the number of rows and columns respectively. Each cell is visited once.

### Space Complexity:

**DFS**: O(M x N) in the worst case, i.e., grid is filled with lands, and our DFS goes by depth.
**BFS**: O(min(M, N)) because, in the worst case, a queue will be filled during BFS traversal. This happens when the width or height of the island is the largest.

### Base Cases and Edge Cases:

1. **Base Case inside `dfs` and `bfs`**: If a cell is not land or already visited, the function should return.
2. **Edge Cases**: The solution handles when grid is empty (`if not grid or not grid[0]: return 0`).

### Optimizing Clues:

1. **Union-Find/Disjoint Set Union**: Another approach to solve this is to use Union-Find. Every cell can be treated as a node, and we can connect neighboring land cells. The number of distinct sets will be our answer.
2. **`directions` array**: We use this array to generalize movements. An alternative could be explicitly writing out the four directions.

### Mnemonics to Remember:

1. **"Land to Explore"**: The essence of the problem is to find pieces of land and explore them entirely.
2. **"DFS and BFS"**: Both methods aim to explore the entirety of a piece of land from a starting point.
3. **"Visited Set"**: Always remember, whether it's DFS or BFS, you need a way to track where you've been to avoid infinite loops.

Both the DFS and BFS approaches have their merits. DFS is more compact and uses the call stack, while BFS uses an explicit queue and processes cells in layers. Both effectively solve the problem.