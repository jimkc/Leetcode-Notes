## Question
Given a binary search tree and the lowest and highest boundaries as L and R, trim the tree so that all its elements lies in [L, R] (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.

**Example :**
<pre>
Input: 
    1
   / \
  0   2

  L = 1
  R = 2

Output: 
    1
      \
       2
       
       
       
       
Input: 
    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

Output: 
      3
     / 
   2   
  /
 1
</pre>

## Thinking
Try to think from the beginning what you want to return, then think for the situation the 
current node would met, then the situation the left and right node would met.

## Coding
Time: O(n); Go through each node once. </br>
Space: O(n) Even though we don't explicitly use any additional memory, the call stack of our recursion could be as large as the number of nodes in the worst case..
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def trimBST(self, root, L, R):
        """
        :type root: TreeNode
        :type L: int
        :type R: int
        :rtype: TreeNode
        """
        # Think step2
        if root == None:
            return None
        
        # Think step3
        if root.val < L:
            return self.trimBST(root.right, L, R)
        elif root.val > R:
            return self.trimBST(root.left, L, R)
        
        # Think step4
        if root.left != None:
            root.left = self.trimBST(root.left, L, R)
        if root.right != None:
            root.right = self.trimBST(root.right, L, R)
        
        # Think step1
        return root 
```

