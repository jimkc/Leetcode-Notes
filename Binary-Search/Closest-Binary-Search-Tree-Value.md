## Question
Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.</br>

**Example :**
<pre>
Input: root = [4,2,5,1,3], target = 3.714286

    4
   / \
  2   5
 / \
1   3

Output: 4
</pre>

Note:

Given target value is a floating point.
You are guaranteed to have only one unique value in the BST that is closest to the target.


## Thinking

## Coding
Time: O(logn); Go through the binary tree. </br>
Space: O(1) 
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def closestValue(self, root, target):
        """
        :type root: TreeNode
        :type target: float
        :rtype: int
        """
        nearest = None
        dist = float("inf")
        while root:
            if abs(root.val - target) <= dist:
                nearest = root
                dist = abs(root.val - target)
            if target > root.val:
                root = root.right
            else: 
                root = root.left
                
        
        return nearest.val
```

