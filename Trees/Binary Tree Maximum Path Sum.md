---

---
---
[LC Link](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

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
    def maxPathSum(self, root: TreeNode) -> int:
        res = [root.val]

        # return max path sum without split
        def dfs(root):
            if not root:
                return 0

            leftMax = dfs(root.left)
            rightMax = dfs(root.right)
            leftMax = max(leftMax, 0)
            rightMax = max(rightMax, 0)

            # compute max path sum WITH split
            res[0] = max(res[0], root.val + leftMax + rightMax)
            return root.val + max(leftMax, rightMax)

        dfs(root)
        return res[0]

```

This problem is about finding the maximum path sum in a binary tree, where the path can be any path between two nodes, including only a single node. Let's break down the problem and solution in depth:

### First Principles:
1. Each node has a potential value it can contribute to the path sum.
2. A node can act as:
   a. A terminal node (either the starting or the ending node of the path).
   b. A passing node (a node which the path passes through but doesn't end at).

### Solving Intuition:
The core intuition here is understanding the difference between the **maximum contribution a node can make** to the path sum and the **maximum path sum which passes through the node**.

1. The maximum contribution a node can make will either include:
   a. Just its value.
   b. Its value plus the maximum of left or right subtree contributions.
2. The maximum path sum which passes through the node would include:
   a. Its value.
   b. Its value plus contributions from both the left and right subtrees.

### Time Complexity:
- **Time Complexity**: O(n), as each node in the tree is visited exactly once.
- **Space Complexity**: O(h), where h is the height of the tree, which is the maximum number of recursive stack frames used.

### Raw Algorithm:
1. For each node, compute its maximum contribution to the path sum.
2. For each node, compute the maximum path sum passing through it and update the global maximum.

### Programming Approach:
- **if not root**: Base case. If the current node is null, it can't contribute anything, so return 0.
- **leftMax and rightMax**: Recursive calls to get the maximum contributions of the left and right subtrees.
- **leftMax = max(leftMax, 0) and rightMax = max(rightMax, 0)**: A node shouldn't contribute negatively to the path sum. If the maximum contribution of a subtree is negative, consider it as 0.
- **res[0] = max(res[0], root.val + leftMax + rightMax)**: Update the result with the maximum path sum that passes through the current node.
- **return root.val + max(leftMax, rightMax)**: Return the maximum contribution of the current node.

### Why The Code Is Written The Way It Is:
- **Why recursion?** - As with many tree problems, recursion allows for a natural and clean exploration of all nodes.
- **Why use res as a list with one element instead of a simple variable?** - Python variables are passed by object reference, not by value. By using a list, the function is able to modify the list's content, effectively allowing us to store and update the global maximum.
- **Why max(leftMax, 0) and max(rightMax, 0)?** - To ensure that a node doesn't negatively contribute to the path sum. If a node has a negative value and both its children can't compensate for that, it's better to not include that path at all.

### Base Cases and Edge Cases:
- **Base Case**: If the node is null, return 0.
- **Edge Cases**: The function is resilient against negative values, as it checks if the contribution from the subtrees would be negative and resets it to zero if so.

### Optimizing Clues:
The given solution is optimal in terms of time complexity as each node is visited only once.

### Mnemonics:
**"MaxContribution ≠ MaxPassThrough"**:
1. **"MaxContribution"** – The maximum value a node can add to the path sum.
2. **"MaxPassThrough"** – The maximum sum of a path that passes through the node, possibly using both left and right subtrees.

In conclusion, the problem is solved by understanding the distinction between a node's maximum contribution to a path sum and the maximum path sum that passes through it. By leveraging the tree's recursive nature and using a global variable to keep track of the maximum path sum, the solution efficiently computes the desired result.