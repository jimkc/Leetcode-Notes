## Question
Given a binary tree, find its maximum depth.<br>

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Note: A leaf is a node with no children.

**Example :**
<pre>
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7

return its depth = 3.
</pre>


## Thinking


## Coding
Time: O(n); Go through each node once. </br>
Space: O(logn) The stack of the recursive is at most the height.
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        
        self.max_depth = 0 # Remember to use self
        def cntDepth(root, curdepth):
            
            if root == None:
                return None
            
            if curdepth > self.max_depth:
                self.max_depth = curdepth
                
            if root.left != None:
                cntDepth(root.left, curdepth+1)
            if root.right != None:
                cntDepth(root.right, curdepth+1)
        
        cntDepth(root, 1)
        return self.max_depth
```

