This is a classic problem known as the "Maximum Subarray Problem". To approach this problem using first principles, we need to consider various strategies and their complexities.

### First Principles Thinking:

1. **Brute Force Approach:**
   - **Idea:** Consider every possible subarray, compute its sum and update the maximum sum.
   - **Complexity:** \(O(n^3)\) (because there are \(O(n^2)\) subarrays and it takes \(O(n)\) to compute the sum for each subarray).
   - **Problem:** The time complexity makes this approach impractical for large arrays.

2. **Optimized Brute Force:**
   - **Idea:** While considering every possible subarray, we can use the already computed sum to optimize our calculations.
   - **Complexity:** \(O(n^2)\).
   - **Problem:** Still too slow for large arrays.

3. **Kadane's Algorithm:**
   - **Idea:** Maintain a running sum and reset it to zero whenever it turns negative.
   - **Complexity:** \(O(n)\).
   - **Advantage:** Linear time complexity makes it efficient for large arrays.

4. **Divide and Conquer:**
   - **Idea:** Split the array into halves and compute the maximum subarray sum in each half. Additionally, compute the maximum crossing subarray sum. The answer is the maximum of these three values.
   - **Complexity:** \(O(n \log n)\).
   - **Advantage:** This is a more sophisticated approach which can be used as a building block for solving more complex problems.

### Kadane's Algorithm (O(n) Solution):

The idea behind Kadane's algorithm is quite intuitive. We initialize two variables, `current_sum` and `max_sum`. As we traverse the array:

- We add the current number to `current_sum`.
- If `current_sum` becomes negative, we reset it to zero because a negative sum won't help in maximizing the subarray sum.
- At each step, we also check if `current_sum` is greater than `max_sum` and update `max_sum` if it is.

```python
def maxSubArray(nums):
    max_sum = float('-inf')
    current_sum = 0
    for num in nums:
        current_sum += num
        max_sum = max(max_sum, current_sum)
        if current_sum < 0:
            current_sum = 0
    return max_sum
```

### Divide and Conquer Approach:

For the divide and conquer approach:

1. Divide the array into two halves.
2. Recursively find the maximum subarray sum in the left half, the right half, and the crossing sum.
3. The maximum of the three values obtained in the above step is our answer.

The trickiest part is computing the maximum crossing subarray sum, but it can be efficiently found by checking for the maximum subarray sum that ends at the midpoint on the left half and starts at the midpoint in the right half.

This approach provides a good exercise in recursive problem solving and can be extended to solve other similar problems.


In summary, the problem-solving strategy involves starting with a brute force approach, understanding its limitations, and then exploring optimized algorithms and understanding the principles behind them.

---
Certainly! Here's a step-by-step explanation and the implementation of the divide and conquer approach:

### **Explanation:**

1. **Base Case:** If the array has only one element, the maximum subarray is the array itself.

2. **Divide:** Split the array into two halves.

3. **Conquer:** 
    - Recursively find the maximum subarray for the left half.
    - Recursively find the maximum subarray for the right half.

4. **Combine:** This is the trickiest part. Find the maximum subarray that crosses the midpoint.
    - For the left part of this crossing sum, start from the middle and move to the left, tracking the maximum sum as you go.
    - For the right part, start from the middle + 1 and move to the right, doing the same.
    - Combine the left and right parts to get the crossing sum.

5. **Return:** The maximum value from the three sums (left, right, crossing) is the solution for the current array/subarray.

### **Implementation:**

```python
def maxSubArray(nums):
    # Base Case
    if len(nums) == 1:
        return nums[0]

    mid = len(nums) // 2
    left = nums[:mid]
    right = nums[mid:]

    # Recursively find max subarrays for left and right halves
    left_max = maxSubArray(left)
    right_max = maxSubArray(right)

    # Find max crossing sum
    left_cross_max = float('-inf')
    right_cross_max = float('-inf')
    current_sum = 0
    for i in range(mid-1, -1, -1):  # Move from middle towards left
        current_sum += nums[i]
        left_cross_max = max(left_cross_max, current_sum)

    current_sum = 0
    for i in range(mid, len(nums)):  # Move from middle towards right
        current_sum += nums[i]
        right_cross_max = max(right_cross_max, current_sum)

    cross_max = left_cross_max + right_cross_max

    # Return the maximum from the three
    return max(left_max, right_max, cross_max)

# Test
nums = [-2,1,-3,4,-1,2,1,-5,4]
print(maxSubArray(nums))  # Output: 6
```

This divide and conquer approach efficiently handles all possible positions of the maximum subarray: entirely in the left half, entirely in the right half, or straddling the midpoint. The time complexity is \(O(n \log n)\) due to the recursive division of the array and the linear work done at each level of recursion.

---


```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        res = nums[0]

        total = 0
        for n in nums:
            total += n
            res = max(res, total)
            if total < 0:
                total = 0
        return res

```

Let's deep dive into this solution for the "Maximum Subarray" problem using the **First Principles Approach** and the other guidelines you've provided. The problem aims to find the contiguous subarray (at least one number) which has the largest sum and return its sum.

### First Principles:

#### What are the base truths?
1. A subarray's sum is the sum of its elements.
2. To maximize the sum of a subarray ending at a certain index, we only need to consider the sum of the subarray immediately before it.
3. If the sum of the subarray immediately before the current index is negative, it won't contribute to the maximum sum of the subarray ending at the current index.

### Intuition and Approach:

The solution uses a **[[Greedy]] Approach** called **Kadane's Algorithm**. At each step, it calculates the maximum subarray sum ending at that index. The idea is to iteratively compute the sum of the subarray and reset it when it turns negative, since a negative sum will not help in achieving a maximum sum.

### Code Explanation:

- **Initialization**: 
    - `res` is initialized to `nums[0]` as it will store the result, and in the worst case (when all numbers are negative), the maximum subarray would be the maximum single number.
    - `total` is initialized to `0` to keep track of the current running sum of the subarray.

- **Iteration through the Array**:
    - `for n in nums:` iterates through each number in the list.

    - `total += n` updates the current sum by adding the current number.
    
    - `res = max(res, total)` updates the result with the maximum value between the current result and the current sum. This effectively captures the maximum sum encountered so far.
    
    - `if total < 0:` checks if the current sum has turned negative. If so, it resets `total` to `0`, as any negative sum will not help in achieving the maximum subarray sum.

- **Return**: 
    - After iterating through the entire list, the function returns the value of `res`, which represents the maximum subarray sum.

### Why the loop and variables?

1. **Loop**: The loop processes each element in the array exactly once, which is essential for computing the running sum and checking if it's a new maximum.

2. **`res` variable**: Holds the maximum subarray sum found so far.

3. **`total` variable**: Keeps track of the running sum. If this becomes negative, it's reset to zero since a negative value will reduce the value of subsequent sums.

### Time and Space Complexity:
- **Time Complexity**: \(O(N)\). Each element in the array is processed once.
- **Space Complexity**: \(O(1)\). Only a constant amount of space is used, regardless of the input size.

### Summary:
The solution essentially iterates through the array, maintaining a running sum and a maximum sum. At every step, the running sum is added to the current element, and the maximum sum is updated accordingly. If the running sum becomes negative, it's reset to zero. This logic ensures that only subarrays with a positive sum are considered, leading to the correct answer.