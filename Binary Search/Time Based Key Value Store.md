---

---
---
[LC Link](https://leetcode.com/problems/time-based-key-value-store/)

---
---

```python
class TimeMap:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.keyStore = {}  # key : list of [val, timestamp]

    def set(self, key: str, value: str, timestamp: int) -> None:
        if key not in self.keyStore:
            self.keyStore[key] = []
        self.keyStore[key].append([value, timestamp])

    def get(self, key: str, timestamp: int) -> str:
        res, values = "", self.keyStore.get(key, [])
        l, r = 0, len(values) - 1
        while l <= r:
            m = (l + r) // 2
            if values[m][1] <= timestamp:
                res = values[m][0]
                l = m + 1
            else:
                r = m - 1
        return res

```

The problem you've shared appears to represent a time-based key-value store. The idea is to be able to set values for specific keys at specific times, and then query the last set value for a key before a given timestamp.

### First Principles:
A foundational idea here is that when we're trying to retrieve information based on certain criteria (like a timestamp), it's more efficient to have the data sorted by that criterion. This enables efficient lookup strategies like binary search.

### Solving Intuition:
1. We need to save multiple values for a single key, each associated with a unique timestamp.
2. If we query a value for a specific timestamp, we want the latest (highest) timestamp that is not greater than the query timestamp.

This naturally leads to the idea of saving values in a sorted order for each key, where the sorting criterion is the timestamp.

### Time/Space Complexity:
- **`set` method**:
  - Time complexity: O(1), since appending to a list is constant time.
  - Space complexity: O(1) for each call, but in total it will be O(T), where T is the total number of timestamps across all keys.
  
- **`get` method**:
  - Time complexity: O(log T) because of the binary search, where T is the number of timestamps for the key.
  - Space complexity: O(1).

### Raw Algorithm:
1. **For the `set` method**:
   - If the key isn't in the store, initialize it with an empty list.
   - Append the [value, timestamp] pair to the list associated with the key.

2. **For the `get` method**:
   - Use binary search to find the highest timestamp not greater than the query timestamp.

### Programming Approach:
1. **Data Structures**:
   - `self.keyStore`: A dictionary where the key is the actual key, and the value is a list of [value, timestamp] pairs. Lists in Python are dynamic arrays and are good for our use case because we can do constant-time appends and efficient lookups with binary search.
   
2. **`set` Method**:
   - We simply check if the key exists in `keyStore`. If it doesn't, we initialize it.
   - We then append the new value and its timestamp to the list associated with the key.

3. **`get` Method**:
   - First, we retrieve the list of values associated with the key. If the key isn't present, we get an empty list by default.
   - We then use binary search on the timestamps to get the desired value:
     - If the current midpoint's timestamp is less than or equal to the query timestamp, it's a potential answer. So, we update our result and move right (to potentially get a more recent valid timestamp).
     - Otherwise, we move left.
   
4. **Variables**:
   - `l` and `r` are used for binary search, indicating the current range.
   - `res` stores the current best value for the given timestamp.
   - `values` is the list of [value, timestamp] pairs associated with the key.
   - `m` is the midpoint of the current range.

5. **Base & Edge Cases**: 
   - If the key isn't in `keyStore`, `get` should return an empty string.
   - The final result of `get` is the last value we found that meets the criteria, which is why `res` is returned at the end.

### Optimizing Clues:
The current solution is quite optimized for the problem statement. The use of binary search ensures we're retrieving values in logarithmic time, which is efficient given the data structure chosen.

### Mnemonics to Remember:
**"TimeMap Retrieval:**
1. **Store in ascending timestamp order**.
2. **Use binary search for time-based lookup**.

Understanding this simplified structure will make it easier to recall how the TimeMap operates when dealing with time-based key-value storage problems.