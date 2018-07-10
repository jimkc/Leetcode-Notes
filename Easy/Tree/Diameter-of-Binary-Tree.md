## Question
Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

**Example :**
<pre>
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
</pre>

Note: The length of path between two nodes is represented by the number of edges between them.

## Thinking


## Coding
Time: O(n);  </br>
Space: O(n).
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def diameterOfBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        
        self.best = 0
        # maxdepth counts the current nodes max depth
        def maxdepth(root):
            if root == None:
                return -1 # The leaf returns 1, so we -1 back
            ld = maxdepth(root.left)
            rd = maxdepth(root.right)
            self.best = max(self.best, ld + rd + 2)  
            return 1 + max(rd,ld) 
        maxdepth(root)
        return self.best
```

