---

---
---
[LC Link](https://leetcode.com/problems/pacific-atlantic-water-flow/)

---
---

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        ROWS, COLS = len(heights), len(heights[0])
        pac, atl = set(), set()

        def dfs(r, c, visit, prevHeight):
            if (
                (r, c) in visit
                or r < 0
                or c < 0
                or r == ROWS
                or c == COLS
                or heights[r][c] < prevHeight
            ):
                return
            visit.add((r, c))
            dfs(r + 1, c, visit, heights[r][c])
            dfs(r - 1, c, visit, heights[r][c])
            dfs(r, c + 1, visit, heights[r][c])
            dfs(r, c - 1, visit, heights[r][c])

        for c in range(COLS):
            dfs(0, c, pac, heights[0][c])
            dfs(ROWS - 1, c, atl, heights[ROWS - 1][c])

        for r in range(ROWS):
            dfs(r, 0, pac, heights[r][0])
            dfs(r, COLS - 1, atl, heights[r][COLS - 1])

        res = []
        for r in range(ROWS):
            for c in range(COLS):
                if (r, c) in pac and (r, c) in atl:
                    res.append([r, c])
        return res

```

### Problem Understanding:

This problem asks us to determine which cells in a given matrix of heights can flow water to both the Pacific and Atlantic oceans. The Pacific ocean touches the top and left borders of the matrix, while the Atlantic ocean touches the bottom and right borders.

### 1st Principles of Problem-Solving:

**Core Principle**: Identify cells from which water can flow to the top/left borders (Pacific) and to the bottom/right borders (Atlantic). Cells that can flow to both oceans meet the criteria.

### Solving Intuition:

The immediate intuition is that we should start our search from the borders where the oceans are. This is a reversal in perspective: instead of checking if water from each cell can reach the oceans, we're checking if water from the oceans can reach each cell. This guarantees that if we reach a cell from both the Pacific and Atlantic starting points, it's a valid cell.

### Raw Algorithm:

1. From each cell on the borders, use DFS to traverse the matrix.
2. The traversal is allowed to move to neighboring cells if the height of the neighboring cell is equal to or greater than the current cell.
3. For each traversal starting from the Pacific, mark visited cells. Repeat for the Atlantic.
4. At the end, find cells that were visited in both traversals.

### Why The Code Is Written The Way It Is:

**1. `pac` and `atl` sets**:
- **Purpose**: Keep track of cells that can be reached from the Pacific and Atlantic oceans, respectively.
- **Why sets?**: Sets allow O(1) average time complexity for adding, deleting, and checking the existence of items.

**2. `dfs` function**:
- **Purpose**: To traverse the matrix from a given starting point.
- **Base Condition**: The function immediately returns if it goes out of bounds, if it encounters a cell with a height less than `prevHeight`, or if it revisits a cell. This ensures we're only moving to cells of equal or greater height, and we're not stuck in loops.

**3. Looping through rows and columns**:
- This is done to start the DFS from every cell on the border (the edge cells of the matrix). We do this twice for each ocean: Once for the Pacific and once for the Atlantic.

**4. Final Loop to find common cells**:
- Iterates through each cell in the matrix and checks if a cell has been visited from both the Pacific and Atlantic starting points. If yes, it's added to the result.

### Time Complexity:

**O(ROWS * COLS)** - We're essentially performing a DFS from each border cell. In the worst case, we could visit all cells in the matrix twice (once from Pacific starts and once from Atlantic starts).

### Space Complexity:

**O(ROWS * COLS)** - We might need to store all cells in the `pac` and `atl` sets. Moreover, the depth of the recursive stack in DFS contributes to this space complexity.

### Base Cases and Edge Cases:

**Base Cases**: 
- The `dfs` function contains conditions to handle out-of-bounds cases and also to ensure that we don't traverse to cells with lesser height.

**Edge Cases**: 
- Cells directly on the borders are trivially reachable by their adjacent ocean. 

### Optimizing Clues:

- A BFS approach could also be used in place of DFS. Instead of using recursion, a queue would be used to process each cell's neighbors sequentially.

### Mnemonics to Remember:

- **"Two Oceans, One Start"**: Always remember that we're starting from the borders where the oceans touch and checking where the water can go.
- **"Flow Uphill, Not Down"**: In the traversal, water can move to cells of equal or greater height but not to cells of lower height.

Overall, the solution cleverly reverses the problem. Instead of seeing where each cell's water flows to, it sees from where each ocean's water can come from. This guarantees that if a cell can be reached from both oceans' starting points, it meets the criteria.