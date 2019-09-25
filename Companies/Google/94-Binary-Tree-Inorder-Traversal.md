## Question
Given a binary tree, return the inorder traversal of its nodes' values.<br>

**Example :**   
<pre>
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
</pre>

Follow up: Recursive solution is trivial, could you do it iteratively?<br>

## Thinking

## Coding
**Method 1: Recursion**<br>
Time: O(n); </br>
Space: O(n+logn)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        
        def helper(node, inorder):
            if node == None:
                return
            
            if node.left:
                helper(node.left, inorder)
            
            inorder.append(node.val)
            
            if node.right:
                helper(node.right, inorder)
        
        inorder = []        
        helper(root, inorder)
        return inorder
```

**Method 2: Iterative**<br>
Time: O(n); </br>
Space: O(n)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        
        ans, stack = [], []
        
        while True:
            while root:
                stack.append(root)
                root = root.left
            
            if not stack:
                return ans
    
            node = stack.pop()
            ans.append(node.val)
            root = node.right
```


