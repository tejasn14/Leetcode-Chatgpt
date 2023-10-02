---
kanban-plugin:
---
---
[LC Link](https://leetcode.com/problems/top-k-frequent-elements/)
### First Principles Thinking

The problem is to find the top \( k \) most frequent elements in an integer array. Constraints are:

1. The array's length \( n \) is at least 1 and at most \( 10^5 \).
2. Each element in the array can range from \( -10^4 \) to \( 10^4 \).
3. \( k \) is between 1 and the number of unique elements in the array.

The fundamental task is to identify the frequency of each element and then find the \( k \) most frequent elements.

### Brute Force Approach

#### Explanation

- Calculate the frequency of each element.
- Sort the elements by their frequency.
- Pick the top \( k \) elements.

#### Programming/Coding Principles

- Use a dictionary to store the frequency count of each element.
- Sort the dictionary by value.

#### Python Code

```python
def topKFrequent(nums, k):
    sorted_freq = sorted(Counter(nums).items(), key=lambda x: x[1], reverse=True)
    return [x[0] for x in sorted_freq[:k]]
```

#### Time and Space Complexity Analysis

- Time Complexity: \( O(n \log n) \) for sorting.
- Space Complexity: \( O(n) \) for storing frequencies.

### Optimized Approaches

#### Approach 1: Using Heap

##### Intuition

- Use a min-heap to keep track of the top \( k \) elements as you iterate through the frequency dictionary.

##### Transition from Brute Force

- Instead of sorting all the elements, we use a heap to keep only the top \( k \) elements.

##### Programming/Coding Principles

- Use a heap data structure to maintain the top \( k \) frequent elements.

##### Python Code

```python
import heapq

def topKFrequent(nums, k):
    freq = {}
    for num in nums:
        if num not in freq:
            freq[num] = 0
        freq[num] += 1
    
    heap = []
    for num, count in freq.items():
        heapq.heappush(heap, (count, num))
        if len(heap) > k:
            heapq.heappop(heap)

    return [num for count, num in heap]
```

##### Time and Space Complexity Analysis

- Time Complexity: \( O(n \log k) \)
- Space Complexity: \( O(n + k) \) 

### Strategies and Techniques

1. **Heap**: This is an excellent way to keep track of the \( k \) largest/smallest elements as you iterate through an array.
2. **Hash Table**: Useful for counting frequencies of elements in a collection.

The follow-up requirement asks for a time complexity better than \( O(n \log n) \), and the optimized approach using a heap achieves that. We used the heap to keep track of the top \( k \) elements with \( O(n \log k) \) time complexity, which is better than \( O(n \log n) \).

#### Approach 2: Bucket Sort-Like Approach

##### Intuition

- Utilize a bucket-sort-like approach to efficiently find the top \( k \) frequent elements.

##### Transition from Brute Force

- This approach replaces the sorting step with a more efficient bucket-sort-like mechanism.

##### Programming/Coding Principles

- Utilize a dictionary to store element frequencies.
- Create a list of buckets to group elements with the same frequency.
- Iterate through buckets in reverse order to find the top \( k \) frequent elements.

##### Python Code

```python
def topKFrequent(nums, k):
    # Step 1: Create a dictionary to store the frequency of each element
    frequency_map = Counter(nums)
    
    # Step 2: Create a list of buckets to store elements with the same frequency
    buckets = [[] for _ in range(len(nums) + 1)]

    # Step 3: Place elements in their respective buckets based on frequency
    for num, freq in frequency_map.items():
        buckets[freq].append(num)

    # Step 4: Iterate through buckets in reverse order to find the top K frequent elements
    result = []
    for i in range(len(buckets) - 1, -1, -1):
        result.extend(buckets[i])
        if len(result) >= k:
            break

    return result[:k]
```

##### Time and Space Complexity Analysis

- Time Complexity: \( O(n) \)
- Space Complexity: \( O(n) \) 

### Strategies and Techniques

1. **Bucket Sort**: A technique that distributes elements into buckets based on their values, reducing the need for a complete sorting operation.
2. **Dictionary**: Used to store and access the frequency of elements efficiently.
3. **List of Buckets**: Groups elements with the same frequency into separate buckets.
4. **Reverse Iteration**: Iterates through buckets in reverse order to find the top \( k \) frequent elements efficiently.

This optimized approach achieves a time complexity of \( O(n) \) and a space complexity of \( O(n) \), making it more efficient than the sorting-based approach when finding the top \( k \) frequent elements.

---
---
### Neetcode

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        freq = [[] for i in range(len(nums) + 1)]

        for n in nums:
            count[n] = 1 + count.get(n, 0)
        for n, c in count.items():
            freq[c].append(n)

        res = []
        for i in range(len(freq) - 1, 0, -1):
            for n in freq[i]:
                res.append(n)
                if len(res) == k:
                    return res

        # O(n)

```

### First Principles Thinking

1. **Fundamental Truths**: We have an array of integers, `nums`, and an integer `k`. We need to return the `k` most frequent elements in `nums`.

2. **Constraints**: 
    - We are interested in finding elements that are most frequent.
    - Frequency counts of integers can vary and may be the same for multiple integers.
    - Return exactly `k` elements.

3. **Building Up**: 
    - The basic approach would involve counting the frequency of each element.
    - We then sort these elements based on their frequency.
    - Finally, we select the top `k` elements.

#### Intuition

1. **Frequency Counts**: Naturally, the first step is to find the frequency of each element.
2. **Organizing Frequencies**: The next step would be to organize these frequencies in a way that allows us to easily select the top `k`.
  
### Explanation of Solution

#### Programming/Coding Principles

1. **Raw Algorithm**:
    - Use a dictionary to store the frequency of each number.
    - Use a list of lists where the index corresponds to frequency and the value is a list of numbers with that frequency.
    - Traverse the list of lists in reverse to gather the `k` most frequent numbers.

2. **Data Structure**:
    - Dictionary (`count`): To map each number to its frequency.
    - List of Lists (`freq`): To group numbers by frequency.
    
3. **Looping Structure**: 
    - A `for` loop to count frequencies.
    - Another `for` loop to populate the frequency list.
    - A third `for` loop to collect the `k` most frequent numbers.

4. **Variables**:
    - `count`: To keep track of each element's frequency.
    - `freq`: To organize elements by frequency.
    - `res`: To collect the top `k` most frequent numbers.
  
5. **Update Criteria**: 
    - Update `count` for each element.
    - Populate `freq` based on `count`.
  
6. **Base Case**: There is no base case in the given loops.
  
7. **Edge Cases**: 
    - If `nums` is empty, return an empty list.
    - If multiple numbers have the same frequency, any can be chosen.

#### Python Code

Your code captures the above algorithm well:

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        freq = [[] for i in range(len(nums) + 1)]
        for n in nums:
            count[n] = 1 + count.get(n, 0)
        for n, c in count.items():
            freq[c].append(n)
        res = []
        for i in range(len(freq) - 1, 0, -1):
            for n in freq[i]:
                res.append(n)
                if len(res) == k:
                    return res
```

#### Time and Space Complexity

- **Time Complexity**: \( O(N) \), where \( N \) is the length of `nums`.
  
- **Space Complexity**: \( O(N) \) for the `count` and `freq` data structures.

### Strategies and Techniques

1. **Frequency Mapping**: Mapping elements to their frequencies is often the first step in frequency-related problems.
  
2. **Bucketing by Frequency**: Grouping numbers by their frequency makes it easier to select the top `k`.

### Mnemonics

1. **"Count-Freq-Collect"**: First, we "count" the frequency, then organize them in "freq" buckets, and finally "collect" the top `k`.

2. **"Reverse for Relevance"**: We traverse the frequency list in reverse because the most frequent items are at the end.

By understanding the first principles, the raw algorithm, and the programming choices made, you can better generalize this strategy to solve similar problems that require frequency counts or selecting elements based on their frequency.
