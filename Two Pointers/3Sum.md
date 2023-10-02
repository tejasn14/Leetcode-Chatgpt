---

---
---
[LC Link](https://leetcode.com/problems/3sum/)
### First Principles Thinking

The problem requires finding all unique triplets from the array `nums` that sum to zero. The fundamental aspects of this problem are:

1. The elements in each triplet should be distinct (i.e., i != j, i != k, and j != k).
2. The solution set must not contain duplicate triplets.
3. Triplets must sum to zero.

Based on these fundamentals, let's proceed to solving the problem.

### Brute Force Approach

#### Explanation of the Naive Solution
The simplest approach involves three nested loops to go through each possible triplet to check if they add up to zero. This is similar to the two sum problem but with an additional loop.

#### Programming/Coding Principles
- **Raw Algorithm**: 
  1. Start three nested loops from 0 to `len(nums)`.
  2. For each triplet, check if they sum up to zero.
- **Data Structure**: Use a set to store unique triplets.
- **For Loop**: Three nested for-loops.
- **Base Case**: None.
- **Edge Case**: Check for unique triplets.

#### Python Code for Brute Force Approach

```python
def threeSum(nums):
    nums.sort()
    result = set()
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            for k in range(j + 1, len(nums)):
                if nums[i] + nums[j] + nums[k] == 0:
                    result.add((nums[i], nums[j], nums[k]))
    return list(map(list, result))
```
#### Analysis of Time and Space Complexity
- **Time Complexity**: \(O(n^3)\)
- **Space Complexity**: \(O(n)\) to store the result.

### Optimized Approaches

#### Intuition and Train of Thought
The array can be sorted first, which allows us to use the Two Pointer technique inside another loop.

#### Step-by-step Transition
1. Sort the array.
2. Initialize an empty list to store unique triplets.
3. Loop through the array and apply the Two Pointer technique for the remaining part of the array to find pairs that sum to `-nums[i]`.

#### Programming/Coding Principles
- **Raw Algorithm**: 
  1. Sort the array.
  2. Use one loop and inside it use the Two Pointer technique.
- **Data Structure**: List to store unique triplets.
- **For Loop & While Loop**: A single for-loop and nested while loop.
- **Update Criteria**: Skip duplicate numbers.
- **Base Case**: None.
- **Edge Case**: Check for unique triplets.

#### Python Code for the Optimized Solution

```python
def threeSum(nums):
    nums.sort()
    result = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        left, right = i + 1, len(nums) - 1
        while left < right:
            s = nums[i] + nums[left] + nums[right]
            if s < 0:
                left += 1
            elif s > 0:
                right -= 1
            else:
                result.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                left += 1
                right -= 1
    return result
```

#### Analysis of Time and Space Complexity
- **Time Complexity**: \(O(n^2)\)
- **Space Complexity**: \(O(n)\) for storing the result.

### Strategies and Techniques
The two-pointer technique inside a loop is a powerful tool when dealing with sorted arrays. It allows us to optimize the time complexity significantly. Always think about sorting the array if the order of elements doesn't matter in the problem. This often opens the door for various optimizations.

---
---

### Neetcode

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()

        for i, a in enumerate(nums):
            # Skip positive integers
            if a > 0:
                break

            if i > 0 and a == nums[i - 1]:
                continue

            l, r = i + 1, len(nums) - 1
            while l < r:
                threeSum = a + nums[l] + nums[r]
                if threeSum > 0:
                    r -= 1
                elif threeSum < 0:
                    l += 1
                else:
                    res.append([a, nums[l], nums[r]])
                    l += 1
                    r -= 1
                    while nums[l] == nums[l - 1] and l < r:
                        l += 1
                        
        return res

```

Alright, this code solves the problem of finding all the unique triplets in the array `nums` which gives the sum of zero.

## First Principles of Problem-solving:

The core principle here is **Two-pointer Technique** used in conjunction with a **sorted array** and an **external loop** to fix one element and search for other two.

## Intuition:

Since we're looking for three numbers that sum to zero and the array is sorted after the initial sorting step, a very useful property comes into play: 
- If a large number + a smaller number is too big, we can make it smaller by decreasing the large number.
- If a large number + a smaller number is too small, we can make it bigger by increasing the smaller number.

This intuition forms the basis of our two-pointer approach.

## Time and Space Complexity:

**Time Complexity:** O(n^2) - Looping through each element and then using two pointers to traverse the rest.
**Space Complexity:** O(1) if you don't count the space required for the output, otherwise O(n) for the output.

## Raw Algorithm:

1. Sort the list.
2. Traverse the sorted list using `i`:
    - If current element is positive, break (as no sum of positive numbers will give 0).
    - To avoid duplicates, if the current element is the same as the previous one, continue.
    - Initiate two pointers: `l` starting right next to `i`, and `r` at the end of the list.
    - Move the pointers inward until they meet:
        - Calculate the sum of the numbers at these three indices.
        - If sum is too big, decrease `r`.
        - If sum is too small, increase `l`.
        - If sum is zero, store this triplet and move both pointers.
        - Avoid duplicates by moving the `l` pointer while it's the same as the previous.

## Programming Approach:

**Why the for loop?**

The loop with `i` is to fix one element and then find other two elements that together form a sum of zero. This is essential for breaking down the 3Sum problem into a 2Sum problem.

**Why the while loop?**

The while loop is the manifestation of the two-pointer technique. The two pointers `l` and `r` converge, and as they do so, they check pairs of elements to form a 2Sum with the element fixed by `i`.

**Why the conditional checks inside the loops?**

- `if a > 0`: This is to stop checking once we have crossed into positive territory, as three positive numbers cannot sum to zero.
- `if i > 0 and a == nums[i - 1]`: This avoids checking the same number more than once, ensuring our results are unique.
- `threeSum > 0`, `threeSum < 0`: To decide which pointer to move based on the current sum.
- `while nums[l] == nums[l - 1] and l < r`: To skip over duplicate numbers after finding a valid triplet.

## Further Optimizing Clues:

The given solution is one of the most efficient approaches for this problem. Yet, some general tips for problems like this:
1. Sorting is a powerful tool, especially when combined with two-pointers or binary search.
2. Always be on the lookout for opportunities to skip duplicate work or computations.

## Mnemonics to remember:

**T.S.D**:
- **T**wo-pointer technique after fixing one element.
- **S**orting to simplify the search and comparison.
- **D**eduplication to avoid duplicate triplets.

## Base and Edge Cases:

- If the list has fewer than 3 numbers, it's impossible to find a triplet.
- If all numbers are positive or all are negative, no solution.
- If more than three zeros are present, one of the solutions will be [0, 0, 0].

The elegance of this solution comes from the combination of sorting, enumeration, and the two-pointer approach, which together streamline the process of finding valid triplets.
