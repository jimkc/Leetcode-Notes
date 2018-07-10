## Question
Given a binary tree, return the tilt of the whole tree.<br>

The tilt of a tree node is defined as the absolute difference between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.<br>

The tilt of the whole tree is defined as the sum of all nodes' tilt.

**Example :**
<pre>
Input: 
         1
       /   \
      2     3
Output: 1
Explanation: 
Tilt of node 2 : 0
Tilt of node 3 : 0
Tilt of node 1 : |2-3| = 1
Tilt of binary tree : 0 + 0 + 1 = 1
</pre>

Note:

The sum of node values in any subtree won't exceed the range of 32-bit integer.
All the tilt values won't exceed the range of 32-bit integer.

## Thinking


## Coding
Time: O(n); . </br>
Space: O(n) For tree in worst case(skewed depth).
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def findTilt(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        
        self.tilt = 0
        
        def cntSum(root):
            
            if root == None:
                return 0
            
            leftSum, rightSum = 0, 0
            if root.left != None:
                leftSum +=  root.left.val + cntSum(root.left)
            if root.right != None:
                rightSum += root.right.val + cntSum(root.right)
            
            self.tilt += abs(leftSum-rightSum)
            return leftSum + rightSum
        cntSum(root)
        return self.tilt
```

