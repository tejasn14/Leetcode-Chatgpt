---
kanban-plugin:
---
---
[LC Link](https://leetcode.com/problems/two-sum/)
### First Principles Thinking

The fundamental idea of the problem is to find two numbers in an array that add up to a given target. The primary constraint is that we can't use an element more than once. The basic requirement for a solution is to be able to quickly check if a number exists in the array that, when added to a given number, equals the target.

### Brute Force Approach

#### Explanation

The naive way to solve this problem would be to use two nested loops to check each pair of numbers to see if they add up to the target.

#### Programming/Coding Principles

- **Best Data Structure**: A simple list or array will suffice for this approach.
- **For Loop**: We would need two nested for loops to iterate over each pair of numbers.
- **Base Case**: The first number could be anything in the list; the second one should be what is needed to reach the target.
- **Edge Case**: We can't use the same element twice.

#### Python Code

```python
def two_sum(nums, target):
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]

# Time Complexity: O(n^2)
# Space Complexity: O(1)
```

### Optimized Approach

#### Intuition

We can optimize the solution by using a HashMap to remember numbers we have seen before and their corresponding indices. This would reduce the time complexity to \(O(n)\).

#### Programming/Coding Principles

- **Best Data Structure**: HashMap for storing previously seen numbers and their indices.
- **For Loop**: Single for loop to iterate through the array.
- **Base Case**: If \( \text{target} - \text{current number} \) exists in the HashMap, we have found the solution.
- **Edge Case**: The HashMap allows us to avoid using the same element twice.

#### Python Code

```python
def two_sum(nums, target):
    num_dict = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_dict:
            return [num_dict[complement], i]
        num_dict[num] = i

# Time Complexity: O(n)
# Space Complexity: O(n)
```

### Strategies and Techniques

1. **HashMap for Quick Lookup**: Using a HashMap allows us to quickly look up elements, thereby reducing the time complexity to \(O(n)\).
2. **Enumerate for Index and Value**: Python's enumerate function can be very useful for getting both index and value in a single loop.
  
These strategies could be generalized to problems that require searching for elements that fulfill specific conditions, especially when the problem has constraints regarding time complexity.


---
---
### Neetcode

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}  # val -> index

        for i, n in enumerate(nums):
            diff = target - n
            if diff in prevMap:
                return [prevMap[diff], i]
            prevMap[n] = i

```

### First Principles Thinking

1. **Fundamental Truths**: The problem states that we are given an array of integers and a target integer. We need to find two indices such that the elements at those indices add up to the target number. 

2. **Constraints**: 
    - Each input would have exactly one solution.
    - We may not use the same element twice.
    - Return the answer in any order.

3. **Building Up**: 
    - Essentially, we need to find a pair `(x, y)` such that `x + y = target`.
  
#### Intuition and Solving Intuition

1. **Identifying Patterns**: For every number `x`, we can find its complement `y` which is `target - x`. 
2. **Optimization Insight**: Checking if a number exists in a data structure can be made very efficient with a hash table.
3. **Time Complexity Constraint**: To find a pair efficiently, it would make sense to get it in one pass, i.e., \(O(N)\).

### Explanation of Solution

#### Programming/Coding Principles

1. **Raw Algorithm**:
    - Initialize an empty hash table.
    - Loop through each element `x` and calculate its complement `y` (target - x).
    - If `y` exists in the hash table, return the indices. Otherwise, store the index of `x`.

2. **Data Structure**: 
    - Hash table (`prevMap`): to keep track of elements we have seen so far along with their indices.

3. **Looping Structure**: 
    - A single `for` loop to iterate through the array.

4. **Variables**: 
    - `prevMap`: Hash table for constant-time lookups to check the existence of the complement.
    - `i, n`: Loop variables to keep track of index and value.

5. **Update Criteria**: 
    - After checking each element, we update `prevMap` with the current number as the key and its index as the value.

6. **Base Case and Edge Case**: 
    - Base Case: The hash table starts empty.
    - Edge Cases: Since the question promises one valid answer, we can bypass most edge case checks.

#### Python Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}  # val -> index

        for i, n in enumerate(nums):
            diff = target - n
            if diff in prevMap:
                return [prevMap[diff], i]
            prevMap[n] = i
```

#### Time and Space Complexity

- **Time Complexity**: \(O(N)\) — Single pass through the array of length \(N\).
- **Space Complexity**: \(O(N)\) — Storing up to \(N\) elements in a hash table.

### Strategies and Techniques

1. **Hash Table for Pair Finding**: Whenever you need to find pairs that satisfy some condition (sum, difference, product), consider using a hash table to achieve that in \(O(N)\) time.
  
2. **Complement Matching**: If you're searching for a value that complements another value to satisfy some condition (e.g., sum to a target, difference to a target), hash tables are a good way to keep track of possible complements.

### Mnemonics

1. **"Target Minus"**: To remember the approach, focus on the idea that for each number, you look for `target - number`. This is the key insight.
  
2. **"Hash the Past"**: For quick recall, remember that you are hashing past numbers to quickly find a match in the future. This is why the hash table is named `prevMap`.

The core of this problem-solving approach revolves around using a hash table for quick look-up and the principle of checking the complement (`target - x`) to find pairs that satisfy a given condition. This technique is not just limited to sum but could be generalized to other operations like difference or product.