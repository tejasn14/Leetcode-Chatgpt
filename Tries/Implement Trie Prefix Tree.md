---

---
---
[LC Link](https://leetcode.com/problems/implement-trie-prefix-tree/)

---
---

```python
class TrieNode:
    def __init__(self):
        self.children = [None] * 26
        self.end = False


class Trie:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        curr = self.root
        for c in word:
            i = ord(c) - ord("a")
            if curr.children[i] == None:
                curr.children[i] = TrieNode()
            curr = curr.children[i]
        curr.end = True

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        curr = self.root
        for c in word:
            i = ord(c) - ord("a")
            if curr.children[i] == None:
                return False
            curr = curr.children[i]
        return curr.end

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        curr = self.root
        for c in prefix:
            i = ord(c) - ord("a")
            if curr.children[i] == None:
                return False
            curr = curr.children[i]
        return True

```

This problem pertains to implementing a **Trie** (also known as a Prefix Tree). A Trie is a specialized tree-like data structure used primarily for storing a dynamic set of strings, where the keys are usually strings. It's vastly efficient in terms of lookup operations, particularly when it comes to prefixes.

### First Principles:

1. **Trie is a Tree**: While it is a specialized tree, it retains tree-like characteristics, including nodes and edges.
2. **Path from Root Represents Strings**: Each path from the root to a node represents a string.
3. **Shared Prefixes Share Nodes**: This is the primary reason tries are efficient for prefix operations.

### Solving Intuition:

When you think of storing strings and searching for prefixes, an array or a list might not be efficient. For instance, to check if a word exists in a list, you might need to scan the entire list. But with a trie, common prefixes of words share the same path. Thus, operations like `search` and `startsWith` can be done in \(O(m)\), where \(m\) is the length of the word or prefix, irrespective of how many words the trie holds.

### Time Complexity:

- **Insert**: \(O(m)\) where \(m\) is the length of the word. Each letter requires constant time to process.
- **Search**: \(O(m)\) where \(m\) is the length of the word.
- **StartsWith**: \(O(m)\) where \(m\) is the length of the prefix.

### Space Complexity:

The space used by a trie is \(O(n \times m)\) where \(n\) is the number of words and \(m\) is the average length of the words. In the worst case, when there are no common prefixes, the trie has as many nodes as there are characters in all words.

### Raw Algorithm:

1. Each `TrieNode` has 26 children, one for each letter of the alphabet.
2. To **insert** a word, you start at the root and traverse the children corresponding to the word's letters. If the child doesn't exist, you create it. When the word is done, you mark the last node as an "end" node.
3. To **search** for a word, you traverse the trie in the same manner. If you can't find a node for one of the word's letters or if the last node isn't an "end" node, the word isn't in the trie.
4. To see if a word **startsWith** a prefix, the process is similar to search but you don't need to check for the "end" node.

### Why The Code Is Written The Way It Is:

- **TrieNode with 26 children**: Since we're only dealing with lowercase English letters, we have 26 children for each node.
- **self.end in TrieNode**: This is used to determine if a word ends at this node.
- **ord(c) - ord('a')**: A nifty trick to convert the character to its index. 'a' becomes 0, 'b' becomes 1, and so on.
- **Insert**: For each character in the word, find the index and check if a node exists. If not, create a node. Then proceed to the next character.
- **Search**: Traverse the TrieNode children based on the word's characters. If a node doesn't exist, return False. Finally, check if the word ends at the final node.
- **startsWith**: This is almost the same as search, but you don't need to check the "end" of the word.

### Base Cases and Edge Cases:

- **Empty Trie**: If you call search or startsWith on an empty trie, it should return False.
- **Empty Word**: If someone tries to insert an empty word, it should ideally be handled, although the current code doesn't check for this explicitly.

### Optimizing Clues:

1. **Dynamic Children Size**: Instead of always allocating memory for 26 children, we can use a dictionary/hashmap. This way, we only allocate memory when we insert a node.
2. **Delete Operation**: The current trie doesn't support delete. Implementing delete can help free up space.

### Mnemonics:

- **"Trie is Try with an 'e'!"**: Just like you "try" to find a word, in Trie, you traverse through the nodes to "try" and find it.
- **"End means Word End"**: The 'end' attribute in TrieNode indicates the end of a word.
  
In summary, the Trie data structure is highly efficient for prefix-based search operations. Its design makes operations like search, insertion, and prefix checking very quick. The recursive nature of the tree structure, combined with the predictable nature of the English alphabet, allows for elegant and intuitive solutions to string-based problems.