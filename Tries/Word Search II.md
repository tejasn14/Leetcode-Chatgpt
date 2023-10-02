---

---
---
[LC Link](https://leetcode.com/problems/word-search-ii/)

---
---

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isWord = False
        self.refs = 0

    def addWord(self, word):
        cur = self
        cur.refs += 1
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
            cur.refs += 1
        cur.isWord = True

    def removeWord(self, word):
        cur = self
        cur.refs -= 1
        for c in word:
            if c in cur.children:
                cur = cur.children[c]
                cur.refs -= 1


class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        root = TrieNode()
        for w in words:
            root.addWord(w)

        ROWS, COLS = len(board), len(board[0])
        res, visit = set(), set()

        def dfs(r, c, node, word):
            if (
                r not in range(ROWS) 
                or c not in range(COLS)
                or board[r][c] not in node.children
                or node.children[board[r][c]].refs < 1
                or (r, c) in visit
            ):
                return

            visit.add((r, c))
            node = node.children[board[r][c]]
            word += board[r][c]
            if node.isWord:
                node.isWord = False
                res.add(word)
                root.removeWord(word)

            dfs(r + 1, c, node, word)
            dfs(r - 1, c, node, word)
            dfs(r, c + 1, node, word)
            dfs(r, c - 1, node, word)
            visit.remove((r, c))

        for r in range(ROWS):
            for c in range(COLS):
                dfs(r, c, root, "")

        return list(res)

```

This solution attempts to find all the words from a list of words that can be formed by tracing paths on a board (similar to the game "Boggle"). The primary data structure used here is a `Trie`, but the solution has a twist in the implementation by using a reference counter (`refs`) in each Trie node to optimize the search.

### First Principles:

1. **Trie for Efficient Word Storage and Search**: Tries allow us to store words in a tree-like structure, where each path from the root to a node represents a word or a prefix. Tries enable efficient insertion and search operations.
  
2. **DFS (Depth First Search) for Path Exploration**: We use DFS to explore all possible paths starting from each cell of the board.

### Solving Intuition:

1. **Build a Trie**: Start by inserting all the words into the Trie.

2. **DFS for Word Search**: For each cell in the board, explore all paths that can be formed. At each cell, check the current path in the Trie.

3. **Reference Counting Optimization**: If a word is found in the Trie, it's removed to prevent rechecking. The reference count (`refs`) for each node indicates if any of its children are part of a valid path, reducing unnecessary exploration.

### Time Complexity:

- **Building the Trie**: O(N * M) where N is the number of words and M is the average length of the words.

- **DFS Exploration**: In the worst case, we can explore the entire board starting from each cell. So, O(ROWS * COLS * 4^L) where L is the maximum word length. The factor of 4 comes from the four possible directions of movement.

### Space Complexity:

- **Trie**: O(N * M) for the words in the Trie.
  
- **DFS Stack**: O(ROWS * COLS) due to the recursive nature of DFS.

### Raw Algorithm:

1. Insert all words into the Trie.
  
2. Start DFS from each cell in the board.

3. For each DFS call:
   - Check boundaries and if the cell is already visited.
   - Explore the path in the Trie.
   - If a word is found, add to results and remove from Trie.
   - Explore in all 4 directions.
   - Backtrack by marking the current cell as unvisited.

### Why The Code Is Written The Way It Is:

- **Dictionaries for Trie Children**: Using a dictionary allows dynamic expansion and efficient checking if a character exists as a child.

- **isWord and refs in TrieNode**: `isWord` indicates the end of a word. `refs` acts as a counter to check the usefulness of a Trie node, aiding in optimizing the search.

- **DFS and Backtracking**: DFS is used to explore all possible paths on the board. Backtracking (via `visit.remove((r, c))`) ensures that a cell is marked unvisited after it has been explored so that it can be used in other paths.

- **Set for Results**: A set is used to store unique words found on the board.

### Base Cases and Edge Cases:

- **Empty Board or Empty Words List**: The function should return an empty list.
  
- **Word Present Multiple Times**: The function should return the word only once.

- **DFS Boundaries**: The DFS should not explore cells outside the board.

### Optimizing Clues:

1. **Pruning using `refs`**: This is a significant optimization. If a Trie node's `refs` is less than 1, it means that no valid words can be formed using that path, and so, the exploration can be stopped early.

2. **Removing Found Words**: Once a word is found, it's removed from the Trie. This ensures that the same word isn't found multiple times and further optimizes the DFS.

### Mnemonics:

- **"Trie Before You Fly"**: Before launching into a DFS on the board, first build the Trie.
  
- **"DFS Deep, But Not Too Deep"**: Use DFS to explore the board deeply, but use the `refs` optimization to avoid unnecessary depths.

In essence, this solution leverages the efficiency of Tries for string operations and combines it with DFS for board exploration, using smart optimizations like reference counting to significantly reduce the search space.