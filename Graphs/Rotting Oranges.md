---

---
---
[LC Link](https://leetcode.com/problems/rotting-oranges/)

---
---

```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        q = collections.deque()
        fresh = 0
        time = 0

        for r in range(len(grid)):
            for c in range(len(grid[0])):
                if grid[r][c] == 1:
                    fresh += 1
                if grid[r][c] == 2:
                    q.append((r, c))

        directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]
        while fresh > 0 and q:
            length = len(q)
            for i in range(length):
                r, c = q.popleft()

                for dr, dc in directions:
                    row, col = r + dr, c + dc
                    # if in bounds and nonrotten, make rotten
                    # and add to q
                    if (
                        row in range(len(grid))
                        and col in range(len(grid[0]))
                        and grid[row][col] == 1
                    ):
                        grid[row][col] = 2
                        q.append((row, col))
                        fresh -= 1
            time += 1
        return time if fresh == 0 else -1

```

### Problem Understanding:

Given a grid representing a matrix of oranges, some are fresh (represented by 1) and some are rotten (represented by 2). Every minute, any fresh orange that is adjacent (up, down, left, or right) to a rotten orange becomes rotten. Determine the minimum number of minutes until no fresh oranges are left. If not all oranges can rot, return -1.

### 1st Principles of Problem-Solving:

**Core Principle**: Simulate the rotting process and see if all fresh oranges will eventually rot. If yes, determine the time it takes.

### Solving Intuition:

The problem can be seen as a BFS traversal. Rotten oranges spread the rot to their neighbors. We need to figure out the minimum time to spread the rot to all oranges.

### Raw Algorithm:

1. Count all fresh oranges.
2. Initialize a queue with all rotten oranges (these are our BFS starting points).
3. Use BFS to simulate the rotting of oranges:
    - For each rotten orange, check its neighbors.
    - If a neighbor is fresh, turn it rotten and add it to the queue.
    - After all neighbors are checked for a rotten orange, it's removed from the queue.
4. Repeat the process until there are no fresh oranges or there are no more rotten oranges to process.

### Why The Code Is Written The Way It Is:

**1. Initializing `q` and `fresh`:**
- `q` (queue) holds the current rotten oranges to be processed.
- `fresh` counts the fresh oranges to track how many are left.

**2. Double Loop Initialization:**
- Iterates through the grid to populate `q` with initial rotten oranges and count the fresh ones.

**3. `directions` list:**
- Helps in checking all 4 neighbors of a given cell.

**4. `while` loop:**
- The main BFS loop.
- It continues as long as there are fresh oranges and rotten oranges in the queue to process.

**5. Nested `for` loop within the `while` loop:**
- This ensures that each level of BFS (i.e., each minute) is processed before increasing the time.

**6. `if` statement at the end:**
- After processing all rotten oranges, if there are still fresh oranges left, then it's impossible for them to rot, so return -1.

### Time Complexity:

**O(n*m)** where n is the number of rows and m is the number of columns in the grid. In the worst case, we might have to process every cell.

### Space Complexity:

**O(n*m)** because in the worst-case scenario, all oranges might be rotten, and thus all can be stored in the queue.

### Base Cases and Edge Cases:

**Base Cases**:
- When checking neighbors, ensure they're within the grid's boundaries.
- Only fresh oranges can be turned rotten.

**Edge Cases**:
- If there are fresh oranges left after processing all rotten ones, not all oranges can be rotten, so return -1.

### Optimizing Clues:

- You could use a two-level BFS instead of the `length` variable to reduce some complexity in the code.
- Use a set to store fresh oranges and remove from this set instead of counting, to quickly determine if rotting is complete.

### Mnemonics to Remember:

- **"Rotten Spreads, Time Treads"**: Remember that with every tick of time, the rot spreads. 
- **"Four Neighbors, One Checker"**: Always check all four directions.

The problem essentially asks you to simulate a process. By modeling the spread of rot as a BFS, the solution cleverly captures the dynamics of the problem. Each "level" of the BFS corresponds to a unit of time, making the algorithm intuitive and efficient.