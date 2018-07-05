## Question
Find the sum of all left leaves in a given binary tree.

**Example :**
<pre>
 3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
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
    def sumOfLeftLeaves(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        self.cnt = 0
        def add(root):
            if root == None:
                return None
            
            if root.left != None and root.left.left == None and root.left.right == None:
                self.cnt += root.left.val
            add(root.left)
            add(root.right)
            
        add(root)
        return self.cnt
```

