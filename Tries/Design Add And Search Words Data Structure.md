---

---
---
[LC Link](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

---
---

```python
class TrieNode:
    def __init__(self):
        self.children = {}  # a : TrieNode
        self.word = False


class WordDictionary:
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word: str) -> None:
        cur = self.root
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
        cur.word = True

    def search(self, word: str) -> bool:
        def dfs(j, root):
            cur = root

            for i in range(j, len(word)):
                c = word[i]
                if c == ".":
                    for child in cur.children.values():
                        if dfs(i + 1, child):
                            return True
                    return False
                else:
                    if c not in cur.children:
                        return False
                    cur = cur.children[c]
            return cur.word

        return dfs(0, self.root)

```

This implementation pertains to a Trie (or Prefix Tree) with an added twist: the capability to search using wildcard characters (in this case, the "." character). The problem is a typical advanced Trie problem, which requires more than just the basic Trie operations.

### First Principles:

1. **Trie for String Storage**: Tries store strings where each path from the root to a node represents a string and nodes with common prefixes share the same path.
2. **Dictionary for Dynamic Allocation**: Unlike the 26 children (for each alphabet) that we saw in the previous Trie, here we have a dictionary that can be expanded dynamically. It's more space-efficient as memory is allocated only when needed.

### Solving Intuition:

1. **Adding Words is Simple**: Just like in a regular Trie, we loop through each character of the word and traverse down the Trie, creating nodes as necessary.
2. **Search with Wildcard is the Challenge**: When we encounter a ".", we can't simply go down one path; we must explore all children.

### Time Complexity:

- **addWord**: O(m) where m is the length of the word. Each letter requires constant time to process.
- **search**: In the worst case, we might need to explore all paths in our Trie, making it O(n * m) where n is the total number of nodes in the Trie and m is the length of the word.

### Space Complexity:

The space used by the Trie is O(n * m) where n is the number of words and m is the average length of the words.

### Raw Algorithm:

1. **addWord**: Start at the root. For every character in the word, check if it exists in the current node's children. If not, create a new TrieNode. Move down the Trie for the next character. Once the word ends, mark the current node as a word.
2. **search**: When a character is ".", search all children's paths recursively. Otherwise, continue down the Trie path for the current character. If we reach the end of the word and the current node is marked as a word, return True.

### Why The Code Is Written The Way It Is:

- **Dictionaries for Children**: Using a dictionary allows dynamic expansion and efficient checking if a character exists as a child. 
- **self.word in TrieNode**: This is a boolean flag to indicate if a word ends at this node.
- **dfs in search**: This recursive function is essential to handle the "." character. When "." is encountered, it tries all possible paths.
  
### Base Cases and Edge Cases:

- **Empty Trie**: If we call search on an empty Trie, it should return False.
- **Empty Word**: The current code would mark the TrieNode as a word. This might be unnecessary.
- **Only Wildcards**: If someone searches with only wildcards like "...", the search should try all possible combinations. 

### Optimizing Clues:

1. **Early Exit**: If at any point during the search, especially during a wildcard ".", if we find a matching word, we can exit early without exploring all paths.
2. **Compressing Paths**: In cases where there are no branches (a single path), nodes can be merged to represent entire strings instead of individual characters. This is called path compression and leads to a Patricia Trie or Radix Tree.

### Mnemonics:

- **"Trie = Tree for tries"**: When you attempt or "try" to find a word, Trie provides the tree structure to do so efficiently.
- **"." is a wildcard, wildcard = wild paths**: When you see ".", remember it can lead to multiple paths, making it wild in nature.

In summary, this Trie structure is an enhancement over the basic Trie with the capability to search using wildcards. The recursive DFS approach allows us to elegantly handle wildcard searches by exploring all possible paths.