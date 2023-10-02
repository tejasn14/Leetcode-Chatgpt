---

---
---
[LC Link](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

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
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        stack = []
        curr = root

        while stack or curr:
            while curr:
                stack.append(curr)
                curr = curr.left
            curr = stack.pop()
            k -= 1
            if k == 0:
                return curr.val
            curr = curr.right

```

Alright! Let's dissect the `kthSmallest` function.

### Problem Statement:
Given a root of a binary search tree (BST) and an integer `k`, find the kth smallest element in the BST.

### First Principles:
1. In a BST, the inorder traversal of its nodes will yield nodes in ascending order.
2. If we perform an inorder traversal, the kth node we visit will be the kth smallest element.

### Solving Intuition:
To find the kth smallest, we can do an inorder traversal and stop once we've visited k nodes. Since an inorder traversal of a BST gives values in increasing order, our stopping point will be the kth smallest value.

### Time Complexity:
- **Time Complexity**: O(H + k), where H is the tree's height. In the worst-case scenario, we might need to visit all nodes, so the complexity becomes O(N), where N is the number of nodes.
- **Space Complexity**: O(H). This is due to the storage required for the recursion stack (implicit stack in our iterative approach).

### Raw Algorithm:
1. Start with the root node.
2. Traverse to the leftmost node (smallest value), adding nodes to a stack as you go.
3. Once there are no more left children, pop a node from the stack and decrement k.
4. If k == 0, return the node's value.
5. Move to the right child and repeat the process.

### Programming Approach:

- **stack**: This is an explicit stack used to facilitate our iterative inorder traversal. 
- **curr**: This is our current node we are inspecting.

### Why Iterative Approach (while loop & stack):
While a recursive approach is more intuitive for tree traversal, the iterative approach is often more efficient in practice, avoiding the overhead of recursive function calls. The stack allows us to backtrack to the previous node once we reach a leaf, mimicking the natural call stack in recursive methods.

### Why the Variables:
- **curr**: Helps us track our current position in the tree.
- **stack**: Acts as our memory, reminding us where we've been, so we can backtrack and continue our traversal.

### Why No Other Data Structures:
- The nature of the problem requires only a stack. Since we are doing an inorder traversal, there's no need for other data structures like heaps, queues, dictionaries, etc.

### Base Cases:
- None explicitly. The while loop will naturally terminate once all nodes are processed or when we've found our kth node.

### Edge Cases:
- **Empty tree**: The function doesn't handle this, but in practice, the assumption might be a valid tree is provided. If unsure, one can always add a simple check at the start.
- **k larger than the number of nodes**: Again, the function assumes valid input. In reality, always verify assumptions with such problems.

### Optimizing Clues:
The given solution is already quite optimal. However, once the kth smallest is found, the function stops and returns, ensuring we don't process more nodes than necessary.

### Mnemonics to Remember:
**"Stack the Left, Pop and Right"**: 
1. "Stack the Left" – push left children to the stack.
2. "Pop" – pop from the stack and process the node.
3. "and Right" – move to the right child and repeat.

In conclusion, the problem is essentially an inorder traversal with a stopping condition. Using an iterative approach with a stack allows us to process the nodes in order, and by tracking our progress with the variable k, we can identify and return the kth smallest node.