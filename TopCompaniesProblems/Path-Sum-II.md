## Question
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.<br>

Note: A leaf is a node with no children.

**Example :**   
<pre>
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1

Return:

[
   [5,4,11,2],
   [5,8,4,5]
]
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: List[List[int]]
        """
        st = []
        ans = []
        self.dfs(root, sum, 0, st, ans)
        
        return ans
    
    def dfs(self, node, target, value, stack, ans):
        
        if not node:
            return None
        
        stack.append(node.val)
        value += node.val
        
        # Check if it is leaf
        if not node.left and not node.right and value == target:
            ans.append(stack+[])
        
        self.dfs(node.left, target, value, stack, ans)
        self.dfs(node.right, target, value, stack, ans)
        
        stack.pop() # When our path goes backward, we have to pop out the current node 
```

