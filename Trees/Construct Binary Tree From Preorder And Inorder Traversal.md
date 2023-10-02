---

---
---
[LC Link](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

---
---

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None

        root = TreeNode(preorder[0])
        mid = inorder.index(preorder[0])
        root.left = self.buildTree(preorder[1 : mid + 1], inorder[:mid])
        root.right = self.buildTree(preorder[mid + 1 :], inorder[mid + 1 :])
        return root

```

Let's break down the given problem:

### Problem Statement:
Given two lists: `preorder` and `inorder` traversal of a tree, build and return the tree.

### First Principles:
1. In a `preorder` traversal, the first node is always the root of the tree (or subtree).
2. In an `inorder` traversal, the order is left-root-right. Therefore, the position of the root in `inorder` divides the list into nodes of the left and right subtrees.

### Solving Intuition:
- Using the `preorder` list, we can always identify the root of the (sub)tree.
- Using the position of this root in the `inorder` list, we can identify which nodes belong to the left subtree and which belong to the right subtree.
- This process can then be recursively applied to both left and right parts to build the complete tree.

### Time Complexity:
- **Time Complexity**: O(n^2) in the worst case. This is because for each node (n nodes in total), in the worst case, we might have to traverse through the entire `inorder` list to find its position (`O(n)` operation). In the average case, however, with a balanced tree, it would be closer to O(n log n).
- **Space Complexity**: O(n), primarily due to the recursive stack, which in the worst case can go as deep as n levels (in the case of a skewed tree).

### Raw Algorithm:
1. Pick the first node from the `preorder` list. This is our current root.
2. Find the position of this root in the `inorder` list. This will help divide the tree into left and right subtrees.
3. Recursively apply steps 1 and 2 to the left and right portions defined by the root's position in the `inorder` list.

### Programming Approach:
- **if not preorder or not inorder**: Base case. If either list is empty, return None.
- **root = TreeNode(preorder[0])**: The first node in `preorder` is the root.
- **mid = inorder.index(preorder[0])**: Find the root's position in the `inorder` list to split it into left and right subtrees.
- **root.left and root.right recursive calls**: Use the above-calculated position to make recursive calls for the left and right parts.

### Why The Code Is Written The Way It Is:
- **Why recursion?** - This problem is inherently recursive as trees are a recursive structure. Building a tree involves breaking down the problem into smaller sub-problems which involve building smaller trees.
- **Why lists slicing (`preorder[1:mid+1]` etc.)?** - These slices represent the respective `preorder` and `inorder` lists for the left and right subtrees. By dividing the list, we're essentially passing the relevant portion of the tree for the next recursive call.
  
### Base Cases and Edge Cases:
- **Base Case**: If either `preorder` or `inorder` is empty, we return None, because there's no tree to be made from empty lists.
- **Edge Cases**: The function assumes that both input lists are valid and consistent with each other. If there's a chance that they might not be, additional error checks would be needed.

### Optimizing Clues:
- The main bottleneck in this approach is finding the root's position in the `inorder` list during every recursive call. One can optimize this by pre-processing the `inorder` list into a dictionary to get O(1) look-up times, bringing the overall complexity down to O(n).

### Mnemonics:
**"PreRoot-InMid = TreeSplit"**:
1. **"PreRoot"** – `preorder`'s first element gives us the root.
2. **"InMid"** – The position of this root in `inorder` gives us the middle point.
3. **"TreeSplit"** – This middle point helps split the tree into left and right parts.

In conclusion, by understanding the nature of `preorder` and `inorder` traversals, one can effectively and efficiently rebuild the tree using these lists. The core lies in determining the root from `preorder` and then using its position in `inorder` to delineate between the left and right subtrees, applying this logic recursively.