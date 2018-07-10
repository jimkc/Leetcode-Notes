## Question
Given two binary trees, write a function to check if they are the same or not.<br>

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

**Example :**
<pre>
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true


Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false


Input:     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

Output: false
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
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """
         
        if p and q:
            return p.val == q.val and self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
        return p is q
```

