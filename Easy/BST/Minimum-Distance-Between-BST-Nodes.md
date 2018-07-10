## Question
Given a Binary Search Tree (BST) with the root node root, return the minimum difference between the values of any two different nodes in the tree.

**Example :**   
<pre>
Input: root = [4,2,6,1,3,null,null]
Output: 1
Explanation:
Note that root is a TreeNode object, not an array.

The given tree [4,2,6,1,3,null,null] is represented by the following diagram:

          4
        /   \
      2      6
     / \    
    1   3  

while the minimum difference in this tree is 1, it occurs between node 1 and node 2, also between node 3 and node 2.
<pre>

Note:<br>

The size of the BST will be between 2 and 100.<br>
The BST is always valid, each node's value is an integer, and each node's value is different.<br>

## Thinking
Use in order traversal in BST instead of sorting all nodes value.

## Coding
Time: O(n) ;<br>
Space: O(n)
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def minDiffInBST(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        
        def dfs(node):
            if node:
                dfs(node.left)
                self.ans = min(self.ans, node.val - self.prev) # prev will always be smaller than node val
                self.prev = node.val
                dfs(node.right)

        self.prev = float('-inf')
        self.ans = float('inf')
        dfs(root)
        return self.ans
        
```

