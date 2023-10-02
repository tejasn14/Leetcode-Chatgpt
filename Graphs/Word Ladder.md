---

---
---
[LC Link](https://leetcode.com/problems/word-ladder/)

---
---

```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        if endWord not in wordList:
            return 0

        nei = collections.defaultdict(list)
        wordList.append(beginWord)
        for word in wordList:
            for j in range(len(word)):
                pattern = word[:j] + "*" + word[j + 1 :]
                nei[pattern].append(word)

        visit = set([beginWord])
        q = deque([beginWord])
        res = 1
        while q:
            for i in range(len(q)):
                word = q.popleft()
                if word == endWord:
                    return res
                for j in range(len(word)):
                    pattern = word[:j] + "*" + word[j + 1 :]
                    for neiWord in nei[pattern]:
                        if neiWord not in visit:
                            visit.add(neiWord)
                            q.append(neiWord)
            res += 1
        return 0

```

Alright, this problem, at its core, is a classic word ladder problem. The challenge is to transform the `beginWord` to the `endWord` by changing just one letter at a time, such that every transformed word must exist in the provided `wordList`.

### First Principles of Problem-Solving:

**Core Principle**: We need to consider every word as a node and there exists an edge between two words if they differ by one letter. The problem then turns into finding the shortest path between `beginWord` (source node) and `endWord` (destination node) in this graph.

### Solving Intuition:

1. **Graph Modeling**: Given the core principle, our first step is to model the problem as a graph problem. 
2. **Breadth First Search (BFS)**: Once we've represented the problem in terms of a graph, BFS becomes an intuitive choice to find the shortest path between nodes.

### Raw Algorithm:

1. Generate all possible generic forms for each word in `wordList` by replacing each letter with '*'. 
2. Group words with the same generic form together, which means they differ by one letter.
3. Use BFS starting from `beginWord`, and at each level explore all the neighboring words. This is done until we find the `endWord` or we have explored all words.

### Why the Code is Written This Way:

1. **Neighborhood Computation (`nei`)**: 
   - A dictionary that holds generic forms of words as keys (like 'h*t' for 'hot') and all words with that generic form as values.
   - It’s used to quickly find neighboring words. 
   
2. **BFS Implementation (`q` and `while q`)**: 
   - The queue (`q`) is initialized with `beginWord` and is used to keep track of words to process.
   - `while q` ensures we continue processing as long as there are words in the queue.
   - For each word, we find its neighbors by generating its generic forms and looking them up in the `nei` dictionary.
   
3. **Visited Words (`visit`)**: 
   - A set to keep track of words we've already processed to avoid cycles.
   
4. **Result Count (`res`)**: 
   - Tracks the transformation sequence length.
   - Initialized to 1 because we start with the `beginWord`.

5. **Pattern Generation**: 
   - For each word, we generate all its possible patterns by replacing each character with '*'.
   - This helps us find all its neighboring words in the graph.

### Time Complexity:
- The outer loop runs for each word, and the inner loop processes each character in the words, so it's **O(N*K)**, where N is the number of words and K is the length of each word.

### Space Complexity:
- The space is dominated by the `nei` dictionary and the BFS queue. In the worst case, both can grow up to the number of words in the `wordList`, hence **O(N*K)**.

### Base Cases and Edge Cases:

**Base Cases**:
- If `endWord` isn’t in the `wordList`, we can instantly return 0.

**Edge Cases**:
- All words are of the same length, so no edge cases concerning varying lengths.
- If BFS completes without finding the `endWord`, return 0 indicating no valid transformation sequence.

### Optimizing Clues:

- To further optimize, one could implement a bi-directional BFS search. Start BFS from both the `beginWord` and `endWord` and stop when they meet in the middle. This often reduces the search space and makes BFS faster in practice.

### Mnemonics to Remember:

- **"One Letter Bridge"**: To remember that words with one letter difference are connected or neighboring in the graph.
- **"Word to Graph"**: As a reminder that this word problem can be modeled and solved as a graph traversal problem.
- **"BFS for Shortest"**: BFS helps in finding the shortest path (or transformation sequence) in the graph.

In summary, this solution transforms a word transformation problem into a graph traversal problem by considering words as nodes and using BFS to find the shortest transformation sequence. The creation of a pattern-based neighbor dictionary (`nei`) is a smart move to optimize the neighbor lookup during BFS traversal.