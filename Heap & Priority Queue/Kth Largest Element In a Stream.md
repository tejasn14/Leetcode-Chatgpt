---

---
---
[LC Link](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

---
---

```python
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        # minHeap w/ K largest integers
        self.minHeap, self.k = nums, k
        heapq.heapify(self.minHeap)
        while len(self.minHeap) > k:
            heapq.heappop(self.minHeap)

    def add(self, val: int) -> int:
        heapq.heappush(self.minHeap, val)
        if len(self.minHeap) > self.k:
            heapq.heappop(self.minHeap)
        return self.minHeap[0]

```

Alright, let's deep dive into this problem. The task is to design a class to find the kth largest element in a stream. New numbers can be added to the stream with the `add` method, and each time a new number is added, the kth largest number should be returned.

### First Principles:

1. **Maintaining k Largest Elements**: If we maintain the k largest elements, then the smallest among these k elements will be the kth largest in the stream.

2. **Min-Heap for Efficient Operations**: A min-heap can efficiently get the smallest element (in constant time) and insert a new element (in logarithmic time). These properties are key to solving this problem.

### Solving Intuition:

- **Why a Min-Heap for k Largest Numbers?**: At first glance, using a max-heap might seem more intuitive since we want k largest numbers. However, with a min-heap of size k, the root of the heap will always be the kth largest element, which is exactly what we want. This setup makes it easy to maintain the k largest numbers efficiently.

### Time Complexity:

- **Initialization**: O(nlogk), where n is the length of the `nums` list. Each insertion into the heap takes O(logk), and we do this n times.
  
- **Add Method**: O(logk) for inserting a new number into the heap and another O(logk) for possibly removing the smallest number from the heap.

### Space Complexity:

- **Overall Space**: O(k) for maintaining the min-heap.

### Raw Algorithm:

1. **Initialization**:
   - Convert the input `nums` list into a min-heap.
   - While the size of the heap is more than k, pop elements from the heap. This ensures that only the k largest elements remain in the heap.

2. **Add Method**:
   - Push the new value into the min-heap.
   - If the size of the heap exceeds k, pop the smallest element.
   - Return the smallest element in the heap (which is the root of the heap).

### Why The Code Is Written The Way It Is:

- **heapq Module for Min-Heap Operations**: Python's standard library provides the `heapq` module which transforms a list into a heap in-place and provides methods like `heappop` and `heappush` for heap operations.

- **While Loop in Initialization**: The while loop ensures that only the k largest elements from the `nums` list remain in the min-heap. It continually pops the smallest element until the heap's size is equal to k.

- **If Condition in the Add Method**: This condition ensures that the size of the heap never exceeds k.

### Base Cases and Edge Cases:

- **Empty nums List**: If the `nums` list is empty during initialization, the min-heap remains empty. The add method can still be used to insert numbers into the stream.

- **k Larger Than nums Size**: In such cases, all the numbers from the `nums` list will remain in the heap after initialization.

### Optimizing Clues:

1. **Maintain Heap Size**: Always ensure that the size of the heap doesn't exceed k. This ensures that heap operations are always O(logk) and not O(logn).

### Mnemonics:

- **"Keep the Heap K-lean"**: Always maintain the heap's size to k for efficient operations.
  
- **"Heap for the Peak"**: Using a min-heap to efficiently find the kth largest number in the stream.

In essence, this solution leverages the properties of min-heaps to maintain the k largest numbers in a stream efficiently. Each time a new number is added, it ensures that only the k largest numbers are kept, making it easy to return the kth largest number.