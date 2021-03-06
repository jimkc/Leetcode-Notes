## Question
Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.
**Example :**
<pre>
Input:</br> 
Tree 1                     Tree 2                
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
Output: 
Merged tree:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
</pre>
Note: The merging process must start from the root nodes of both trees.

## Thinking
Recursively adding tree nodes in preorder traversal. Tree nodes are allowed to be NULL, so it is suitable to use or condition while t1,t2 are both null.
## Coding
Time: O(m); A total of mm nodes need to be traversed. Here, mm represents the minimum number of nodes from the two given trees. </br>
Space: O(m)
```python3
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def mergeTrees(self, t1, t2):
        """
        :type t1: TreeNode
        :type t2: TreeNode
        :rtype: TreeNode
        """
        
        if t1 and t2:
            root = TreeNode(t1.val + t2.val)
            root.left = self.mergeTrees(t1.left, t2.left)
            root.right = self.mergeTrees(t1.right, t2.right)
            return root
        else: 
            return t1 or t2
        
            
        
```

