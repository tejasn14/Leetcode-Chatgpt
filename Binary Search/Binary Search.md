---

---
---
[LC Link](https://leetcode.com/problems/binary-search/)
### First Principles Thinking:
The fundamental truths about the problem are:
1. We are given a sorted array.
2. We need to find the index of a target element in this array.
3. The algorithm must have a time complexity of \(O(\log n)\).

Considering these constraints, it becomes evident that a binary search algorithm can be applied here.

### Brute Force Approach:
Although the question specifically asks for a \(O(\log n)\) solution, the naive way to solve this problem for educational purposes would be to scan the array element by element.

#### Programming/Coding Principles:
- Start from the first element and compare each element with the target until you find it.
- If you find it, return the index. Otherwise, return -1.

#### Python Code for Brute Force:

```python
def search(nums, target):
    for i, num in enumerate(nums):
        if num == target:
            return i
    return -1
```

#### Analysis of Time and Space Complexity:
- Time Complexity: \(O(n)\)
- Space Complexity: \(O(1)\)

### Optimized Approach:

#### Intuition and Train of Thought:
We can solve this problem optimally by using binary search, which inherently has a time complexity of \(O(\log n)\).

#### Programming/Coding Principles:
- Use two pointers, `left` and `right`, to indicate the range of elements under consideration.
- Calculate the mid-point and compare the value at this index with the target.
- Based on the comparison, adjust `left` or `right` pointers to narrow down the search range.

#### Python Code for Optimized Approach:

```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

#### Analysis of Time and Space Complexity:
- Time Complexity: \(O(\log n)\) (As we are dividing the search space by 2 in each iteration)
- Space Complexity: \(O(1)\) (No extra space other than variables)

#### Strategies and Techniques:
- Binary search is a highly effective algorithm for searching in a sorted array.
- Knowing the constraints of the problem can help guide which algorithmic approach to take.
- With each step in binary search, the range of possible solutions is halved, making it extremely efficient for large datasets.

By applying these techniques, you can address a wide array of problems that involve searching in sorted arrays or lists.

---
---

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1

        while l <= r:
            m = l + ((r - l) // 2)  # (l + r) // 2 can lead to overflow
            if nums[m] > target:
                r = m - 1
            elif nums[m] < target:
                l = m + 1
            else:
                return m
        return -1

```