---
kanban-plugin:
---
---
[LC Link](https://leetcode.com/problems/valid-anagram/)
### First Principles Thinking

The problem asks us to find two numbers in the array that sum up to a specific target value. The constraints are:

1. The array contains at least 2 and at most 10^4 elements.
2. Each element's value can range from -10^9 to 10^9.
3. Only one valid answer exists for each input.

The fundamental task is to identify two numbers in the array that add up to the given target.

### Brute Force Approach:

#### Explanation:

- Loop through each pair of numbers and check if they add up to the target. If they do, return their indices.

#### Programming/Coding Principles:

#### Python Code:

```python
def isAnagram(s: str, t: str) -> bool:
    if len(s) != len(t):
        return False
    
    counter = [0] * 26
    for char in s:
        counter[ord(char) - ord('a')] += 1

    for char in t:
        counter[ord(char) - ord('a')] -= 1

    return all(x == 0 for x in counter)
```

#### Time and Space Complexity Analysis:

- Time Complexity: \(O(n)\)
- Space Complexity: \(O(1)\), as we're using only a constant amount of extra space. Number of characters in english alphabet are limited

### Optimized Approaches:

#### Approach 1: Sorting

```python
def isAnagram(s: str, t: str) -> bool:
    return sorted(s) == sorted(t)
```

#### Time and Space Complexity Analysis:

- Time Complexity: \(O(n log n)\) because of sorting.
- Space Complexity: \(O(1)\), as we're not using any extra space.

#### Approach 2: Using a Hash Table

##### Intuition:

- Use a hash table to store elements that we have visited. For each new element, we can easily check if its complement (target - element) exists in the hash table.

##### Transition from Brute Force:

- Instead of using two nested loops, we will use a single loop and a hash table to find the answer in one pass.

##### Programming/Coding Principles:

- We'll use a hash table to keep track of the numbers we have seen so far and their respective indices.

##### Python Code:

```python
from collections import Counter

def isAnagram(s: str, t: str) -> bool:
    return Counter(s) == Counter(t)
```
##### Time and Space Complexity Analysis:

- Time Complexity: \(O(n)\), as we only loop through the array once.
- Space Complexity: \(O(n)\), for the hash table.

### Strategies and Techniques:

1. **Sorting**
2. **Hash Table**: Extremely useful when you need quick look-up to check if an element exists or to store the mapping of elements to some value (in this case, their indices).

---
---
### Neetcode

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)
        return countS == countT

```

### First Principles Thinking

1. **Fundamental Truths**: The problem is to check whether two strings `s` and `t` are anagrams of each other.
   
2. **Constraints**: 
    - Anagrams are strings that contain the same characters with the same frequencies, just possibly in a different order.
    - It is not stated, but we assume that the inputs are case-sensitive.

3. **Building Up**: 
    - The core idea is that anagrams will have the same character frequency distribution. Therefore, comparing the frequency distributions of two strings will determine if they are anagrams.

#### Intuition and Solving Intuition

1. **Pattern Recognition**: If `s` and `t` have different lengths, they can't be anagrams. Further, if their character frequencies are different, they can't be anagrams either.
  
2. **Optimization Insight**: A single pass can be used to build the character frequency dictionary for each string.

3. **Time Complexity Constraint**: The aim is to check anagrams in \(O(N)\) where \(N\) is the length of the strings.

### Explanation of Solution

#### Programming/Coding Principles

1. **Raw Algorithm**:
    - Check if lengths are the same, else return False.
    - Initialize two empty dictionaries for counting characters in `s` and `t`.
    - Traverse both strings simultaneously, populating the dictionaries.
    - Compare the dictionaries.

2. **Data Structure**: 
    - Dictionary (`countS` and `countT`): To keep the count of each character in both strings.

3. **Looping Structure**: 
    - A single `for` loop: It traverses both strings to populate the dictionaries.
  
4. **Variables**: 
    - `countS`, `countT`: Dictionaries to hold character frequencies.
    - `i`: Loop variable.
  
5. **Update Criteria**: 
    - Update count dictionaries using `dict.get(key, default)`.
  
6. **Base Case and Edge Case**: 
    - Base Case: If lengths are different, return False immediately.
    - Edge Case: Case-sensitivity and special characters can be considered if not mentioned in the problem description.

#### Time and Space Complexity

- **Time Complexity**: \(O(N)\) — Single pass through each of the strings.
- **Space Complexity**: \(O(C)\) — Where \(C\) is the number of unique characters in the strings.

### Strategies and Techniques

1. **Character Frequency Matching**: Use dictionaries to match character frequencies when solving anagram or similar string-related problems.
  
2. **Length Check Optimization**: Always check length equality first as it's a quick operation and can rule out being anagrams instantly.

### Mnemonics

1. **"Equal Length, Equal Content"**: Remember that anagrams must have the same length and the same content but possibly in a different order.

2. **"Count to Match"**: The technique revolves around counting the frequency of each character to check for a match.

3. **"Dict Equality"**: Simply comparing two dictionaries (`countS == countT`) in Python checks for key-value pair equality, which is a handy Python feature to remember.

By focusing on the dictionary-based approach to compare frequencies, you build a foundation that extends to other string manipulation problems as well, where character counting could be crucial.