---

---
---
[LC Link](https://leetcode.com/problems/balanced-binary-tree/)

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
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        def dfs(root):
            if not root:
                return [True, 0]

            left, right = dfs(root.left), dfs(root.right)
            balanced = left[0] and right[0] and abs(left[1] - right[1]) <= 1
            return [balanced, 1 + max(left[1], right[1])]

        return dfs(root)[0]

```

Alright, let's take a deep dive into this problem of determining if a binary tree is height-balanced.

### The Problem:
A height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differs by more than 1.

### First Principles:
1. The depth of a tree can be found using a bottom-up approach, i.e., first, get the depths of the child nodes and then decide the depth of the current node based on the child nodes.
2. The balance factor for any node in the tree is the absolute difference between the height of its left subtree and that of its right subtree.

### Solving Intuition:
To determine if a tree is balanced, we can combine two pieces of information:
- The tree's height.
- The balance factor for the tree. 

If at any node, the balance factor exceeds 1, then the tree is unbalanced. If all nodes adhere to this condition, then the tree is balanced.

### Time Complexity:
- **Time Complexity**: O(n), where n is the number of nodes in the binary tree. Every node is visited only once.
- **Space Complexity**: O(h), where h is the height of the binary tree. This is due to the recursive call stack. In the worst case, h = n, making the space complexity O(n).

### Raw Algorithm:
1. Starting from the leaves (base case), determine the height of each subtree.
2. At each node, check the balance factor. If it's greater than 1, the tree isn't balanced.
3. Return the tree's height to the parent node.

### Programming Approach:
- **dfs function**: This recursive function returns two things: whether the subtree rooted at the given node is balanced and its height. 
- **if not root**: Base case of the recursion. If the node is null, it's considered balanced, and its height is 0.
- **left and right**: Recursive calls to the left and right children of the current node.
- **balanced**: This variable checks three things:
  1. If the left subtree is balanced.
  2. If the right subtree is balanced.
  3. If the balance factor of the current node is less than or equal to 1.
- **return**: The function returns both the balance status and the height of the tree rooted at the current node.

### Why the Data Structures:
- **Recursive function (dfs)**: We use the system's call stack to traverse the binary tree. At each node, we might need to wait for results from its children before we can compute our result.
- **List for return values**: We use a list of two values, as a method can return only one value. Here, we've combined both the balance status and height into one list.

### Base Cases:
- If the tree (or subtree) is empty (node is `None`), it's considered balanced, and its height is 0.

### Edge Cases:
- A tree with only one node: it's balanced.
- A skewed tree (essentially a linked list): it's unbalanced if it has more than 2 nodes.

### Optimizing Clues:
The current approach is already efficient. One optimization could be to stop the dfs traversal as soon as an unbalanced subtree is found, but this method essentially accomplishes that due to the recursive nature.

### Mnemonics to Remember:
**"Balance with Depth."**  
At every step, while you're measuring the depth of the tree, you're also checking its balance. This way, depth helps in determining the balance of the binary tree.

In essence, this method synergistically combines the procedures of determining the height of a tree and checking its balance, making the solution both efficient and elegant.