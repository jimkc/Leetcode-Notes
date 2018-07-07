## Question
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

**Example :**
<pre>
For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

    1
   / \
  2   2
 / \ / \
3  4 4  3
But the following [1,2,2,null,3,null,3] is not:
    1
   / \
  2   2
   \   \
   3    3
   
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
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        
        def walk(root, direction):
            if root == None:
                return '0'
            
            path = str(root.val)
            if direction == 'left':
                path += walk(root.left,'left')
                path += walk(root.right,'left')
            else:
                path += walk(root.right,'right')
                path += walk(root.left,'right')
                
            return path
        
        if root != None:
            return walk(root.left,'left') == walk(root.right,'right')
        else:
            return True
```

