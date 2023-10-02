---

---
---

[LC Link](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
### First Principles Thinking

The problem asks us to find the length of the longest substring without repeating characters. A substring is different from a subsequence, as a substring needs to be a contiguous block of characters. Therefore, we need to scan the string, continuously update the longest substring, and make sure the substring doesn't have duplicate characters. 

The fundamental truth in this problem is that a substring without repeating characters can be built by iterating through the string while maintaining a record of the unique characters we've already seen.

### Brute Force Approach

#### Explanation
We could start by checking all possible substrings one by one to see if they have no duplicate characters. If the substring contains all unique characters, we compare its length with the length of the current longest unique substring.

#### Programming/Coding Principles
1. **Raw Algorithm**: 
    1. Initialize a variable `max_length` to 0.
    2. Iterate through the string to find the start and end indices for each possible substring.
    3. For each substring, use a set to store unique characters and check for duplicates.
    4. Update `max_length` if a longer unique substring is found.
  
2. **Best Data Structure**: We could use a set to store unique characters in each substring.
  
3. **Loop**: Nested for-loops for generating all substrings.

4. **Base Case and Edge Cases**: If the string is empty, the length of the longest substring is 0.

#### Python Code

```python
def lengthOfLongestSubstring(s: str) -> int:
    max_length = 0
    
    for i in range(len(s)):
        for j in range(i + 1, len(s) + 1):
            substring = s[i:j]
            if len(set(substring)) == len(substring):
                max_length = max(max_length, len(substring))
                
    return max_length
```

#### Analysis
- **Time Complexity**: O(n^3) (Iterating through all substrings takes O(n^2) and checking each substring for uniqueness takes O(n))
- **Space Complexity**: O(min(n, m)) where m is the size of the character set.

### Optimized Approaches

#### First Optimization
We can optimize the process of checking if a substring has duplicate characters. Instead of generating the substring and then checking its characters, we can use a set to keep track of the unique characters as we iterate through the string.

##### Intuition and Train of Thought
1. Keep two pointers `start` and `end` at the beginning of the string.
2. Move `end` to the right and keep track of characters using a set.
3. If a repeating character is found, remove the character at `start` from the set and move `start` one position to the right.
4. The length of the unique substring is `end - start`.

##### Programming/Coding Principles
1. **Best Data Structure**: Use a set to store unique characters and two pointers for substring boundaries.
2. **Loop**: Single while-loop to iterate through the string.
3. **Update Criteria**: If a duplicate is found, update `start`.

##### Python Code
```python
def lengthOfLongestSubstring(s: str) -> int:
    start = 0
    max_length = 0
    char_set = set()
    
    for end in range(len(s)):
        while s[end] in char_set:
            char_set.remove(s[start])
            start += 1
        char_set.add(s[end])
        max_length = max(max_length, end - start + 1)
        
    return max_length
```

##### Analysis
- **Time Complexity**: O(2n) = O(n) (Each character could be visited twice by `start` and `end`)
- **Space Complexity**: O(min(n, m))

Let's continue to discuss if there's any room for further optimization and generalize strategies for solving this type of problem.

---
---

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        charSet = set()
        l = 0
        res = 0

        for r in range(len(s)):
            while s[r] in charSet:
                charSet.remove(s[l])
                l += 1
            charSet.add(s[r])
            res = max(res, r - l + 1)
        return res

```


The problem at hand asks for the length of the longest substring without repeating characters in a given string. The presented solution uses the sliding window approach to tackle this. Let's dive deeper into understanding the problem and the provided solution:

### 1. First Principles of Problem-solving Approach:

A naive way to approach this problem is to check each possible substring and see if it has unique characters. But this is inefficient and results in a time complexity of O(n^2).

### 2. Solving Intuition:

The optimal way to approach this is to use the sliding window approach. The main idea is to maintain a window in which all characters are unique, and then try to expand this window as much as possible by sliding it.

### 3. Time/Space Complexity:

**Time Complexity**: O(n) - Each character in `s` is visited at most twice (once by `l` and once by `r`).
**Space Complexity**: O(min(n, m)) - where `n` is the size of the string and `m` is the character set size (ASCII).

### 4. Raw Algorithm:

1. Initialize a set to keep track of unique characters in the current window.
2. Use two pointers `l` and `r` to represent the current window's left and right boundaries.
3. Move `r` to the right. If the character at `r` is not in the set, add it. Otherwise, remove the character at `l` from the set and move `l` to the right.
4. At each step, calculate the window size and update the maximum found.

### 5. Programming Approach:

- **Why set**: `charSet` is used to ensure constant-time check for the presence of characters and their quick removal. A set is perfect for this since it offers O(1) average time complexity for insertions, deletions, and lookups.
- **Why the `for` loop**: To iterate over the string `s` and move the `r` pointer.
- **Why the inner `while` loop**: This is used to shrink the window (move the `l` pointer) whenever a repeating character is found.
- **Why the variables**:
  - `charSet`: To store characters in the current window and ensure all are unique.
  - `l` and `r`: Pointers representing the left and right boundaries of the window.
  - `res`: To keep track of the longest substring found.

### 6. Base cases and Edge Cases:

- Base Case: If `s` is empty, the function immediately returns 0.
- Edge Case: The solution handles strings with all unique characters, all repeating characters, and anything in between. However, it assumes standard ASCII characters, but if one expects unicode characters, the space complexity might vary.

### 7. Further Optimizing Clues:

The provided solution is already optimal in terms of its time and space complexity for this problem. The set is the best choice as it provides constant time operations which are essential for the efficiency of the sliding window approach.

### 8. Mnemonics to Remember the Solution:

**"Slide and Track Uniques!"**:
- Use a set (`charSet`) to track unique characters in the current window.
- Slide through the string with `l` and `r`.
- Shrink the window with `l` if a repeating character is encountered.

In summary, the solution efficiently finds the longest substring without repeating characters by sliding through the string and keeping track of unique characters using a set.

