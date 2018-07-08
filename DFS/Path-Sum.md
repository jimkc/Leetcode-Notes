## Question
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.<br>

**Example :**
<pre>
Given the below binary tree and sum = 22,

      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
</pre>

## Thinking


## Coding
Time: O(n); . </br>
Space: O(n).
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def hasPathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: bool
        """
        self.ans = False
        def addpath(node, v):
            if node == None:
                return None
            
            v += node.val
            if node.left == None and node.right == None:
                if v == sum:
                        self.ans = True

            addpath(node.left, v)
            addpath(node.right, v)
        
        if root != None:
            addpath(root,0)
        return self.ans
```

