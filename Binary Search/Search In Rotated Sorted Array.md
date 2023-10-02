---

---
---
[LC Link](https://leetcode.com/problems/search-in-rotated-sorted-array/)

---
---

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1

        while l <= r:
            mid = (l + r) // 2
            if target == nums[mid]:
                return mid

            # left sorted portion
            if nums[l] <= nums[mid]:
                if target > nums[mid] or target < nums[l]:
                    l = mid + 1
                else:
                    r = mid - 1
            # right sorted portion
            else:
                if target < nums[mid] or target > nums[r]:
                    r = mid - 1
                else:
                    l = mid + 1
        return -1

```

This is a binary search problem, but with a twist: the input list (`nums`) is rotated. That is, it's a sorted list that's been shifted so that the smallest item is no longer necessarily at the front. Let's solve this step by step:

### First Principles:
At its core, this problem rests on the first principle that in a sorted list, an element's position relative to another element indicates their ordering.

### Problem Understanding:
Given a rotated sorted list and a target number, we need to find the index of the target number. If the target doesn't exist, return `-1`.

### Intuition:
Since the list is sorted but rotated, either the left or the right half of the list will always be in perfect sorted order. We can use this property to decide which half of the list to inspect further.

### Time/Space Complexity:
- Time complexity: O(log n) because we're using binary search.
- Space complexity: O(1) since we're using a constant amount of space.

### Raw Algorithm:
1. Initialize two pointers, `l` and `r`, to the start and end of the list, respectively.
2. Calculate the midpoint, `mid`.
3. Check the value at `mid`. If it's equal to the `target`, we're done.
4. If not, determine which half (left or right) of the list is sorted.
5. Once we know which half is sorted, we can decide which half the `target` can possibly be in by comparing the `target` to the smallest and largest values in the sorted half.
6. Narrow our search to the half where the `target` can be.
7. Repeat until `l` is greater than `r`.

### Programming Approach:
1. **While Loop**: We use a `while` loop to keep narrowing our search until `l` is greater than `r`. This is the essence of binary search.
2. **Variables**: 
   - `l` and `r` represent the current range we're considering.
   - `mid` calculates the midpoint of the current range.
3. **Deciding Which Half Is Sorted**: We can determine which half is sorted by comparing `nums[l]` and `nums[mid]`.
   - If `nums[l] <= nums[mid]`, the left half is sorted.
   - Otherwise, the right half is sorted.
4. **Deciding Where The Target Might Be**: After determining which half is sorted:
   - If the `target` lies within the sorted range, adjust `l` or `r` to search within this half.
   - Otherwise, adjust `l` or `r` to search the other half.
5. **Data Structures**: 
   - This code doesn't explicitly use advanced data structures like heaps, stacks, etc. It takes advantage of the list's intrinsic ordering.
6. **Base Cases & Edge Cases**: 
   - The problem ensures the target exists only once. 
   - There isn’t an explicit base case because the `while` loop's condition handles it. When `l` surpasses `r`, we know the target isn’t in the list, so we return `-1`.

### Optimizing Clues:
The given solution is already quite optimized given the nature of the problem. The key to the solution's efficiency is the decision-making process that decides which half of the list to look in, which is inherently logarithmic.

### Mnemonics to Remember:
**"In a Rotated Binary Search:**
1. **Determine the sorted half**.
2. **Check if target is within that sorted half's range**.
3. **If yes, go there. If no, go to the other half"**.

Remembering this will ensure you have a mental framework to tackle rotated binary search problems in the future.