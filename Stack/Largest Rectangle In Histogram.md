---

---
 ---
[LC Link](https://leetcode.com/problems/largest-rectangle-in-histogram/)
### First Principles Thinking:
The fundamental truths about the problem are:
1. We are given an array of integers representing the heights of bars in a histogram.
2. Each bar has a unit width of 1.
3. The goal is to find the largest rectangular area possible in the histogram.

The challenge is to determine the rectangle with the maximum area among all possible rectangles formed by the bars of the histogram.

### Brute Force Approach:
#### Explanation:
For each bar, we can look to the left and the right to find the "span" over which it can extend and form a rectangle. The area of this rectangle would be \( \text{height} \times \text{width} \).

#### Programming/Coding Principles:
- Loop through each bar.
- For each bar, extend it as far to the left and the right as possible.
- Calculate the area of each possible rectangle and keep track of the maximum area.

#### Python Code for Brute Force:

```python
def largestRectangleArea(heights):
    max_area = 0
    for i in range(len(heights)):
        left, right = i, i
        while left >= 0 and heights[left] >= heights[i]:
            left -= 1
        while right < len(heights) and heights[right] >= heights[i]:
            right += 1
        width = right - left - 1
        area = width * heights[i]
        max_area = max(max_area, area)
    return max_area
```

#### Analysis of Time and Space Complexity:
- Time Complexity: \(O(n^2)\) (Nested loops for each bar)
- Space Complexity: \(O(1)\) (No extra space used other than input and variables)

### Optimized Approach:

#### Intuition and Train of Thought:
We can make the solution more efficient by using a stack to keep track of bars. As soon as we see a bar that is shorter than the bar at the top of the stack, we know that the rectangle with the taller bar is limited by this shorter bar, and we can calculate its area.

#### Step-by-step transition:
- Use a stack to keep track of the indices of the bars.
- Traverse the heights array. If the stack is empty or the current height is greater than the height at the index at the top of the stack, push the index into the stack.
- Otherwise, pop the stack and calculate the area.

#### Python code for Optimized Approach:

```python
def largestRectangleArea(heights):
    stack = []
    max_area = 0
    for i, h in enumerate(heights):
        while stack and h < heights[stack[-1]]:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    while stack:
        height = heights[stack.pop()]
        width = len(heights) if not stack else len(heights) - stack[-1] - 1
        max_area = max(max_area, height * width)
    return max_area
```

#### Analysis of Time and Space Complexity:
- Time Complexity: \(O(n)\) (Each bar is pushed and popped from the stack exactly once)
- Space Complexity: \(O(n)\) (Stack can grow up to the number of bars)

#### Strategies and Techniques:
- Stacks are useful for keeping track of indices while traversing an array, especially for problems where you need to find spans or contiguous segments.
- The problem demonstrates the classic "Last In, First Out" (LIFO) principle where you may not need to complete the calculation for an element until you see a smaller element.

By understanding these approaches, you'll get an insight into how to tackle problems involving arrays and histograms effectively. You'll also learn the value of optimizing from a brute force to a more efficient algorithm.

---
---

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        maxArea = 0
        stack = []  # pair: (index, height)

        for i, h in enumerate(heights):
            start = i
            while stack and stack[-1][1] > h:
                index, height = stack.pop()
                maxArea = max(maxArea, height * (i - index))
                start = index
            stack.append((start, h))

        for i, h in stack:
            maxArea = max(maxArea, h * (len(heights) - i))
        return maxArea

```

This problem, known as the "Largest Rectangle in Histogram," is a classic example where a monotonic stack is used for efficient computation.

**First Principles of Problem-Solving Approach:**

1. **Understand the Problem:** We are trying to find the largest rectangle that can be formed using the bars of the histogram. Each bar is represented by a number which is the height of the bar.
2. **Visualize the Problem:** Drawing a few examples can help in understanding that the area of the rectangle formed by any bar will be `height_of_bar * width`. The width here will be the difference between the indices where we get bars less than the current one on both left and right.

**Solving Intuition:**

To solve the problem, we need to quickly find for each bar:
1. The nearest smaller height on its left.
2. The nearest smaller height on its right.

If we can find the above efficiently, the width for any bar will be `right - left - 1`. 

A **monotonic stack** (increasing in this case) can be used for this purpose. The stack will help us maintain heights in an increasing order. As soon as we encounter a height smaller than the top of the stack, we know we've found the "right boundary" for those bars in the stack which are taller than the current one.

**Time/Space Complexity:**

Time: O(N) - Every bar is pushed and popped from the stack exactly once.
Space: O(N) - In the worst case, the stack might have all the bars.

**Raw Algorithm of the Code:**

1. Initialize `maxArea` to 0.
2. Use a stack to maintain a list of pairs, each pair consisting of the index of a bar and its height.
3. For each bar in `heights`:
   - While the stack is not empty and the height of the current bar is less than the height at the top of the stack, pop from the stack. This means we've found the right boundary for the bar at the top of the stack.
   - Calculate the area with the bar at the top of the stack. Update `maxArea` if this area is larger.
   - Push the current bar onto the stack.
4. After processing all bars, there might be some bars left in the stack. These bars did not have any smaller height on their right. For each of these bars, calculate the area considering the right boundary as the end of the histogram.

**Programming Approach:**

1. **Why the stack:** To maintain a list of bars in increasing order of heights.
2. **Why the while loop:** To keep popping from the stack as long as we find a height smaller than the top of the stack.
3. **Why the variables `maxArea`, `start`, `index`, `height`:** To keep track of the largest area found, the start index of the current bar's rectangle, and to unpack the bar details from the stack.
4. **Why the second for loop:** To process bars left in the stack which did not have a smaller height on their right.

**Base Cases/Edge Cases:**
- Base case: If there's only one bar, the maximum rectangle area will be the height of that bar.
- Edge case: If all bars are of the same height, the maximum rectangle area will be `number_of_bars * height_of_each_bar`.

**Further Optimizing Clues:**
The current solution is already quite optimized. However, you could potentially make the code cleaner or use fewer variables, but that might compromise readability.

**Mnemonics to Remember the Solution:**
"Stack the bars tall, pop when they fall."
1. Maintain bars in increasing height order in the stack.
2. Pop from the stack when you find a shorter bar, calculate the area.
3. After all bars are processed, calculate the area for remaining bars in the stack.

Note: Like before, the `List` type for the function signature is not imported in the code snippet. Remember to import it using `from typing import List` when implementing this in an actual coding environment.
