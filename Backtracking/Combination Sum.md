
### First Principles Thinking:

1. **Fundamental Truths**: 
    - We have an array `candidates` containing distinct integers.
    - We have a target integer `target`.
    - The same number may be chosen from `candidates` an unlimited number of times.
  
2. **Objective**: 
    - To return a list containing all unique combinations of integers from `candidates` that sum to `target`.

3. **Constraints**: 
    - 1 <= candidates.length <= 30
    - 2 <= candidates[i] <= 40
    - 1 <= target <= 40
    - All elements of `candidates` are distinct.

### Brute Force Approach:

#### Explanation:

A naive approach would involve trying out all combinations of numbers from the `candidates` array to find combinations that sum to `target`. We can achieve this using recursion.

#### Programming/Coding Principles:

- **Raw Algorithm**: 
    1. Start with an empty list to represent the current combination.
    2. Recursively explore each candidate, adding it to the current combination and checking if the target is reached.

- **Data Structures**: 
    - List to hold the result (list of combinations).
    - Temporary list to hold the current combination.

- **Loops**: 
    - Recursion takes care of iteration.

- **Base Case and Edge Case**: 
    - Base Case: Target is zero.
    - Edge Case: Target is negative.
  
#### Python Code for the Brute Force Approach:

```python
def combinationSum(candidates, target):
    res = []
    
    def dfs(target, path, start):
        if target == 0:
            res.append(path[:])
            return
        if target < 0:
            return
        for i in range(start, len(candidates)):
            dfs(target - candidates[i], path + [candidates[i]], i)
    
    dfs(target, [], 0)
    return res
```

#### Analysis:

- **Time Complexity**: \(O(2^n)\)
- **Space Complexity**: \(O(n)\) for the recursion stack

### Optimized Approaches:

#### Intuition and Train of Thought for Optimization:

We can keep track of the current sum of the elements in the combination to avoid recomputation. Also, we can sort the candidates array to break out of the loop when the target becomes negative, thereby avoiding unnecessary recursive calls.

#### Step-by-step Transition:

1. Sort the candidates array.
2. Break the loop if the current candidate makes the target negative.

#### Programming/Coding Principles:

- **Raw Algorithm**: 
    1. Same as the brute force but with optimizations.

- **Data Structures**: 
    - Same as the brute force.

#### Python Code for the Optimized Solution:

```python
def combinationSum(candidates, target):
    res = []
    candidates.sort()
    
    def dfs(target, path, start):
        if target == 0:
            res.append(path[:])
            return
        for i in range(start, len(candidates)):
            if candidates[i] > target:
                break
            dfs(target - candidates[i], path + [candidates[i]], i)
    
    dfs(target, [], 0)
    return res
```

#### Analysis:

- **Time Complexity**: \(O(2^n)\) (worst case)
- **Space Complexity**: \(O(n)\) for the recursion stack

### Strategies and Techniques:

1. **Recursion for Combinatorial Problems**: Recursive solutions can be useful for generating combinatorial sets.
2. **Early Termination for Optimization**: Sorting the candidates and using early termination can reduce the number of recursive calls.

These strategies can be generalized to other problems that require generating combinations or exploring all possibilities.

---
#### Neetcode

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []

        def dfs(i, cur, total):
            if total == target:
                res.append(cur.copy())
                return
            if i >= len(candidates) or total > target:
                return

            cur.append(candidates[i])
            dfs(i, cur, total + candidates[i])
            cur.pop()
            dfs(i + 1, cur, total)

        dfs(0, [], 0)
        return res

```

### First Principles Thinking

1. **Objective**: The goal is to find all unique combinations in an array where the candidate numbers sum to a given target.
2. **Constraints**: You can use each candidate number as many times as you want, which implies repetition of numbers in combinations is allowed.
3. **Invariant**: The sum should equal the target.

#### Intuition and Solving Approach

The intuitive approach to solving this problem is through recursive DFS. For each number in the candidate list, we have two choices:

1. Include the number in the current combination.
2. Exclude it and move on to the next number.

By recursively making these decisions, we can explore all possible combinations.

#### Programming Approach

- **Why List `res` and `cur`**: 
  - `res` holds all possible combinations that sum to `target`. 
  - `cur` holds the current combination under consideration.
  
- **Why Recursion and DFS**: 
  - DFS effectively captures the decision-making process—include or exclude an element.
  
- **Why variables `i` and `total`**: 
  - `i` is the index of the current candidate to consider.
  - `total` holds the current sum of the numbers in `cur`.

- **Why `cur.copy()`**: 
  - A copy of `cur` is stored in `res` to preserve the current state.

##### Raw Algorithm

1. Initialize an empty list `res` to store the final combinations.
2. Define a recursive function `dfs(i, cur, total)`:
  1. Base case: If `total` equals `target`, append a copy of `cur` to `res`.
  2. Base case: If `i` is out of bounds or `total > target`, return.
  3. Include the current candidate (`candidates[i]`) in `cur` and update `total`.
  4. Call `dfs(i, cur, total + candidates[i])` because repetition is allowed.
  5. Remove the last element from `cur` (backtrack) and call `dfs(i + 1, cur, total)`.

##### Python Code

```python
from typing import List

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        def dfs(i, cur, total):
            if total == target:
                res.append(cur.copy())
                return
            if i >= len(candidates) or total > target:
                return
            cur.append(candidates[i])
            dfs(i, cur, total + candidates[i])
            cur.pop()
            dfs(i + 1, cur, total)
        dfs(0, [], 0)
        return res
```

##### Time and Space Complexity

- **Time Complexity**: \(O(2^n \times n)\) — \(2^n\) represents the tree of recursive calls, and \(n\) is for the `.copy()` operation.
- **Space Complexity**: \(O(n)\) — Storing the `res` list and temporary list `cur`.

#### Further Optimization Clues

- Sorting `candidates` may provide early stopping conditions in some cases, although it doesn't change the time complexity.
  
#### Mnemonics to Remember

- "Include or Exclude": At each step, you either include or exclude a candidate number.
- "Total for Target": Keep an eye on the `total` to check if it matches the `target`.
  
#### Generalizable Strategies

- **DFS for Decision Making**: Whenever you have a problem involving multiple decisions, DFS is a useful approach.
- **[[Backtracking]]**: When you include an element, remember to backtrack by popping it out if it doesn't lead to a solution.

This follows the systematic approach you're aiming to adopt, helping you understand the problem from its foundational principles up to its coding intricacies.