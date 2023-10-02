---

---
---
[LC Link](https://leetcode.com/problems/subtree-of-another-tree/)

---
---

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, s: TreeNode, t: TreeNode) -> bool:
        if not t:
            return True
        if not s:
            return False

        if self.sameTree(s, t):
            return True
        return self.isSubtree(s.left, t) or self.isSubtree(s.right, t)

    def sameTree(self, s, t):
        if not s and not t:
            return True
        if s and t and s.val == t.val:
            return self.sameTree(s.left, t.left) and self.sameTree(s.right, t.right)
        return False

```

Certainly! Let's dissect the problem of determining if one binary tree is a subtree of another.

### Problem Statement:
Given two non-empty binary trees `s` and `t`, the task is to check whether tree `t` has an identical subtree present in tree `s`.

### First Principles:
1. A subtree of `s` is identical to `t` if the roots are the same and all their descendants are the same.
2. A subtree can start at any node of `s`, not just the root. Therefore, we must check every node in `s` to see if it is the start of an identical subtree to `t`.

### Solving Intuition:
To determine if `t` is a subtree of `s`, you'd start by finding a node in `s` that has the same value as the root of `t`. Once found, you can then compare this potential subtree of `s` with `t` to check if they are identical. If they are, then `t` is indeed a subtree of `s`.

### Time Complexity:
- **Time Complexity**: O(m*n), where m is the number of nodes in tree `s` and n is the number of nodes in tree `t`. This is because, in the worst case, we compare every node in `s` with every node in `t`.
- **Space Complexity**: O(m) where m is the height of the tree `s`. This is the space needed for the recursion stack in the worst case.

### Raw Algorithm:
1. If tree `t` is empty, return True because an empty tree is a subtree of any tree.
2. If tree `s` is empty but `t` is not, return False.
3. Compare the current node in `s` with the root of `t` using the `sameTree` function.
4. If they are the same, return True; otherwise, check the left and right subtrees of the current node in `s`.

### Programming Approach:
- **if not t**: Base case to handle an empty subtree. An empty subtree is always a subtree of any tree.
- **if not s**: Base case to handle if the main tree is empty but the subtree isn't.
- **self.sameTree(s, t)**: This checks if tree `t` is identical to the tree rooted at the current node in `s`.
- **self.isSubtree(s.left, t) or self.isSubtree(s.right, t)**: If not, then check the left and right children of `s`.

### Why the Data Structures:
- **Recursive function (`isSubtree` and `sameTree`)**: Recursion naturally fits this problem. It allows for an elegant depth-first traversal of the tree.

### Base Cases:
- `t` is empty.
- `s` is empty but `t` is not.

### Edge Cases:
- Both trees are identical.
- `t` is not present in `s` at all.
- `t` is present multiple times in `s`.

### Optimizing Clues:
One optimization could involve pre-order traversals to convert both trees into strings. If the string of `t` is a substring of the string of `s`, then `t` is a subtree of `s`. This approach would require careful encoding to avoid false positives (e.g., tree 12##3 and tree 1##23 will have the same string in a naive implementation).

### Mnemonics to Remember:
**"Check the Root, then Left and Right to Boot."**  
The essence of this solution is to compare the current node in `s` with the root of `t`. If they aren't identical, we keep checking with the left and right children of `s`.

In summary, the solution employs a depth-first approach using recursion. The main function (`isSubtree`) checks every node in `s`, and for each node, it uses the helper function (`sameTree`) to see if the subtree rooted at that node is identical to `t`.