---

---
---
[LC Link](https://leetcode.com/problems/find-median-from-data-stream/)

---
---

```python
class MedianFinder:
    def __init__(self):
        """
        initialize your data structure here.
        """
        # two heaps, large, small, minheap, maxheap
        # heaps should be equal size
        self.small, self.large = [], []  # maxHeap, minHeap (python default)

    def addNum(self, num: int) -> None:
        if self.large and num > self.large[0]:
            heapq.heappush(self.large, num)
        else:
            heapq.heappush(self.small, -1 * num)

        if len(self.small) > len(self.large) + 1:
            val = -1 * heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        if len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -1 * val)

    def findMedian(self) -> float:
        if len(self.small) > len(self.large):
            return -1 * self.small[0]
        elif len(self.large) > len(self.small):
            return self.large[0]
        return (-1 * self.small[0] + self.large[0]) / 2.0

```

The given class `MedianFinder` is designed to find the median from a continuous stream of numbers. It involves using two heaps - a max heap and a min heap - to maintain the numbers in a way that allows us to efficiently find the median. Let's break it down using the 1st principles approach:

### Problem-Solving Approach:

**Understanding the Problem**: The median of a list is the middle number when the numbers are in ascending order. If the list has an odd length, the median is the central number; if the list has even length, the median is the average of the two central numbers. With a stream of numbers, we cannot simply maintain a sorted list efficiently. Hence, a new approach is needed.

### Solving Intuition:

**Dual Heap Approach**: The intuition is to maintain the smallest half of the numbers in a max heap and the largest half in a min heap. If we ensure that the heaps are balanced (either of the same size or at most a difference of one), then the median is either:
- The top of the max heap (for odd total numbers)
- The average of the tops of both heaps (for even total numbers).

### Key Components:

#### 1. `__init__`:

- We initialize two empty lists `self.small` and `self.large`. The list `self.small` acts as a max heap by storing its values as negatives. Python's `heapq` module provides min-heap functionality by default.

#### 2. `addNum`:

- Depending on the value of `num`, it's pushed to either the max heap (`self.small`) or the min heap (`self.large`).
  
- After adding `num`, we check if the heaps are balanced. If not, we pop and push values between them until they're balanced.

#### 3. `findMedian`:

- Based on the sizes of the heaps, it returns the median. If one heap has more elements than the other, the top of that heap is the median. Otherwise, the median is the average of the tops of both heaps.

### Time Complexity:

1. `addNum`: O(log n) - Insertion into a heap is O(log n).
2. `findMedian`: O(1) - We're directly accessing the tops of the heaps.

### Space Complexity:

O(n) - We're storing all the numbers in the two heaps.

### Why The Code Is Written The Way It Is:

1. **Why `self.small` and `self.large`?**: They efficiently partition the data into two halves, making median retrieval efficient.

2. **Why `-1 * num`?**: Python's heapq doesn't support max heaps. Multiplying by -1 is a trick to simulate max heap functionality.

3. **Balancing Heaps after adding `num`**: Ensures that we can efficiently get the median value in constant time.

### Base Cases and Edge Cases:

1. **Adding to empty heaps**: Initially, everything goes to `self.small` (or can be programmed to go to `self.large`).

2. **Heaps of different sizes**: The code ensures the heaps differ in size at most by one.

### Optimizing Clues:

1. **Lazy Deletion**: If we were to support deletions, instead of immediately deleting an element, we could mark it and only remove it when it reaches the top of a heap.

### Mnemonics to Remember:

1. **"Two Heaps are Neat"**: Remember the dual heap strategy - one max heap for the smaller half, and one min heap for the larger half.

2. **"Flip to Max"**: By multiplying numbers by -1, we flip the functionality of a min heap to act as a max heap in Python.

3. **"Balance to Peek"**: By balancing the heaps after each addition, we can instantly peek at the median.

In essence, the `MedianFinder` leverages the properties of heaps to efficiently track and return the median from a continuous stream of numbers. It's a classic problem-solving example illustrating the utility of data structures in algorithm design.