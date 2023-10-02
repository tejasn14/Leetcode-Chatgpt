### First Principles Thinking:

1. **Fundamental Truths**: 
    - We have a collection of candidate numbers `candidates`.
    - Each number can be used only once.

2. **Objective**: 
    - To find all unique combinations that sum up to a given `target`.
  
3. **Constraints**: 
    - 1 <= candidates.length <= 100
    - 1 <= candidates[i] <= 50
    - 1 <= target <= 30

### Brute Force Approach:

#### Explanation:

The brute-force approach would involve generating all combinations of the candidate numbers and checking each one to see if it sums up to the target.

#### Programming/Coding Principles:

- **Raw Algorithm**: 
    1. Sort the input array `candidates`.
    2. Use recursion to generate all combinations and check if they sum to the `target`.

- **Data Structures**: 
    - List to hold the result (list of combinations).
    - Temporary list to hold the current combination.
    
- **Loops**: 
    - Recursion for generating combinations.
  
- **Base Case and Edge Case**: 
    - Base Case: When the current sum equals or exceeds the target, or when no more elements are left.

#### Python Code for the Brute Force Approach:

```python
def combinationSum2(candidates, target):
    res = []
    candidates.sort()
    
    def dfs(target, start, path):
        if target == 0:
            res.append(path)
            return
        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i-1]:
                continue
            if candidates[i] > target:
                break
            dfs(target - candidates[i], i + 1, path + [candidates[i]])
    
    dfs(target, 0, [])
    return res
```

#### Analysis:

- **Time Complexity**: \(O(2^N)\), \(N\) being the length of `candidates`.
- **Space Complexity**: \(O(N)\) for the recursion stack.

### Optimized Approaches:

Due to the nature of the problem and the constraints, the above approach is already efficient.

### Strategies and Techniques:

1. **Recursion for Combinatorial Problems**: Recursion is again a powerful tool for this type of problem.
2. **DFS for Generating Combinations**: DFS is a suitable method for generating the combinations.
3. **Avoid Duplicates**: To avoid duplicate combinations, sort the array and skip over subsequent identical elements.

The method can be generalized to similar problems involving combinations or target sums. Sorting the candidates and then using DFS for traversal are key steps for such problems.


---
---
### Neetcode

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()

        res = []

        def backtrack(cur, pos, target):
            if target == 0:
                res.append(cur.copy())
                return
            if target <= 0:
                return

            prev = -1
            for i in range(pos, len(candidates)):
                if candidates[i] == prev:
                    continue
                cur.append(candidates[i])
                backtrack(cur, i + 1, target - candidates[i])
                cur.pop()
                prev = candidates[i]

        backtrack([], 0, target)
        return res

```

### First Principles Thinking

1. **Objective**: The goal is to find all unique combinations in an array where the candidate numbers sum to a given target.
2. **Constraints**: Each number in the candidate list can only be used once in a combination, and all combinations must be unique.
3. **Invariant**: The sum should equal the target.

#### Intuition and Solving Approach

The problem is similar to its predecessor, `combinationSum`, but with a constraint that each candidate can be used only once, and combinations must be unique. DFS still remains an effective way to explore the solution space, with additional handling for duplicate candidates.

#### Programming Approach

- **Why List `res` and `path`**: 
  - `res` stores all the unique combinations that sum to the target.
  - `path` stores the current combination under evaluation.
  
- **Why Recursion and DFS**: 
  - DFS is an excellent mechanism to enumerate all the possibilities in a decision-making problem.
  
- **Why variables `target` and `start`**: 
  - `target` stores the remaining sum needed to form the target.
  - `start` is the starting index to prevent using the same element multiple times in a combination.
  
- **Why Sorting**: 
  - Sorting enables us to handle duplicates effectively and break early when a candidate is greater than the remaining `target`.
  
- **Why `path + [candidates[i]]`**: 
  - This is an immutable way to add an element to the current path, making it easier to manage the state during recursion.

##### Raw Algorithm

1. Sort the `candidates` array.
2. Initialize an empty list `res` to store the final unique combinations.
3. Define a recursive function `dfs(target, start, path)`:
  1. Base case: If `target` equals 0, append `path` to `res`.
  2. Loop from `start` to `len(candidates)`, considering each candidate for inclusion in the combination:
      1. Skip duplicate candidates.
      2. If the candidate is greater than the remaining `target`, break.
      3. Recursively call `dfs` with the updated target and path.

##### Python Code

```python
def combinationSum2(candidates, target):
    res = []
    candidates.sort()
    
    def dfs(target, start, path):
        if target == 0:
            res.append(path)
            return
        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i-1]:
                continue
            if candidates[i] > target:
                break
            dfs(target - candidates[i], i + 1, path + [candidates[i]])
    
    dfs(target, 0, [])
    return res
```

##### Time and Space Complexity

- **Time Complexity**: \(O(2^n \times n \log n)\) — \(2^n\) is the number of recursive calls (worst case), \(n \log n\) is for sorting.
- **Space Complexity**: \(O(n)\) — mainly for the storage of the `res` list and the recursion stack.

#### Further Optimization Clues

- The existing algorithm is already quite optimized due to early breaking conditions and the handling of duplicates.

#### Mnemonics to Remember

- "Sort, Skip, Stop": Sort the candidates, skip duplicates, and stop the loop if the candidate is greater than the target.
  
#### Generalizable Strategies

- **DFS for Decision Trees**: DFS is a powerful tool for exploring decision-based problems.
- **Early Break**: Whenever you have sorted data, look for opportunities to terminate loops early.

By understanding the code at this granular level, you'll be well-equipped to tackle similar problems. This approach aligns with your goal to understand every aspect of the solution, from problem-solving intuition to implementation details.