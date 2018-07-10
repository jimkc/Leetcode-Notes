## Question
Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.<br>

**Example :**
<pre>
Input: The root of a Binary Search Tree like this:
              5
            /   \
           2     13

Output: The root of a Greater Tree like this:
             18
            /   \
          20     13
</pre>

## Thinking
We can alter the sum value after we finish the right side, the order to call recursive is important.

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
    def convertBST(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        
        def convert(node, sumv):
            
            if node != None:
                sumv = convert(node.right, sumv)
                sumv += node.val # Sumv is added with the current node value, so the left child will add all greater node value
                node.val = sumv # Update the node value
                sumv = convert(node.left, sumv)

            return sumv 
        
        convert(root,0)
        
        return root
```
