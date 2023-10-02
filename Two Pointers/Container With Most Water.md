---

---
---
[LC Link](https://leetcode.com/problems/container-with-most-water/)
### First Principles Thinking

For this problem, the primary goal is to maximize the area between two lines on the x-axis, formed by the heights given in the array. The fundamental principles here are:

1. The area is determined by the distance between the lines on the x-axis (width) and the height of the shorter line between the two (height).
2. The area of any container can be represented as `Area = Width * Height`.
3. We can't slant the container, meaning the lines will remain vertical.

### Brute Force Approach

#### Explanation of the Naive Solution
The brute-force approach would involve calculating the area for every possible pair of lines and storing the maximum area seen so far. 

#### Programming/Coding Principles
- **Raw Algorithm**: Nested for-loop to iterate through every pair of lines and calculate the area.
- **Data Structure**: None.
- **For Loop**: Two nested for-loops.
- **Update Criteria**: Keep track of the maximum area.
- **Base Case & Edge Case**: None, in this case.

#### Python Code for Brute Force Approach
```python
def maxArea(height):
    max_area = 0
    for i in range(len(height)):
        for j in range(i+1, len(height)):
            width = j - i
            minHeight = min(height[i], height[j])
            area = width * minHeight
            max_area = max(max_area, area)
    return max_area
```

#### Analysis of Time and Space Complexity
- **Time Complexity**: \(O(n^2)\)
- **Space Complexity**: \(O(1)\)

### Optimized Approaches

#### Intuition and Train of Thought
We can solve this problem in a more efficient way by using the Two Pointer technique. Start with pointers at both ends of the array and move them inward to maximize the area.

#### Step-by-step Transition
1. Initialize two pointers, one at the start and one at the end of the array.
2. Calculate the area and update the maximum area.
3. Move the pointer pointing to the shorter line inward.

#### Programming/Coding Principles
- **Raw Algorithm**: Two Pointer technique.
- **Data Structure**: None.
- **While Loop**: Iterate while `left < right`.
- **Update Criteria**: Update the maximum area and move the pointer of the shorter line inward.
- **Base Case & Edge Case**: None.

#### Python Code for the Optimized Solution
```python
def maxArea(height):
    left, right = 0, len(height) - 1
    max_area = 0
    
    while left < right:
        width = right - left
        minHeight = min(height[left], height[right])
        max_area = max(max_area, width * minHeight)
        
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_area
```

#### Analysis of Time and Space Complexity
- **Time Complexity**: \(O(n)\)
- **Space Complexity**: \(O(1)\)

### Strategies and Techniques
The Two Pointer technique is extremely useful for this kind of problem where you are looking to maximize or minimize a value based on two elements in an array. It drastically improves time complexity by avoiding unnecessary calculations. Whenever you have a problem involving a sequence and a need to optimize, consider whether a two-pointer approach could be useful.


---
---

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        l, r = 0, len(height) - 1
        res = 0

        while l < r:
            res = max(res, min(height[l], height[r]) * (r - l))
            if height[l] < height[r]:
                l += 1
            elif height[r] <= height[l]:
                r -= 1
            
        return res

```

Alright, let's break down the solution for the problem of finding the maximum area of water that can be contained within a pair of vertical lines, where the lines are represented by the heights in the `height` list.

## First Principles of Problem-solving:

The key principle here is the two-pointer technique. If you observe the solution, it starts with two pointers at the start and the end of the list and then moves inwards.

**Why?** 

Because starting with the widest possible width (distance between the two ends) ensures we don't miss out on potentially larger areas. Moving pointers inwards will allow us to adjust height while checking various areas.

## Intuition:

The intuition behind this problem is to maximize the area formed by two lines. The area is determined by two factors:
1. The distance between two lines (width).
2. The minimum height of the two lines.

Since we start with the maximum possible width, we only have to adjust the height to potentially find a larger area.

## Time and Space Complexity:

**Time Complexity:** O(n) - We go through the list once.
**Space Complexity:** O(1) - Constant extra space regardless of the input size.

## Raw Algorithm:

1. Initialize two pointers `l` at 0 and `r` at the end of the list.
2. While `l` is less than `r`:
    - Calculate the area using the current pair of lines.
    - Compare it with the maximum area found so far.
    - If the height at `l` is less than the height at `r`, move `l` to the right (increase it). Otherwise, move `r` to the left (decrease it).
3. Return the maximum area.

## Programming Approach:

**Why the while loop?**

The while loop is used to iterate through the list and constantly adjust the pointers until they meet.

**Why the variables `l`, `r`, and `res`?**

- `l` and `r` represent the two vertical lines.
- `res` is used to keep track of the maximum area found so far.

**Why `min(height[l], height[r]) * (r - l)`?**

This calculates the area between two lines. The area is the width (distance between two lines) multiplied by the height (minimum of the two lines).

**Why the conditional checks inside the loop?**

This is crucial. If `height[l]` is less than `height[r]`, we move `l` forward because we hope to find a taller line to possibly get a bigger area. If we moved the taller line (in this case `r`), we would never get a larger area since we're decreasing the width and not potentially increasing the height.

## Further Optimizing Clues:

The algorithm is already optimized, but always remember:

1. Early breaks: If, at some point, you realize further moving pointers won't result in a bigger area, you can break early.
2. Use of efficient data structures: Although not applicable here, always consider if data structures can simplify and optimize your solution.

## Mnemonics to remember:

**W.H.M.**: 
- **W**idth maximization (start with widest distance)
- **H**eight adjustment (always try to adjust the shorter line to find a taller one)
- **M**aximum area tracking (always track the maximum area)

## Base and Edge Cases:

- When the list has only one element, the area is 0 since we need at least two lines to form an area.
- When all lines have the same height, the maximum area will be determined by the widest distance between any two lines.

Remember, understanding the problem deeply and recognizing patterns and principles is key to mastering such problems.
