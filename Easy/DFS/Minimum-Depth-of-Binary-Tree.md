## Question
Given a binary tree, find its minimum depth.<br>

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Example :**
<pre>
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
   
return its minimum depth = 2.
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
    def minDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        self.minpath = float('inf')
        
        def walk(node, d):
            
            if node == None:
                return None
            d += 1
            
            if node.left == None and node.right == None:
                if d < self.minpath:
                    self.minpath = d
            walk(node.left,d)
            walk(node.right,d)
        
        if root != None:
            walk(root,0)
            return self.minpath
        else:
            return 0
```

