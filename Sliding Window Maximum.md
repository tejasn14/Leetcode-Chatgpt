[LC Link](https://leetcode.com/problems/sliding-window-maximum/)
### First Principles Thinking

This problem is about maximizing the elements in a moving window of fixed size \( k \) over a given array. The core principles here involve optimization and data structure choice to ensure the maximum element in a sliding window can be efficiently found.

### Brute Force Approach

#### Explanation

1. Iterate through the array in steps of size 1.
2. For each new window, find the maximum element within the window and store it in a list.

#### Programming/Coding Principles

1. **Raw Algorithm**: Nested loops; one to slide the window and another to find the maximum element in the window.
2. **Best Data Structure**: A simple list to keep the maximum values of each window.
3. **Loop**: Use a for-loop to iterate over the array.

#### Python Code

```python
def maxSlidingWindow(nums, k):
    n = len(nums)
    if n == 0:
        return []
    
    max_window = []
    
    for i in range(n - k + 1):
        max_val = max(nums[i:i+k])
        max_window.append(max_val)
    
    return max_window
```

#### Analysis

- **Time Complexity**: \(O(n \times k)\), where \( n \) is the length of the array and \( k \) is the window size.
- **Space Complexity**: \(O(n - k + 1)\) for storing the maximum values.

### Optimized Approaches

#### First Optimization: Using a Deque

##### Intuition and Train of Thought

1. Use a deque to maintain the maximum elements of the current window.
2. Move the sliding window from left to right, updating the deque accordingly.

##### Programming/Coding Principles

1. **Best Data Structure**: A deque to store the elements in a way that the maximum element is always at the front.
2. **Loop**: Single for-loop to traverse the array once.
3. **Update Criteria**: Remove elements from the back while they are smaller than the element being inserted and remove the front element if it's out of the window.

##### Python Code

```python
from collections import deque

def maxSlidingWindow(nums, k):
    if not nums:
        return []
    
    deq = deque()
    max_nums = []
    
    for i, n in enumerate(nums):
        while deq and nums[deq[-1]] < n:
            deq.pop()
        
        deq.append(i)
        
        if deq[0] == i - k:
            deq.popleft()
        
        if i >= k - 1:
            max_nums.append(nums[deq[0]])
    
    return max_nums
```

#### Analysis

- **Time Complexity**: \( O(n) \)
- **Space Complexity**: \( O(k) \) for the deque.

### Strategies and Techniques

1. **Sliding Window**: This problem is a prime example of the sliding window technique for subarray problems.
2. **Deque**: A deque is useful for efficiently adding/removing elements from both ends.

The sliding window and deque techniques are versatile and can be used in various array or string manipulation problems that require you to maintain a contiguous sequence of elements satisfying some criteria.

---
---

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        output = []
        q = collections.deque()  # index
        l = r = 0
        # O(n) O(n)
        while r < len(nums):
            # pop smaller values from q
            while q and nums[q[-1]] < nums[r]:
                q.pop()
            q.append(r)

            # remove left val from window
            if l > q[0]:
                q.popleft()

            if (r + 1) >= k:
                output.append(nums[q[0]])
                l += 1
            r += 1

        return output

```

The problem at hand asks for the maximum value in each sliding window of size `k` of a given list of numbers. The sliding window approach is fitting, but with the added need to efficiently find the maximum in a window of `k` values. Here's the breakdown:

### 1. First Principles of Problem-solving Approach:

If you were to find the maximum value of each window naively, for each window you'd need O(k) time, resulting in an O(nk) time complexity for the entire array. This is inefficient for large values of `k` and `n`.

### 2. Solving Intuition:

A natural intuition is to keep track of potential candidates for the maximum. This can be done using a double-ended queue (`deque`), ensuring that the largest value is always at the head of the queue.

### 3. Time/Space Complexity:

**Time Complexity**: O(n) - Each element is processed once.
**Space Complexity**: O(k) - The `deque` (`q`) can store up to `k` elements.

### 4. Raw Algorithm:

1. Iterate over the list with a sliding window of size `k`.
2. Use a `deque` to maintain indices of potential candidates for the maximum value.
3. For each element, remove indices of smaller values from the back of the `deque` as they will never be considered as max again.
4. Append the current index to the `deque`.
5. If the leftmost index of the window is greater than the front of the `deque`, pop the front. This ensures that only indices within the current window are stored.
6. If the window size has reached `k`, append the value at the front of the `deque` (the current maximum) to the output.

### 5. Programming Approach:

- **Why `deque`**: A `deque` is an appropriate data structure since you need to efficiently pop elements from both ends. It helps maintain the maximum at the front and remove smaller or out-of-window values from the back.
- **Why the `while` loop**: Iterates over the list, simulating the sliding window movement.
- **Why the variables**: 
  - `output`: To store the result.
  - `q`: A deque to keep track of indices of potential maximum values.
  - `l` and `r`: Pointers representing the left and right ends of the window.
  
### 6. Base cases and Edge Cases:

- Base Case: If `nums` is empty, the function immediately returns an empty list.
- Edge Case: This solution is generic and caters to any input size, including cases where `len(nums) < k`. However, if `k` is 0 or negative, this code doesn't handle it, and such edge cases would need to be added.

### 7. Further Optimizing Clues:

The provided solution is already optimal. A classic mistake is to attempt using a priority queue or heap to maintain the maximum, but it becomes inefficient when trying to remove out-of-window values. The deque-based approach is the most efficient for this specific problem.

### 8. Mnemonics to Remember the Solution:

**"Deque the Max!"**: 
- Use a `deque` to keep track of indices.
- Always keep the maximum at the front.
- Expel out-of-window and smaller values as they become irrelevant.

In a nutshell, this solution leverages a `deque` to efficiently keep track of the maximum value in a sliding window while iterating over the list.
