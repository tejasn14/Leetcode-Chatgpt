---

---
---
[LC Link](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

---
---

```python
# RECURSIVE DFS
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0

        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))


# ITERATIVE DFS
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        stack = [[root, 1]]
        res = 0

        while stack:
            node, depth = stack.pop()

            if node:
                res = max(res, depth)
                stack.append([node.left, depth + 1])
                stack.append([node.right, depth + 1])
        return res


# BFS
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        q = deque()
        if root:
            q.append(root)

        level = 0

        while q:

            for i in range(len(q)):
                node = q.popleft()
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            level += 1
        return level

```

All three methods provided solve the same problem - finding the maximum depth of a binary tree. Let's break them down one by one.

### RECURSIVE DFS (Depth-First Search):

#### First Principles:
A binary tree's maximum depth can be recursively defined in terms of its sub-trees. Specifically, the depth is the maximum of the depths of the left and right sub-trees, plus 1.

#### Solving Intuition:
The key insight is that you don't need to traverse the entire tree before calculating the depth. You calculate the depth as you go, based on the depths of the child sub-trees.

#### Time/Space Complexity:
- **Time Complexity**: O(n), each node is visited once.
- **Space Complexity**: O(h), where h is the height of the tree. This is due to the recursive call stack. In worst case (skewed tree), it's O(n).

#### Programming Approach:
- **Base Case**: If the root is null, the depth is 0.
- **Recursive Call**: Otherwise, the depth is 1 (for the current node) plus the maximum of the depths of the left and right sub-trees.

### ITERATIVE DFS:

#### First Principles:
Instead of using a recursive approach to traverse the tree, we can use a stack to iteratively traverse the tree in a depth-first manner.

#### Solving Intuition:
Using a stack, we start with the root. At each step, we pop a node from the stack and push its children. This way, we delve deeper into the tree before backtracking, similar to the recursive approach.

#### Time/Space Complexity:
- **Time Complexity**: O(n)
- **Space Complexity**: O(n), in the worst case, when the tree is skewed.

#### Programming Approach:
- **Stack**: Holds nodes and their corresponding depths.
- **While Loop**: Iterates as long as there are nodes left to process.
- **Node Processing**: The node's depth is compared to the current maximum depth (`res`).

### BFS (Breadth-First Search):

#### First Principles:
Instead of delving deep into one branch of the tree before backtracking (like DFS), BFS processes all nodes at the current depth before moving on to nodes at the next depth level.

#### Solving Intuition:
Using a queue, we can process nodes in the order they are encountered. For each node, we queue up its children. Thus, nodes are processed level by level.

#### Time/Space Complexity:
- **Time Complexity**: O(n)
- **Space Complexity**: O(w), where w is the maximum width of the tree. This is the most number of nodes the queue will hold at any point.

#### Programming Approach:
- **Queue**: Holds nodes to be processed.
- **While Loop**: Processes nodes level by level.
- **Inner For Loop**: Processes all nodes at the current depth.
- **Level Counter**: Increases as we move on to the next depth level.

### Why the Data Structures:
- **Recursive DFS**: Uses the call stack implicitly.
- **Iterative DFS**: Uses a stack to mimic the call stack in the recursive version.
- **BFS**: Uses a queue to process nodes level by level.

### Base Cases:
- For all methods: if the tree is empty (`root` is null), the depth is 0.

### Edge Cases:
- A tree with only one node (the depth is 1).
- A skewed tree (essentially a linked list) where the depth is equal to the number of nodes.

### Optimizing Clues:
The given methods are already optimized for the problem at hand. Further optimization would involve micro-optimizations specific to certain languages or platforms.

### Mnemonics to Remember:
"**DFS deep, BFS broad**" – DFS dives deep into the tree's branches, while BFS scans the tree level by level.