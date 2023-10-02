---
kanban-plugin:
---
---
[LC Link](https://leetcode.com/problems/group-anagrams/)
### First Principles Thinking

The problem requires us to group anagrams from a given list of strings. An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, using all the original letters exactly once. The constraints are:

1. The number of strings is between 1 and \(10^4\).
2. The length of each string is between 0 and 100.
3. All characters in the strings are lowercase English letters.

The fundamental task is to identify words that are anagrams and group them together.

### Brute Force Approach

#### Explanation

- Compare each string with every other string to check if they are anagrams.

#### Programming/Coding Principles

- Use nested loops to compare each string with every other string.
- Use a dictionary to store the frequency of characters for each string for comparison.

#### Python Code

```python
def groupAnagrams(strs):
    groups = []
    visited = [False] * len(strs)
    
    for i in range(len(strs)):
        if visited[i]:
            continue
        
        anagrams = [strs[i]]
        visited[i] = True
        
        for j in range(i+1, len(strs)):
            if visited[j]:
                continue
            
            if sorted(strs[i]) == sorted(strs[j]):
                anagrams.append(strs[j])
                visited[j] = True
                
        groups.append(anagrams)
        
    return groups
```

#### Time and Space Complexity Analysis

- Time Complexity: (O(n^2 * k)) where \(n\) is the number of strings and \(k\) is the maximum length of a string.
- Space Complexity: \(O(n * k)\) to store the groups and the `visited` array.

### Optimized Approaches

#### Approach 1: Categorizing by Sorted String

##### Intuition

- Strings that are anagrams will have the same sorted string. We can use this sorted string as a key in a hash table.

##### Transition from Brute Force

- Instead of checking every pair of strings, sort each string and use it as a key to group all its anagrams together.

##### Programming/Coding Principles

- Use a hash table to store groups of anagrams. The sorted string will be the key.

##### Python Code

```python
def groupAnagrams(strs):
    anagrams = collections.defaultdict(list)

	for s in strs:
		sorted_str = ''.join(sorted(s))
		anagrams[sorted_str].append(s)
	
	return list(anagrams.values())
```

##### Time and Space Complexity Analysis

- Time Complexity: \(O(n * k \log k)\) due to sorting.
- Space Complexity: \(O(n * k)\) to store the hash table and groups.

### Strategies and Techniques

1. **Hash Table**: Useful for grouping elements based on some categorization.
2. **Sorting**: A good way to compare anagrams.
3. **Categorizing by a Key**: Using sorted strings or character counts as keys to identify anagrams.

This problem can be seen as a variant of string matching, similar to the previous problem about anagrams. The key difference here is that you're grouping multiple strings rather than comparing just two. The strategy of using sorting or hash tables to compare strings is a recurring technique in these types of problems.

---
---
### Neetcode

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ans = collections.defaultdict(list)

        for s in strs:
            count = [0] * 26
            for c in s:
                count[ord(c) - ord("a")] += 1
            ans[tuple(count)].append(s)
        return ans.values()

```

### First Principles Thinking

1. **Fundamental Truths**: Anagrams are words or phrases that contain the same characters but may be ordered differently.
   
2. **Constraints**: You're supposed to group these anagrams together. The order of output doesn't matter.

3. **Building Up**: Anagrams, by definition, will have the same characters with the same frequency in each string.

#### Intuition

1. **Character Frequency**: Since anagrams have the same characters with the same frequencies, using the frequencies as a form of "signature" for each string can effectively group anagrams together.

### Explanation of Solution

#### Explanation

1. **Algorithm**: Iterate through the list of strings. For each string, count the frequency of each character and use this count as a key in a dictionary. Append the string to the list of values for that key.

#### Programming/Coding Principles

1. **Raw Algorithm**: 
    - Initialize an empty defaultdict with lists as default values (`ans`).
    - For each string (`s`), initialize a counter (`count`) array of length 26 (for each letter of the alphabet).
    - Increment the corresponding index in `count` for each character in `s`.
    - Convert `count` to a tuple and use it as a key in `ans`. Append `s` to `ans[tuple(count)]`.

2. **Data Structure**: 
    - defaultdict for storing groups of anagrams.
    - List (`count`) for storing character frequencies.

3. **Looping Structure**: 
    - One loop to iterate through the list of strings (`strs`).
    - Nested loop to iterate through characters in each string (`s`).

4. **Update Criteria**: Increment the corresponding index in `count` for each character in `s`.
  
5. **Base Case**: N/A.
  
6. **Edge Cases**: An empty string or list can be processed in the same manner.

#### Python Code

Your code is efficient for this task. Here it is for reference:

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ans = collections.defaultdict(list)
        for s in strs:
            count = [0] * 26
            for c in s:
                count[ord(c) - ord("a")] += 1
            ans[tuple(count)].append(s)
        return ans.values()
```

#### Time and Space Complexity

- **Time Complexity**: \(O(NK)\), where \(N\) is the length of `strs`, and \(K\) is the maximum length of a string in `strs`.
  
- **Space Complexity**: \(O(NK)\), to store the `ans` defaultdict.

### Strategies and Techniques

1. **Character Counting**: For problems dealing with anagrams or string manipulation in general, character counting is often a useful technique.

2. **Dictionary with List Values**: When you need to group items based on some characteristic, a dictionary with list values is often useful.

### Mnemonics

1. **"Count and Group"**: Remember that you are counting the characters to group the anagrams.

2. **"26 for ABC"**: There are 26 slots in the `count` array for the 26 alphabets.

By understanding these principles and strategies, you'll be well-equipped to tackle similar problems that involve grouping or categorizing strings.
