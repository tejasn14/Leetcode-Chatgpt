---

---
---
[LC Link](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

---
---

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        start , end = 0, len(nums) - 1 
        curr_min = float("inf")
        
        while start  <  end :
            mid = (start + end ) // 2
            curr_min = min(curr_min,nums[mid])
            
            # right has the min 
            if nums[mid] > nums[end]:
                start = mid + 1
                
            # left has the  min 
            else:
                end = mid - 1 
                
        return min(curr_min,nums[start])

```

The problem at hand is about finding the minimum element in a rotated sorted list. Rotated sorted lists are fascinating because, while they disrupt the continuous increasing order, there's still a semblance of order present.

### First Principles:
In a sorted list, the element's position reflects its ordering. In a rotated sorted list, either the left or the right half of the list is in perfect sorted order. These foundational principles help in narrowing down our search for a specific element, such as the minimum.

### Solving Intuition:
1. A key observation in a rotated sorted list is that the rotation point is the only place where the next number is smaller than the previous number.
2. If a number at a particular index is greater than the last element of the list, the rotation point (and hence the smallest number) must be to its right.
3. Otherwise, the rotation point must be to its left.

### Time/Space Complexity:
- **Time complexity**: O(log n). This is because, at every step, we're effectively discarding half of the possibilities. 
- **Space complexity**: O(1). We're using a constant amount of space regardless of the input size.

### Raw Algorithm:
1. Initialize pointers `start` and `end` to point to the beginning and end of the list.
2. Find the midpoint, `mid`.
3. Update the current minimum, `curr_min`, to be the minimum of `curr_min` and `nums[mid]`.
4. Check if the number at `mid` is greater than the number at `end`.
5. If true, it means the minimum number is in the right half, so update `start` to `mid + 1`.
6. Otherwise, the minimum number is in the left half, so update `end` to `mid - 1`.
7. Continue until `start` is not less than `end`.
8. Finally, return the minimum of `curr_min` and `nums[start]`.

### Programming Approach:
1. **While Loop**: We loop until `start` is not less than `end`. This ensures we continue the search as long as there's a valid subrange to consider.
2. **Variables**:
   - `start` and `end` help us keep track of our current search range.
   - `curr_min` stores the smallest number we've found so far. Initializing it to `float("inf")` ensures any number will be smaller in comparison.
   - `mid` calculates the center of our current range.
3. **Decision Logic**: The key lies in how we decide which half to search next:
   - If `nums[mid] > nums[end]`, the smallest number lies to the right.
   - Otherwise, it's to the left.
4. **Data Structures**: The list itself and basic variables are the primary structures. The nature of the problem doesn't necessitate stacks, queues, etc.
5. **Base & Edge Cases**: The logic inherently handles cases like having one element or having two elements where one is the rotation point. The final return ensures we get the minimum between the current minimum and the last number checked.
6. **Optimizing Clues**: The given solution is already efficient. A possible micro-optimization would be to return `nums[0]` if `nums[0] < nums[-1]` because this means the array isn't rotated.

### Mnemonics to Remember:
**"Rotated Min Search:**
1. **Check the middle**.
2. **Middle greater than end? Go right**.
3. **Otherwise, go left"**.

Understanding this simple structure helps in quickly grasping the rotated binary search's essence, especially when looking for the minimum element.