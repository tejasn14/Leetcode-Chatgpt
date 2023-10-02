---

---
---
[LC Link](https://leetcode.com/problems/longest-consecutive-sequence/)
### First Principles Thinking

For this problem, the primary aim is to find the longest consecutive sequence in an unsorted array of integers. The fundamental principle is that we're interested in sequences, and sequences of numbers are naturally ordered. A naive approach would consider sorting the array, but we are constrained to \(O(n)\) time complexity.

### Brute Force Approach

#### Explanation

A naive way to do this is to sort the array first and then scan for the longest continuous sequence. However, sorting takes \(O(n\log n)\) time, so this approach doesn't meet the requirement.

#### Programming/Coding Principles

- Sort the array
- Initialize counters to keep track of the current and longest sequence lengths.
- Traverse the array, checking for consecutive elements.

#### Python Code

```python
def longestConsecutive(nums):
    if not nums:
        return 0
    
    nums.sort()
    longest_streak = 1
    current_streak = 1
    
    for i in range(1, len(nums)):
        if nums[i] != nums[i-1]:
            if nums[i] == nums[i-1] + 1:
                current_streak += 1
            else:
                longest_streak = max(longest_streak, current_streak)
                current_streak = 1
                
    return max(longest_streak, current_streak)

# Time Complexity: O(n log n)
# Space Complexity: O(1)
```

### Optimized Approach

#### Intuition

To improve, we can use a HashSet to achieve \(O(n)\) time complexity. The HashSet allows us to check for a number's presence in constant time.

#### Programming/Coding Principles

- Insert all numbers in a HashSet.
- For each number, check if it is the starting number of a sequence (i.e., `n-1` doesn't exist).
- If it is, traverse the HashSet to find the length of its sequence.

#### Python Code

```python
def longestConsecutive(nums):
    if not nums:
        return 0
    
    num_set = set(nums)
    longest_streak = 0
    
    for num in num_set:
        if num - 1 not in num_set:
            current_num = num
            current_streak = 1
            
            while current_num + 1 in num_set:
                current_num += 1
                current_streak += 1
                
            longest_streak = max(longest_streak, current_streak)
            
    return longest_streak

# Time Complexity: O(n)
# Space Complexity: O(n)
```

### Strategies and Techniques

1. **HashSet for Quick Lookup**: HashSets provide \(O(1)\) lookups, which is crucial for keeping the time complexity within bounds.
2. **Sequence Initialization**: Checking if the current number is the start of a sequence avoids duplicate work.

This approach can be generally applied to similar problems where you need to find sequences or sets within an array quickly.

---
---
#### NEETCODE

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numSet = set(nums)
        longest = 0

        for n in numSet:
            # check if its the start of a sequence
            if (n - 1) not in numSet:
                length = 1
                while (n + length) in numSet:
                    length += 1
                longest = max(length, longest)
        return longest

```

### First Principles Thinking

1. **Fundamental Truths**: The problem is to find the longest consecutive sequence in an unsorted array of integers.

2. **Constraints**: The sequence has to be consecutive, and integers can be negative, positive, or zero.

3. **Building Up**: Since the sequence must be consecutive, each element \( x \) in a sequence will be followed by \( x+1 \).

#### Intuition

1. **Element Discovery**: To find the longest sequence, we don't need to examine each number more than once.

2. **Sequence Initialization**: Start from the smallest element in each possible sequence and then keep checking for the next consecutive elements.

### Explanation of Solution

#### Explanation

1. **Algorithm**: Store all numbers in a set for fast lookup. Iterate through each unique number. If a number is the smallest in its sequence, check how long the sequence is.

#### Programming/Coding Principles

1. **Raw Algorithm**:
    - Convert the list to a set.
    - For each number \( n \) in the set, if \( n-1 \) is not present, then \( n \) is the smallest element of its sequence.
    - Starting from \( n \), keep checking for \( n+1, n+2, \ldots \) in the set until a number is not found.
    - Keep track of the longest sequence found.

2. **Data Structure**: 
    - Set (`numSet`): For fast \( O(1) \) lookups. 
    
3. **Looping Structure**: 
    - One loop iterating through the unique numbers in `numSet`.
    - Inner `while` loop to find each sequence.

4. **Variables**:
    - `longest`: to store the longest sequence length.
    - `length`: to store the current sequence length.
  
5. **Update Criteria**: Increment `length` as you find each next number in the sequence.
  
6. **Base Case**: \( n-1 \) not in set, which makes \( n \) the smallest element in the sequence.
  
7. **Edge Cases**: Empty list, list with one element, or list with duplicate elements.

#### Python Code

Your code is already quite efficient:

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numSet = set(nums)
        longest = 0
        for n in numSet:
            if (n - 1) not in numSet:
                length = 1
                while (n + length) in numSet:
                    length += 1
                longest = max(length, longest)
        return longest
```

#### Time and Space Complexity

- **Time Complexity**: \( O(N) \), where \( N \) is the length of `nums`.
  
- **Space Complexity**: \( O(N) \), for the set storage.

### Strategies and Techniques

1. **Set for Quick Lookup**: Whenever you need fast lookups and don't care about order, sets are useful.

2. **Sequence Start Point**: To find sequences, look for the starting point. Here, it's the smallest number of each sequence.

### Mnemonics

1. **"Set-Start-Stretch"**: We use a set to find the start of each sequence and then stretch it to find its length.

2. **"One-Way Check"**: Remember, we only need to check one way (i.e., \( n+1, n+2, \ldots \)) because we are starting from the smallest element of each sequence.

By internalizing these first principles, code logic, and strategies, you can apply similar techniques to other problems that involve finding sequences or subsets within a dataset.