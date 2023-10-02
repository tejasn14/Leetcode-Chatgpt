---

---
---
[LC Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

---
---

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution:
    def lowestCommonAncestor(
        self, root: "TreeNode", p: "TreeNode", q: "TreeNode"
    ) -> "TreeNode":
        while True:
            if root.val < p.val and root.val < q.val:
                root = root.right
            elif root.val > p.val and root.val > q.val:
                root = root.left
            else:
                return root

```

Certainly! Let's dive into this problem step by step.

### Problem Statement:
Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST. 

### First Principles:
The defining characteristic of a BST is that for every node `N`, all nodes in `N's` left subtree have values less than `N`, and all nodes in `N's` right subtree have values greater than `N`.

### Solving Intuition:
Given the BST property, if both `p` and `q` are greater than a node `root`, then the LCA must be on the right side of `root`. If both `p` and `q` are smaller than `root`, then the LCA must be on the left. If `p` and `q` lie on opposite sides of `root` (or one of them is the `root`), then `root` must be their LCA.

### Time Complexity:
- **Time Complexity**: O(log n) in the best/average case, and O(n) in the worst case (if the BST is skewed).
- **Space Complexity**: O(1), since we are not using any additional data structures, only a few pointers.

### Raw Algorithm:
1. Start at the `root` of the BST.
2. If both `p` and `q` are to the right of `root`, move to the right child of `root`.
3. If both `p` and `q` are to the left of `root`, move to the left child of `root`.
4. If `p` and `q` are on opposite sides of `root` or one of them is the `root`, then `root` is their LCA.

### Programming Approach:
- **while True**: The loop keeps running until the LCA is found.
- **if root.val < p.val and root.val < q.val**: Both nodes are on the right, so we move to the right child.
- **elif root.val > p.val and root.val > q.val**: Both nodes are on the left, so we move to the left child.
- **else**: This means `p` and `q` are on different sides of `root` or one of them is the `root`.

### Why the While Loop:
The loop is used to traverse the BST based on the values of `p` and `q` compared to the current `root` until the LCA is found.

### Why the Variables:
- **root**: Represents the current node being inspected in the BST.
- **p** and **q**: The two nodes for which we are finding the LCA.

### Why the Data Structure:
This problem uses the characteristics of a BST. The properties of a BST are employed to guide the traversal towards the LCA of the nodes, eliminating half of the possibilities in each comparison. 

### Base Cases:
- Both `p` and `q` are to the right of the current node.
- Both `p` and `q` are to the left of the current node.
- `p` and `q` are on opposite sides of the current node, or one of them is the current node.

### Edge Cases:
- If `p` or `q` is the root.
- If `p` is an ancestor of `q` or vice-versa.

### Optimizing Clues:
This algorithm is already efficient due to the properties of the BST. In each iteration, you're essentially eliminating half of the remaining tree from consideration.

### Mnemonics to Remember:
**"Left, Right, or In Sight"** 
- "Left" if both nodes are to the left, 
- "Right" if both are to the right, 
- "In Sight" when the current node is in between, making it the LCA. 

In summary, the solution leverages the inherent properties of a BST. By comparing the values of `p` and `q` with the current node, it determines whether to move left, move right, or return the current node as the LCA.