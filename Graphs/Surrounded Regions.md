---

---
---
[LC Link](https://leetcode.com/problems/surrounded-regions/)

---
---

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        ROWS, COLS = len(board), len(board[0])

        def capture(r, c):
            if r < 0 or c < 0 or r == ROWS or c == COLS or board[r][c] != "O":
                return
            board[r][c] = "T"
            capture(r + 1, c)
            capture(r - 1, c)
            capture(r, c + 1)
            capture(r, c - 1)

        # 1. (DFS) Capture unsurrounded regions (O -> T)
        for r in range(ROWS):
            for c in range(COLS):
                if board[r][c] == "O" and (r in [0, ROWS - 1] or c in [0, COLS - 1]):
                    capture(r, c)

        # 2. Capture surrounded regions (O -> X)
        for r in range(ROWS):
            for c in range(COLS):
                if board[r][c] == "O":
                    board[r][c] = "X"

        # 3. Uncapture unsurrounded regions (T -> O)
        for r in range(ROWS):
            for c in range(COLS):
                if board[r][c] == "T":
                    board[r][c] = "O"

```

### Problem Understanding:

The problem asks to "capture" all regions in a board that are completely surrounded by 'X's. However, a region is not captured if it touches the board's border.

### 1st Principles of Problem-Solving:

**Core Principle**: Determine regions that are surrounded by 'X's but exempt regions touching the border.

### Solving Intuition:

Instead of starting from every 'O' and determining if it's surrounded, it's simpler to reverse our thinking. If any 'O' on the border cannot be captured, then any 'O' connected to it also can't be captured. This insight leads to a 3-phase approach:

1. **DFS from border 'O's**: This will mark all 'O's connected to the border.
2. **Capture all standalone 'O's**: Convert them to 'X' as they're definitely surrounded.
3. **Revert the markings from step 1**.

### Raw Algorithm:

1. Use DFS from all 'O's on the border. Change these 'O's to temporary 'T's. This marks all 'O's connected to the border.
2. Traverse the entire board. Change all remaining 'O's to 'X'.
3. Traverse the entire board again. Convert all 'T's back to 'O's.

### Why The Code Is Written The Way It Is:

**1. `capture` function**:
- **Purpose**: To traverse the board using DFS from a starting 'O'.
- **Base Condition**: The function stops its traversal if out-of-bounds, or if the current cell isn't 'O'.

**2. 3 Iterations over the board**:
- **1st Iteration**: To find 'O's on the border and apply DFS from them.
- **2nd Iteration**: To capture all unmarked 'O's.
- **3rd Iteration**: To revert 'T's back to 'O's.

**3. 'T' as a placeholder**:
- 'T' isn't one of the original board characters. It temporarily marks 'O's that are exempt from capture, so they're not affected in the 2nd iteration.

### Time Complexity:

**O(ROWS * COLS)** - At worst, every cell is visited three times: once to mark it (if needed), once to capture it (if needed), and once to revert the marking.

### Space Complexity:

**O(ROWS * COLS)** - In the worst-case scenario, the recursive DFS (`capture` function) will place `ROWS * COLS` calls on the call stack. 

### Base Cases and Edge Cases:

**Base Cases**: 
- The `capture` function handles cases where the DFS should stop: moving out of bounds or finding a non-'O' cell.

**Edge Cases**: 
- 'O's on the borders should not be captured. They are the starting points for the DFS.

### Optimizing Clues:

- Instead of 3 full iterations over the board, we can attempt to do all tasks in fewer passes.
- A BFS approach using a queue can replace the DFS to avoid possible stack overflow for large boards.

### Mnemonics to Remember:

- **"Border First, Inside Next"**: Start by checking 'O's on the border, then handle the rest.
- **"O on the edge keeps the X at bay"**: An 'O' on the border prevents its connected region from being captured.

The solution cleverly tackles the problem by handling the straightforward cases first ('O's on the border) and then managing the 'O's inside based on those results. This is a nice example of simplifying a problem by breaking it down into manageable steps and handling each step efficiently.