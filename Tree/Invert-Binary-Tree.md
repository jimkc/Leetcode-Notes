## Question
Invert a binary tree.

**Example :**
<pre>
Input:

     4
   /   \
  2     7
 / \   / \
1   3 6   9
Output:

     4
   /   \
  7     2
 / \   / \
9   6 3   1
</pre>

## Thinking


## Coding
Time: O(n);  </br>
Space: O(logn).
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def invertTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        
        def invert(root):
            if root == None:
                return None
            
            tmp = root.right
            root.right = root.left
            root.left = tmp
            
            if root.right != None:
                invert(root.right)
            if root.left != None:
                invert(root.left)
            
        invert(root)
        return root
```
