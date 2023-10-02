---

---
---
[LC Link](https://leetcode.com/problems/max-area-of-island/)

---
---

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        ROWS, COLS = len(grid), len(grid[0])
        visit = set()

        def dfs(r, c):
            if (
                r < 0
                or r == ROWS
                or c < 0
                or c == COLS
                or grid[r][c] == 0
                or (r, c) in visit
            ):
                return 0
            visit.add((r, c))
            return 1 + dfs(r + 1, c) + dfs(r - 1, c) + dfs(r, c + 1) + dfs(r, c - 1)

        area = 0
        for r in range(ROWS):
            for c in range(COLS):
                area = max(area, dfs(r, c))
        return area

```

### Understanding the Problem:

The problem requires us to find the maximum area of an island in a grid. Each cell in the grid is either a "water" (represented by 0) or a "land" (represented by 1). An island is represented by contiguous lands horizontally or vertically, and the area of an island is the number of land cells that make it up.

### 1st Principles of Problem-Solving:

**Core Principle**: Traverse the grid and whenever land is found, explore its entire extent to determine the size of the island it belongs to.

### Solving Intuition:

One immediate intuition here is a traversal problem. Whenever a land cell (`1`) is found, explore its neighbors in all four cardinal directions (North, South, East, West). The number of cells visited gives the area of that island. 

### Raw Algorithm:

1. Iterate over every cell in the grid.
2. If the cell is land and not yet visited:
   a. Start a Depth First Search (DFS) to explore the full extent of the island.
   b. Keep track of visited cells to avoid double counting or endless looping.
3. Return the maximum area found.

### Why The Code Is Written The Way It Is:

**1. `visit` set**:
- **Purpose**: Keeps track of the cells that have already been visited. Helps in avoiding counting a cell more than once and avoids infinite loops.
- **Why set?**: Checking existence, adding, or removing an element from a set is O(1) on average.

**2. `dfs(r, c)` function**:
- **DFS**: The recursive nature of this function allows for a depth-first traversal. Upon encountering land, it immediately dives deeper in all cardinal directions.
- **Base Condition**: Before exploring further, the function checks if the cell is out of bounds, is water, or has been visited before. If any of these conditions hold true, exploration is stopped (by returning 0).
- **Recursive Call**: For each valid land cell, the function contributes `1` to the area (for the cell itself) and then explores the neighboring cells.

**3. Loop through `ROWS` and `COLS`**:
- **Purpose**: This ensures every cell in the grid is inspected.
- **Max-Area Calculation**: As we explore each land cell and its neighboring islands, we update the `area` variable to remember the largest island found so far.

### Time Complexity:

**O(ROWS * COLS)** - In the worst case, we'll need to visit all cells in the grid once.

### Space Complexity:

**O(ROWS * COLS)** - In the worst-case scenario (like a grid filled with land), the `visit` set can grow to the size of the grid. The recursive call stack in DFS can also contribute to the space complexity.

### Base Cases and Edge Cases:

**Base Case**: 
- The `dfs` function returns `0` if it hits water, an already visited cell, or goes out of bounds. This is essential for ensuring that the function doesn't infinitely recurse or explore irrelevant cells.

### Optimizing Clues:

- A Breadth-First Search (BFS) approach with a queue can also be employed here. Instead of recursively visiting neighbors, you'd enqueue them and process sequentially.
- In certain cases, changing the data structure (like using a list to represent `visit`) might alter performance, but the set is generally optimal for this problem due to O(1) lookups.

### Mnemonics to Remember:

- **"Dive Deep, Count Land"**: As you find a piece of land, dive deep into its surroundings to find its size.
- **"Visit to Avoid a Revisit"**: Use the `visit` set to remember where you've been, ensuring efficiency and correctness.

In essence, this solution dives deep into every piece of land it finds, exploring its full extent to determine the size of the island it belongs to, always remembering where it's been to ensure no double-counting.