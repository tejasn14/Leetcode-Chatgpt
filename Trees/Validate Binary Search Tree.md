---

---
---
[LC Link](https://leetcode.com/problems/validate-binary-search-tree/)

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
    def isValidBST(self, root: TreeNode) -> bool:
        def valid(node, left, right):
            if not node:
                return True
            if not (left < node.val < right):
                return False

            return valid(node.left, left, node.val) and valid(
                node.right, node.val, right
            )

        return valid(root, float("-inf"), float("inf"))

```

Certainly! Let's break down the `isValidBST` function step by step:

### Problem Statement:
Given the root of a binary tree, determine if it is a valid binary search tree (BST). A valid BST is defined as follows:
- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

### First Principles:
1. A binary search tree (BST) is a binary tree where each node has a numeric value. Additionally, the nodes in its left subtree only contain values less than or equal to the node’s value, and nodes in its right subtree only contain values greater than or equal to the node’s value.
2. A valid BST means that the above property holds for every node in the tree, not just the immediate children.

### Solving Intuition:
The key to solving this problem lies in recognizing the constraints applied to each node in the tree, given its position. Every node has a potential range of values it can take, and as we go deeper into the tree, this range narrows based on the ancestor nodes.

### Time Complexity:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is due to the recursive call stack's depth, which can be O(n) in the worst case of a skewed tree.

### Raw Algorithm:
1. Start by checking the root with an initial range of (-∞, ∞).
2. As you go down the tree, narrow the range of acceptable values for a node based on its ancestors.
3. If a node's value is outside the acceptable range, return False.

### Programming Approach:
- **valid function**: This is a recursive helper function to check if a node (and its descendants) lies within a given range.
- **left and right**: These are the current boundaries for the valid value of the `node`. They are updated as we traverse down the tree.

### Why Recursion:
- Recursion naturally fits this problem because the same validity check applies to every subtree in the binary tree. With recursion, we can break the problem down into smaller sub-problems (subtrees) and solve them.

### Why the Variables:
- **node**: Current node being evaluated.
- **left, right**: These represent the current valid range for the value of `node`.

### Why No Data Structures like Queues, Stacks, etc.:
- The problem is inherently recursive, and the natural call stack provides all we need for this depth-first traversal.

### Base Cases:
- **if not node**: If the node is None (reached a leaf node's child), it is considered valid (True).
  
### Edge Cases:
- **Node values at the extreme**: If your BST can contain the maximum or minimum possible integer values, using them as initial boundaries wouldn't work. Thus, we use `float("-inf")` and `float("inf")` to represent the initial boundaries, ensuring any real number will fall between them.

### Optimizing Clues:
The solution is pretty optimal, given that you have to visit every node in the tree to determine the BST validity. However, one could think of iterative methods using an explicit stack, but the complexity remains the same.

### Mnemonics to Remember:
**"Bounds on Nodes"**: Remember that as you move through the tree, there are bounds/constraints on node values based on their ancestors. Stick to these bounds to validate the BST.

In summary, this problem requires checking each node in the context of its potential valid range of values. We accomplish this by recursively diving deeper into the tree while adjusting the valid range based on the nodes we've traversed.