---

---
---

[LC Link](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)


### First Principles Thinking

The problem is to find two numbers in a sorted array such that their sum equals a given target. The indices of these numbers need to be returned, and the array is 1-indexed. Several fundamental truths emerge from the problem statement:

1. The array is sorted in non-decreasing order.
2. The indices of the two numbers to be found should be different and in ascending order (1 <= index1 < index2).
3. You may not use the same element twice.
4. The solution must use only constant extra space.

Using these principles, we can approach the problem in various ways to achieve an optimal solution.

### Brute Force Approach

#### Explanation of the Naive Solution
The most straightforward way to solve this problem would be to use two nested loops to go through each pair of numbers to check if they add up to the target.

#### Programming/Coding Principles
- **Raw Algorithm**: 
  1. Start with index1 at the first element.
  2. Loop through the array for index2 from index1 + 1 to the end.
  3. Check if the elements at index1 and index2 sum up to the target.
- **Data Structure**: Use the input array itself. No extra data structure needed.
- **For Loop**: Two nested for-loops would suffice.
- **Base Case**: None.
- **Edge Case**: The question guarantees a solution, so no need to handle this scenario.

#### Python Code for Brute Force Approach

```python
def twoSum(numbers, target):
    for i in range(len(numbers)):
        for j in range(i + 1, len(numbers)):
            if numbers[i] + numbers[j] == target:
                return [i + 1, j + 1]  # 1-indexed
```

#### Analysis of Time and Space Complexity
- **Time Complexity**: \(O(n^2)\) because of the nested loops.
- **Space Complexity**: \(O(1)\) as we are not using any extra space.

### Optimized Approaches

#### Intuition and Train of Thought
Given that the array is sorted, we can utilize two pointers starting from each end, moving towards each other based on the sum's comparison to the target. 

#### Step-by-step Transition
1. Start with one pointer at the beginning and another at the end.
2. Add the elements at these pointers.
3. If the sum is too large, move the end pointer backward.
4. If the sum is too small, move the start pointer forward.
5. Stop when you find the target sum.

#### Programming/Coding Principles
- **Raw Algorithm**: 
  1. Initialize two pointers, one at start and one at end.
  2. While the pointers haven't met, continue checking sums.
- **Data Structure**: Input array itself.
- **While Loop**: A single while loop would suffice for this approach.
- **Update Criteria**: Based on comparison with the target sum.
- **Base Case**: Not applicable.
- **Edge Case**: Guaranteed solution.

#### Python Code for the Optimized Solution

```python
def twoSum(numbers, target):
    left, right = 0, len(numbers) - 1
    
    while left < right:
        current_sum = numbers[left] + numbers[right]
        
        if current_sum == target:
            return [left + 1, right + 1]  # 1-indexed
        elif current_sum < target:
            left += 1
        else:
            right -= 1
```

#### Analysis of Time and Space Complexity
- **Time Complexity**: \(O(n)\)
- **Space Complexity**: \(O(1)\)

### Strategies and Techniques
The technique used here involves using two pointers to traverse a sorted array to find a specific sum. This approach can be generalized to various two-pointer techniques in similar problems, where the array is sorted, and you're trying to find a set of elements that satisfy specific conditions. Keep in mind how the sorted property of the array made this optimization possible.

---
---
### Neetcode
```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l, r = 0, len(numbers) - 1

        while l < r:
            curSum = numbers[l] + numbers[r]

            if curSum > target:
                r -= 1
            elif curSum < target:
                l += 1
            else:
                return [l + 1, r + 1]

```

The given code is an implementation to solve the Two Sum II problem. This version of the Two Sum problem provides an input list `numbers` which is sorted in non-decreasing order. The task is to find two numbers such that they add up to a specific `target`. The function returns the indices of the two numbers (1-indexed) as an integer array of size 2.

Let's break it down in detail:

## First Principles of Problem-solving:

The basic principle used here is **Iterative Comparison** with the **Two-pointer Technique**. As the list is sorted, we can have a pointer at the beginning and end of the list, and move them inward based on the current sum and target.

## Intuition:

The core intuition is that since the list is sorted:
- If the sum of the numbers at the current two pointers is greater than the target, then to reduce the sum, we should move the right pointer to the left.
- If the sum is less than the target, then to increase the sum, we should move the left pointer to the right.

## Time and Space Complexity:

**Time Complexity:** O(n) - We're going through the list with two pointers at most once.
**Space Complexity:** O(1) - No extra space is used other than a few variables.

## Raw Algorithm:

1. Initiate two pointers, `l` at the start and `r` at the end of the list.
2. While `l` is less than `r`:
    - Compute the sum of the numbers at indices `l` and `r`.
    - If this sum is greater than the target, move the `r` pointer leftwards.
    - If this sum is less than the target, move the `l` pointer rightwards.
    - If the sum matches the target, return the indices (1-indexed).

## Programming Approach:

**Why the while loop?**

The while loop is used to iterate over the array with the two pointers until they converge, ensuring all possible pairs are checked.

**Why the variables `l` and `r`?**

The variables `l` and `r` are the two pointers used to iterate over the array. They start at opposite ends of the array and move towards each other. This approach is efficient because the array is sorted.

**Why the conditional checks inside the loop?**

The conditions `curSum > target` and `curSum < target` are used to decide which pointer to move based on how the current sum compares to the target.

## Further Optimizing Clues:

The provided solution is efficient given the constraints (sorted input list). However, always remember:
1. For unsorted lists, using a hashmap can be effective.
2. In this sorted version, the two-pointer technique is the optimal approach.

## Mnemonics to remember:

**T.L.R**:
- **T**wo-pointer technique on a sorted list.
- **L**eft pointer starts at the beginning.
- **R**ight pointer starts at the end.

## Base and Edge Cases:

- If the list is empty or has only one element, the solution is not possible.
- If multiple pairs have the same sum, this method returns the pair which is found first (outermost).

Understanding the fundamental principle that in a sorted list, moving pointers from opposite ends towards the center can be used to hone in on a target sum, is key to the two-pointer approach in this problem.
