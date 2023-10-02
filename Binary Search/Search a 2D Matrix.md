---

---
---
[LC Link](https://leetcode.com/problems/search-a-2d-matrix/)
### First Principles Thinking:

The fundamental truths about the problem are:
1. The 2D matrix is sorted row-wise and also across rows.
2. The first integer of each row is greater than the last integer of the previous row.
3. The algorithm must have a time complexity of \(O(\log(m \times n))\).

Given these characteristics, a binary search algorithm can be applied. Because the matrix is not just sorted row-wise but also between rows, we can view it as a single sorted list for searching purposes.

### Brute Force Approach:

Even though the problem calls for \(O(\log(m \times n))\), the naive way to solve it would be to scan every element of the 2D matrix.

#### Programming/Coding Principles:

- Use nested loops to go through each row and each column.
- If the target is found, return true.

#### Python Code for Brute Force:

```python
def searchMatrix(matrix, target):
    for row in matrix:
        for element in row:
            if element == target:
                return True
    return False
```

#### Analysis of Time and Space Complexity:

- Time Complexity: \(O(m \times n)\)
- Space Complexity: \(O(1)\)

### Optimized Approach:

#### Intuition and Train of Thought:

We can treat the entire 2D matrix as a sorted list and use binary search on it to find the target value.

#### Programming/Coding Principles:

- Calculate the total number of elements \(m \times n\).
- Perform a binary search treating the 2D matrix as a 1D sorted list.

#### Python Code for Optimized Approach:

```python
def searchMatrix(matrix, target):
    m, n = len(matrix), len(matrix[0])
    left, right = 0, m * n - 1
    
    while left <= right:
        mid = (left + right) // 2
        mid_val = matrix[mid // n][mid % n]
        
        if mid_val == target:
            return True
        elif mid_val < target:
            left = mid + 1
        else:
            right = mid - 1
            
    return False
```

#### Analysis of Time and Space Complexity:

- Time Complexity: \(O(\log(m \times n))\)
- Space Complexity: \(O(1)\)

### Strategies and Techniques:

- Transforming the 2D matrix into a 1D space allows you to apply a binary search algorithm to meet the time complexity requirements.
- Keep track of the size and shape of the 2D matrix to map between 1D and 2D indexes.

This optimized approach can be applied to various other problems that require searching in a sorted 2D matrix, satisfying the given constraints.

---
---

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        ROWS, COLS = len(matrix), len(matrix[0])

        top, bot = 0, ROWS - 1
        while top <= bot:
            row = (top + bot) // 2
            if target > matrix[row][-1]:
                top = row + 1
            elif target < matrix[row][0]:
                bot = row - 1
            else:
                break

        if not (top <= bot):
            return False
        row = (top + bot) // 2
        l, r = 0, COLS - 1
        while l <= r:
            m = (l + r) // 2
            if target > matrix[row][m]:
                l = m + 1
            elif target < matrix[row][m]:
                r = m - 1
            else:
                return True
        return False

```

**Problem Statement:**
The given problem is to search for a target number in a matrix where the rows and columns are both sorted in increasing order. If the number exists in the matrix, return `True`, otherwise return `False`.

**First Principles of Problem-Solving Approach:**

1. **Understand the Problem:** Before diving into the solution, understand the constraints and properties of the matrix. Given rows and columns are sorted, can we exploit this property?
2. **Break Down the Problem:** The problem essentially becomes:
   - Find the row where the target could potentially exist.
   - In that row, use a search technique to find the target.

**Solving Intuition:**
With sorted rows and columns, for any row `i`, if `target > matrix[i][-1]` then target can only be present in a row below `i`. Similarly, if `target < matrix[i][0]`, then target can only be present in a row above `i`. This allows us to perform a binary search on the rows first, and once we've identified the row, we can then perform a binary search on the columns.

**Time/Space Complexity:**

Time: 
- O(log(ROWS)) for binary search on rows.
- O(log(COLS)) for binary search on columns.
Overall: O(log(ROWS) + log(COLS))

Space: O(1) as we are using only constant space.

**Raw Algorithm of the Code:**

1. Define the initial boundaries for rows: top as 0 and bot as the last row index.
2. Perform a binary search on rows to find the potential row where target can be located:
    - If target is greater than the last element of the middle row, move top pointer.
    - If target is less than the first element of the middle row, move bot pointer.
    - If target lies within the range of the current row, break out of the loop.
3. Once the row is identified, define the boundaries for columns: `l` as 0 and `r` as the last column index.
4. Perform a binary search on the columns of the identified row:
    - If target is greater than the middle element, move `l`.
    - If target is less than the middle element, move `r`.
    - If target is found, return `True`.

**Programming Approach:**

1. **Why the while loop:** To perform binary search on rows and columns.
2. **Why the variables:** 
   - `top`, `bot` for binary search on rows.
   - `l`, `r` for binary search on columns.
   - `ROWS`, `COLS` for lengths of dimensions.
3. **Why the condition `if not (top <= bot):`**: To check if the target is not in the matrix. This is based on the final value of `top` and `bot` after the first binary search.
4. **Why no other data structures:** The properties of the matrix itself allow us to efficiently search using binary search without the need for additional data structures.

**Base Cases/Edge Cases:**
- If the matrix has only one row or one column, the algorithm should still work.
- If the target is smaller than the smallest element of the matrix or larger than the largest element, it should quickly return `False`.

**Further Optimizing Clues:**
The current solution is already quite optimal due to the binary search approach. Any further optimization would be micro-optimizations that might not provide significant improvements.

**Mnemonics to Remember the Solution:**
"Row first, then go column-wise. If both agree, the target will rise."
1. First, zero in on the row using binary search.
2. Once row is identified, search within the row.

Note: As before, the `List` type for the function signature is not imported in the code snippet. Remember to import it using `from typing import List` when implementing this in an actual coding environment