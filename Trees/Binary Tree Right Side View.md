---

---
---
[LC Link](https://leetcode.com/problems/binary-tree-right-side-view/)

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
    def rightSideView(self, root: TreeNode) -> List[int]:
        res = []
        q = collections.deque([root])

        while q:
            rightSide = None
            qLen = len(q)

            for i in range(qLen):
                node = q.popleft()
                if node:
                    rightSide = node
                    q.append(node.left)
                    q.append(node.right)
            if rightSide:
                res.append(rightSide.val)
        return res

```

Sure! Let's break down the problem and understand the solution in a structured manner.

### Problem Statement:
Given a binary tree, the goal is to visualize it from the right side and return the nodes that can be seen. 

### First Principles:
A binary tree is a data structure in which each node has at most two children: left and right. By looking from the right side, you'll see the rightmost node on each level.

### Solving Intuition:
The best way to address the problem is by considering a level-order traversal (BFS) because the requirement focuses on levels. When traversing each level, the last node encountered is the one visible from the right side.

### Time Complexity:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Every node is processed once.
- **Space Complexity**: O(n), mainly because of the queue. In the worst case, it might hold all nodes at the deepest level, which could be n/2 nodes.

### Raw Algorithm:
1. Use a queue to facilitate BFS.
2. For each level in the tree:
   1. Track the rightmost node.
   2. Enqueue child nodes (left first, then right).
   3. At the end of the level, the rightmost node is the one visible from the right side.

### Programming Approach:
- **res**: This list stores the nodes that can be seen from the right.
- **q**: A queue that helps in BFS traversal.
- **rightSide**: At every level, it tracks the last (rightmost) node encountered.
- **while q**: Ensures that nodes are processed as long as there are nodes in the queue.
- **for i in range(qLen)**: Iterates through all nodes in the current level.
- **if rightSide**: Checks if a rightmost node was found at the current level.

### Why the While Loop:
The while loop ensures continuous processing of nodes as long as there are nodes left to process in the queue.

### Why the Variables:
- **res**: To store nodes that can be seen from the right.
- **q**: To facilitate BFS traversal.
- **rightSide**: To track the rightmost node at each level.

### Why the Data Structure:
- **Deque (from collections)**: Perfect for BFS as it allows O(1) operations for append and pop from both ends. Lists aren't efficient for popping elements from the front, making deque a better choice.

### Base Cases:
- **q = collections.deque([root])**: Starts with the root. If the tree is empty, nothing happens.
- **if rightSide**: Checks to ensure only valid, encountered nodes are appended to the result.

### Edge Cases:
- **Unbalanced Trees**: The algorithm still works correctly. The nodes that can be seen from the right in skewed trees are just the ones on the deepest path.

### Optimizing Clues:
While the provided solution is already optimal for this problem, if one were to consider some sort of 'view' problems related to trees in the future, they might think of caching results or considering dynamic programming if there are overlapping subproblems. However, for this problem, BFS remains the most straightforward and efficient solution.

### Mnemonics to Remember:
**"Rightmost at Each Level"**: Remember that you're looking for the rightmost node at every level of the tree during BFS traversal.

In summary, the problem is essentially a twist on the classic level-order traversal where instead of collecting all nodes at each level, you're interested in only the rightmost node. The deque is used to efficiently traverse the tree level by level, and the rightmost node at each level is appended to the result list `res`.