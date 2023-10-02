---

---
---
[LC Link](https://leetcode.com/problems/invert-binary-tree/)

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
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        
        # swap the children
        root.left, root.right = root.right, root.left
        
        # make 2 recursive calls
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root

```

This problem is about inverting a binary tree. That is, flipping it upside down and reversing left and right children at every node.

### First Principles:
The fundamental property of binary trees that we're using here is its recursive structure. A tree consists of a root and two sub-trees - the left and the right sub-trees. Any operation you wish to do on the tree (like inverting it) can be done by performing the operation on the root and then recursively doing it on its sub-trees.

### Solving Intuition:
1. Visualize inverting a small tree. You'll quickly notice that the root remains the same, but its left and right children get swapped.
2. Extend this observation: once you swap the children of the root, you have to invert the left and right sub-trees as well. This is a classic divide-and-conquer approach.

### Time/Space Complexity:
- **Time complexity**: O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space complexity**: O(h), where h is the height of the tree. This is the space required by the call stack due to recursion. In the worst case (tree is completely unbalanced), it could be O(n).

### Raw Algorithm:
1. If the current node is null, return null.
2. Swap the left and right children of the current node.
3. Recursively invert the left sub-tree.
4. Recursively invert the right sub-tree.
5. Return the current node.

### Programming Approach:
1. **Base Case**: If the node (`root` in this case) is null, we return null. This handles the base case for our recursive approach and also takes care of the edge case where the tree is empty.
2. **Swapping Children**: The key operation is to swap the left and right children, and this is done using a simple tuple assignment in Python: `root.left, root.right = root.right, root.left`.
3. **Recursive Calls**: We then make recursive calls to invert the left and right sub-trees.
4. **Return Value**: Finally, we return the `root`, which now has its immediate children swapped and its sub-trees inverted.

### Variables and Data Structures:
- **TreeNode**: A basic structure representing a node in a binary tree.
- **root**: The current node that we're working with.
- **root.left and root.right**: The left and right children of the node.

### Base & Edge Cases: 
- The base case is when the root is null, indicating we've reached a leaf node or the tree is empty.
- An edge case is when the tree is empty, which the base case also handles.

### Optimizing Clues:
The given solution is already very efficient in terms of time and space complexity. Any further micro-optimizations would be language-specific or hardware-specific and wouldn't have a significant impact on the general efficiency of the solution.

### Mnemonics to Remember:
**"Inverting Trees**:
1. **Swap now**.
2. **Dive deep left and right**.

This simple structure helps recall the process of tree inversion: start by swapping the immediate children and then recursively delve deeper into both sub-trees.