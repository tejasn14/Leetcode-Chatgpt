---

---
---
[LC Link](https://leetcode.com/problems/valid-sudoku/)
### First Principles Thinking

The problem is to determine whether a given 9x9 Sudoku board is valid according to the rules of Sudoku. The constraints and requirements are as follows:

1. The board is 9x9, making it a fixed-size 2D array.
2. Only filled cells need to be validated. 
3. Each row, each column, and each 3x3 sub-grid must contain the digits 1-9 without repetition.

The fundamental question is how to effectively and efficiently verify all these conditions in a single traversal of the board.

### Brute Force Approach

#### Explanation

For each filled cell in the board:

- Check its row to see if the value is repeated.
- Check its column to see if the value is repeated.
- Check its 3x3 sub-grid to see if the value is repeated.

#### Programming/Coding Principles

- Nested loops can be used to traverse the board and perform checks.
  
#### Python Code

```python
def isValidSudoku(board):
    for i in range(9):
        for j in range(9):
            if board[i][j] != ".":
                num = board[i][j]
                
                # Check row
                if board[i].count(num) > 1:
                    return False
                
                # Check column
                if sum(1 for row in board if row[j] == num) > 1:
                    return False
                
                # Check 3x3 sub-grid
                row_start, col_start = 3 * (i // 3), 3 * (j // 3)
                if sum(1 for x in range(row_start, row_start + 3) for y in range(col_start, col_start + 3) if board[x][y] == num) > 1:
                    return False
    return True
```

#### Time and Space Complexity Analysis

- Time Complexity: \(O(9 \times 9 \times (9 + 9 + 9)) = O(729 \times 3) = O(2187)\) which is effectively \(O(1)\) as it's a constant time operation.
- Space Complexity: \(O(1)\)

### Optimized Approach

#### Intuition

- Use sets to keep track of seen numbers for rows, columns, and 3x3 sub-grids to eliminate the need for repetitive checks.

#### Transition from Brute Force

- Use data structures to store intermediate states and remove repetitive calculations.

#### Programming/Coding Principles

- Sets can be used to store seen numbers effectively.

#### Python Code

```python
def isValidSudoku(board):
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]
    
    for i in range(9): 
        for j in range(9):
            num = board[i][j]
            if num == ".":
                continue
            
            box_index = (i // 3) * 3 + j // 3
            
            if (num in rows[i]) or (num in cols[j]) or (num in boxes[box_index]):
                return False
            
            rows[i].add(num)
            cols[j].add(num)
            boxes[box_index].add(num)
            
    return True
```

#### Time and Space Complexity Analysis

- Time Complexity: \(O(9 \times 9) = O(81) = O(1)\) (constant time)
- Space Complexity: \(O(9 + 9 + 9) = O(27) = O(1)\) (constant space)

### Strategies and Techniques

1. **Data Structure Utilization**: Utilizing data structures like sets can significantly improve time complexity by avoiding repetitive calculations.
2. **Spatial Locality**: The 3x3 sub-grid is calculated based on the division and multiplication of indices, which reduces the complexity and number of operations required.

This approach is generalized enough that you can extend it to validate any \(N \times N\) sudoku grid where \(N\) is a perfect square. Just change the hard-coded 9's to \(N\) and update the sub-grid size accordingly.

---
---

### Neetcode
```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        cols = collections.defaultdict(set)
        rows = collections.defaultdict(set)
        squares = collections.defaultdict(set)  # key = (r /3, c /3)

        for r in range(9):
            for c in range(9):
                if board[r][c] == ".":
                    continue
                if (
                    board[r][c] in rows[r]
                    or board[r][c] in cols[c]
                    or board[r][c] in squares[(r // 3, c // 3)]
                ):
                    return False
                cols[c].add(board[r][c])
                rows[r].add(board[r][c])
                squares[(r // 3, c // 3)].add(board[r][c])

        return True

```


### First Principles Thinking

1. **Fundamental Truths**: A Sudoku board is valid if each row, each column, and each of the nine 3x3 grids contain all of the digits from 1 to 9, without any repetition.

2. **Constraints**: 
   - We are given a 9x9 board.
   - Cells contain either a digit from '1' to '9' or a '.' (indicating an empty cell).

3. **Building Up**: 
   - To validate, we need to check each row, column, and 3x3 square independently for violations.

#### Intuition and Solving Intuition

1. **Pattern Recognition**: Duplicates in a row, column, or 3x3 square make the Sudoku invalid.

2. **Optimization Insight**: As we read the board, we can remember digits in each row, column, and square using sets.

3. **Time Complexity Constraint**: O(81) or effectively O(1), as the board size is fixed.

### Explanation of Solution

#### Programming/Coding Principles

1. **Raw Algorithm**:
    - Use separate sets for each row, column, and 3x3 square.
    - Traverse the board. For each cell, check:
      - If the value is already in its respective row, column, or 3x3 square set.
      - If so, return False immediately.
    - If traversal completes without finding duplicates, return True.

2. **Data Structure**: 
    - Default Dictionary of Sets (`cols`, `rows`, `squares`): To keep track of seen digits for each row, column, and square.

3. **Looping Structure**: 
    - Two nested `for` loops: To traverse each row and column on the board.

4. **Variables**: 
    - `cols`, `rows`, `squares`: To hold sets for each row, column, and square.
    - `r` and `c`: Loop variables for rows and columns.
  
5. **Update Criteria**: 
    - Update sets based on encountered digits.

6. **Base Case and Edge Case**: 
    - The function immediately returns False if a duplicate is found.

#### Python Code

```python
from collections import defaultdict
from typing import List

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        cols = defaultdict(set)
        rows = defaultdict(set)
        squares = defaultdict(set)

        for r in range(9):
            for c in range(9):
                if board[r][c] == ".":
                    continue
                if (
                    board[r][c] in rows[r]
                    or board[r][c] in cols[c]
                    or board[r][c] in squares[(r // 3, c // 3)]
                ):
                    return False
                cols[c].add(board[r][c])
                rows[r].add(board[r][c])
                squares[(r // 3, c // 3)].add(board[r][c])

        return True
```

#### Time and Space Complexity

- **Time Complexity**: \(O(1)\) — Fixed size board (81 cells).
- **Space Complexity**: \(O(1)\) — Sets have fixed, maximum sizes due to Sudoku rules (each set can contain at most 9 elements).

### Strategies and Techniques

1. **Set Membership Testing**: Use sets for quick membership testing. This is especially useful when duplicates invalidate the solution.

2. **Early Exit**: If a rule violation is found, exit immediately without further traversal.

### Mnemonics

1. **"Three Set Rule"**: Remember that three sets (for rows, columns, and squares) are used to validate each dimension of the board independently.
  
2. **"Row, Column, Square"**: To remember the sequence of validations: check row, then column, then 3x3 square.

3. **"Shortcut to Square"**: The formula `(r // 3, c // 3)` is the key to identify which 3x3 square a cell belongs to.

By understanding the core principle of validation here, you can apply the same 'set-based validation' idea to similar constraint-validation problems.
