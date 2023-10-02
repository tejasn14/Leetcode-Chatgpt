---

---
---
[LC Link](https://leetcode.com/problems/kth-largest-element-in-an-array/)

---
---

```python
# Solution: Sorting
# Time Complexity:
#   - Best Case: O(n)
#   - Average Case: O(n*log(n))
#   - Worst Case:O(n*log(n))
# Extra Space Complexity: O(n)
class Solution1:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        nums.sort()
        return nums[len(nums) - k]


# Solution: QuickSelect
# Time Complexity:
#   - Best Case: O(n)
#   - Average Case: O(n)
#   - Worst Case: O(n^2)
# Extra Space Complexity: O(1)
class Solution2:
    def partition(self, nums: List[int], left: int, right: int) -> int:
        pivot, fill = nums[right], left

        for i in range(left, right):
            if nums[i] <= pivot:
                nums[fill], nums[i] = nums[i], nums[fill]
                fill += 1

        nums[fill], nums[right] = nums[right], nums[fill]

        return fill

    def findKthLargest(self, nums: List[int], k: int) -> int:
        k = len(nums) - k
        left, right = 0, len(nums) - 1

        while left < right:
            pivot = self.partition(nums, left, right)

            if pivot < k:
                left = pivot + 1
            elif pivot > k:
                right = pivot - 1
            else:
                break

        return nums[k]

```

Let's break down the given solutions to understand the underlying principles, intuition, and code structure.

### Solution 1: Sorting

#### Problem-Solving Approach:

1. **Sorting Mechanism**: In this approach, the idea is simple: Sort the array, then access the k-th largest element by indexing the sorted array from the end.

#### Solving Intuition:

- If you can sort the entire array, then the solution becomes trivial. The k-th largest element will be positioned at `len(nums) - k` in the sorted list.

#### Time Complexity:

- Best, Average, and Worst Case: O(n log n) - Sorting a list has a time complexity of O(n log n) for many sorting algorithms, including the one Python uses by default (Timsort).

#### Space Complexity:

- Extra Space Complexity: O(n) - Sorting in Python requires O(n) space in the worst case.

#### Why The Code Is Written The Way It Is:

1. **`nums.sort()`**: Python provides an inbuilt method for lists to sort themselves. This line uses Timsort to sort the list in ascending order.

2. **`return nums[len(nums) - k]`**: After sorting, the k-th largest element can be accessed using negative indexing. This is a very straightforward approach and is easy to grasp.

#### Base Cases and Edge Cases:

- If the list `nums` is empty, this approach would raise an error since there is no element to access.
  
- If `k` is greater than the length of the list, this approach would again fail as there isn't a k-th largest element.

### Solution 2: QuickSelect

#### Problem-Solving Approach:

1. **Partitioning**: QuickSelect, like QuickSort, partitions the list around a pivot. However, instead of recursively sorting both halves, it recurses only into one half—the half where the desired k-th element is.

#### Solving Intuition:

- The idea behind QuickSelect is similar to QuickSort. By selecting a pivot and partitioning the array so that all elements less than the pivot come before all elements greater than the pivot, one can determine the index of the pivot in the sorted array. If the index matches k, then the pivot is the k-th smallest element. If not, you can repeat the partition on only the half of the array where the k-th element resides.

#### Time Complexity:

- Best and Average Case: O(n) - On average, each iteration of the QuickSelect reduces the size of the problem by a factor of 2 (since you only recurse into one half). This leads to an average complexity of O(n).

- Worst Case: O(n^2) - In the worst case (e.g., when the input list is already sorted), QuickSelect behaves like QuickSort and degenerates to a quadratic time complexity.

#### Space Complexity:

- Extra Space Complexity: O(1) - QuickSelect operates in-place, modifying the input list.

#### Why The Code Is Written The Way It Is:

1. **`self.partition(nums, left, right)`**: The partition function is central to the QuickSelect algorithm. It selects a pivot and ensures all elements less than the pivot come before all elements greater than it. The index of the pivot in the sorted list is then returned.

2. **`while left < right`**: This loop keeps running until the required k-th element is found or `left` and `right` pointers converge.

3. **The If-Else Construct**: It determines which half of the array to recurse into based on where the desired k-th element resides compared to the pivot.

#### Base Cases and Edge Cases:

- If the list `nums` is empty, this approach would raise an error.
  
- If `k` is greater than the length of the list, the approach would again fail.

#### Optimizing Clues:

For QuickSelect:

1. **Randomized Pivot**: To mitigate the worst-case scenario (especially for already sorted arrays), you could choose a random pivot. This ensures that the algorithm doesn't consistently hit the worst-case scenario, making its average time complexity close to O(n).

2. **Iterative Approach**: Instead of recursion, you could use an iterative approach with a loop to further save on call stack space.

#### Mnemonics:

- **"Sort for Simplicity"**: When in doubt, sorting the array provides a straightforward, though not always optimal, approach to many problems.

- **"Quickly Select with a Pivot"**: Remember that QuickSelect uses a pivot, just like QuickSort, but only ventures into one half of the array.

Both approaches to this problem have their merits. Sorting is simple and always works but might be suboptimal for larger lists. QuickSelect can be faster on average but has potential pitfalls in its worst-case scenario. Knowing when to use which approach is key to efficient problem-solving.