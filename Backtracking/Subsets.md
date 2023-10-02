[LC Link](https://leetcode.com/problems/subsets/)
### First Principles Thinking:

1. **Fundamental Truths**: 
   - We have an array `nums` containing unique integers.
   - We need to generate all possible subsets (power set).

2. **Objective**: 
   - To return a list containing all the subsets of `nums`.

3. **Constraints**: 
   - 1 <= nums.length <= 10
   - -10 <= nums[i] <= 10
   - All numbers are unique.

### Brute Force Approach:

#### Explanation:

One way to generate all subsets is to use recursion. We start with an empty set and then, for each element in the array, we generate new subsets by adding that element to all existing subsets.

#### Programming/Coding Principles:

- **Raw Algorithm**: 
  1. Initialize the power set with an empty set.
  2. Recursively add each element to all existing subsets and add these new subsets to the power set.
  
- **Data Structures**: 
  - List to hold the subsets.

- **Loops**: 
  - No explicit loops, recursion takes care of it.

- **Base Case and Edge Case**: 
  - Base Case: Reached the end of the array.
  
#### Python Code for the Brute Force Approach:

```python
def subsets(nums):
    res = [[]]
    
    for num in nums:
        new_sets = []
        for curr_set in res:
            new_set = curr_set + [num]
            new_sets.append(new_set)
        res += new_sets
        
    return res
```

#### Analysis:

- **Time Complexity**: \(O(2^n)\), where \(n\) is the length of the input array.
- **Space Complexity**: \(O(2^n)\)

### Optimized Approaches:

#### Intuition and Train of Thought for Optimization:

Instead of generating new lists in the recursive approach, we can use backtracking to generate all subsets efficiently.

#### Step-by-step Transition:

1. Use a helper function for the recursive backtracking.
2. At each step, either include or exclude the current element.

#### Programming/Coding Principles:

- **Raw Algorithm**: 
  1. Use backtracking to explore each possibility.
  
- **Data Structures**: 
  - List to hold the subsets and a temporary list for the current subset.

- **Loops**: 
  - No explicit loops, recursion takes care of it.

- **Base Case and Edge Case**: 
  - Base Case: Reached the end of the array.
  
#### Python Code for the Optimized Solution:

```python
def subsets(nums):
    res = []
    
    def backtrack(start=0, curr=[]):
        if start == len(nums):
            res.append(curr[:])
            return
        # Include the current element
        curr.append(nums[start])
        backtrack(start + 1, curr)
        curr.pop()
        # Exclude the current element
        backtrack(start + 1, curr)
        
    backtrack()
    return res
```

#### Analysis:

- **Time Complexity**: \(O(2^n)\), where \(n\) is the length of the input array.
- **Space Complexity**: \(O(2^n)\)

### Strategies and Techniques:

1. **Recursion for Combinatorial Problems**: Recursive solutions can be useful for generating combinatorial sets.
2. **Backtracking for Optimization**: Using backtracking can reduce memory usage by reusing the same data structure to build the solution.

These strategies can be generalized to other problems that require generating combinatorial sets or exploring all possibilities.


---
### Neetcode
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []

        subset = []

        def dfs(i):
            if i >= len(nums):
                res.append(subset.copy())
                return
            # decision to include nums[i]
            subset.append(nums[i])
            dfs(i + 1)
            # decision NOT to include nums[i]
            subset.pop()
            dfs(i + 1)

        dfs(0)
        return res

```

### First Principles Analysis

The problem at hand is to find all possible subsets of a given list `nums` of integers. Let's identify the foundational truths of the problem:

1. A subset is any combination of elements from the array, including the empty set and the set itself.
2. For an array of size \( n \), there are \( 2^n \) subsets.
3. We can either choose to include an element in the subset or exclude it, which gives rise to a recursive structure.

#### Intuition and Approach

The problem can be tackled through recursion, mimicking a Depth-First Search (DFS). At each index \( i \) in the array, we have two choices: to include the element at that index or not to include it. We can build up all subsets through these decisions.

##### Programming Approach

- **Why List `res` and `subset`**: 
  - `res` collects all the subsets, and `subset` holds the current subset being generated.
- **Why Recursion and DFS**: 
  - Recursive DFS is used to make the decision to include or exclude each element. We call `dfs(i + 1)` to proceed to the next decision point.
- **Why `subset.copy()`**: 
  - It is necessary to store a copy of the `subset` list; otherwise, the list will be modified in subsequent iterations.
  
##### Raw Algorithm

1. Create an empty list `res` to store all subsets and another empty list `subset` to store the current subset.
2. Define a recursive function `dfs(i)`:
    1. If \( i \) equals or exceeds the length of `nums`, append a copy of `subset` to `res` and return.
    2. Make a decision to include `nums[i]` in `subset`, call `dfs(i + 1)`.
    3. Make a decision NOT to include `nums[i]` in `subset`, call `dfs(i + 1)`.

##### Python Code

```python
from typing import List

class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        subset = []

        def dfs(i):
            if i >= len(nums):
                res.append(subset.copy())
                return
            # decision to include nums[i]
            subset.append(nums[i])
            dfs(i + 1)
            # decision NOT to include nums[i]
            subset.pop()
            dfs(i + 1)

        dfs(0)
        return res
```

##### Time and Space Complexity

- Time Complexity: \(O(2^n \times n)\) — There are \(2^n\) subsets and each copy operation takes \(O(n)\).
- Space Complexity: \(O(n)\) — Storing the `res` list and `subset`.

#### Further Optimization Clues

- One could use bit manipulation to solve this problem in a non-recursive way, but it won't drastically reduce the time complexity.

#### Mnemonics to Remember

- "Two Choices": At each index, remember there are two choices, to include or not to include the element.
- "DFS Decisions": DFS is the tool to execute those choices.
- "Copy to Store": Make a copy of `subset` to store in `res`.

#### Generalizable Strategies

1. **DFS for Combinatorial Choices**: Whenever you need to make multiple decisions (like include or exclude), DFS is a powerful technique.
2. **Use of Temporary Data Structures**: When building combinations, a temporary data structure like `subset` is useful to keep track of the current state.

This covers your query in-depth, adhering to the approach you are trying to master for problem-solving.