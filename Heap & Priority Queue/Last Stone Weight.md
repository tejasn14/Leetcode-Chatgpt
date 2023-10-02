---

---
---
[LC Link](https://leetcode.com/problems/last-stone-weight/)

---
---

```python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        stones = [-s for s in stones]
        heapq.heapify(stones)

        while len(stones) > 1:
            first = heapq.heappop(stones)
            second = heapq.heappop(stones)
            if second > first:
                heapq.heappush(stones, first - second)

        stones.append(0)
        return abs(stones[0])

# There's a private _heapify_max method.
# https://github.com/python/cpython/blob/1170d5a292b46f754cd29c245a040f1602f70301/Lib/heapq.py#L198
class Solution(object):
    def lastStoneWeight(self, stones):
        heapq._heapify_max(stones)
        while len(stones) > 1:
            max_stone = heapq._heappop_max(stones)
            diff = max_stone - stones[0]
            if diff:
                heapq._heapreplace_max(stones, diff)
            else:
                heapq._heappop_max(stones)
        
        stones.append(0)
        return stones[0]

```

Alright, let's dive into the problem.

## Problem Statement:

We have a collection of stones, each stone has a positive integer weight. Every turn, we choose the two heaviest stones and smash them together. Suppose the stones have weights `x` and `y` with `x <= y`. The result of this smash is:

1. If `x == y`, both stones are totally destroyed.
2. If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y-x`.

The process continues until there is either one stone left or no stones left. Return the weight of the last stone (if it exists) or `0` otherwise.

## First Principles:

1. **Prioritizing Maximum Elements**: We need to continually select the two heaviest stones. This makes the problem a perfect candidate for a max heap.
2. **Smashing Stones Together**: Once we have the two heaviest stones, we'll smash them and potentially push the result back into the heap.

## Solving Intuition:

The problem lends itself well to using a heap because we continually need to identify and remove the largest elements. If we try to do this with an array or list, the process would be inefficient as finding/removing an element takes O(n) time. With a max heap, these operations can be done in O(logn) time.

### Time Complexity:

- **Initialization**: O(nlogn), due to inserting n elements into the heap.
- **While loop**: Each loop iteration takes O(logn) due to the popping of two maximum elements and potential insertion of their difference. In the worst case, we do this n times, resulting in a complexity of O(nlogn).

Thus, the overall time complexity is O(nlogn).

### Space Complexity:

- **Overall Space**: O(n) for the heap.

## Raw Algorithm:

1. Convert the stones into a max heap.
2. Continually pop the two maximum elements (stones) from the heap.
3. If their weights are different, push their difference back into the heap.
4. Continue until there's at most one stone left in the heap.
5. Return the remaining stone's weight, or `0` if no stone is left.

## Why The Code Is Written The Way It Is:

1. **Why use negative values in the first solution?**: Python's standard library `heapq` provides a min heap by default. By inserting the negative of stone weights, we simulate a max heap behavior. 
   
2. **Why the while loop?**: The loop continues as long as there are more than one stone left, ensuring all stones are processed.

3. **Why `stones.append(0)`?**: This is a neat trick to ensure there's always an element to return at the end. If the heap is empty, this ensures we return `0`.

4. **Why the second solution?**: While the first solution cleverly circumvents the absence of a max heap in the standard library, the second solution uses Python's private `_heapify_max` method to handle max heaps directly. While this solution is more intuitive, using private methods (those prefixed with `_`) can be risky as they're considered internal and might change in future versions of Python.

## Base Cases and Edge Cases:

1. **All stones of the same weight**: In this case, all stones would be destroyed, resulting in a return value of `0`.

2. **Only one stone**: The function would simply return the weight of this stone.

3. **Two stones of different weights**: The result would be the difference of their weights.

## Optimizing Clues:

1. The first solution's use of negative values for simulating a max heap is clever but might not be immediately intuitive. If the programming language you're using supports max heap directly, consider using it for clarity.

2. The use of private heap methods in the second solution, while more straightforward, might not be stable across different Python versions.

## Mnemonics:

- **"Smash the Heaviest"**: Always pop the two heaviest stones from the heap.
  
- **"Negate to Elevate"**: In the first solution, negating stone weights elevates them to the top of the heap.

In essence, this problem revolves around efficiently identifying and processing the largest elements in a collection. By understanding the strengths of heaps and how they can quickly identify extreme values, the problem becomes relatively straightforward to solve.