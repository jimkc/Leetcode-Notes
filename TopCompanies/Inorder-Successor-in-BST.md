## Question
Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

Note: If the given node has no in-order successor in the tree, return null.

**Example :**   
<pre>
Input: root = [2,1,3], p = 1

  2
 / \
1   3

Output: 2


Input: root = [5,3,6,2,4,null,null,1], p = 6

      5
     / \
    3   6
   / \
  2   4
 /   
1

Output: null
</pre>

## Thinking
First method is classic inorder traversal.

## Coding
Time: O(n); 
Space: O(n)
```python3
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def inorderSuccessor(self, root, p):
        """
        :type root: TreeNode
        :type p: TreeNode
        :rtype: TreeNode
        """
        order = []
        self.inorderTraversal(root, order)
        for i in range(len(order)-1):
            if order[i] == p.val:
                return order[i+1]
        
    def inorderTraversal(self, node, order):
    
        if not node:
            return
        
        if node.left:
            self.inorderTraversal(node.left, order)
        
        order.append(node.val)
        
        if node.right:
            self.inorderTraversal(node.right, order)
```

Faster, to stop while met the value, beacause its a BST.
```python3
class Solution(object):
    def inorderSuccessor(self, root, p):
        succ = None
        while root:
            if p.val < root.val:
                succ = root
                root = root.left
            else:
                root = root.right
        return succ
```
