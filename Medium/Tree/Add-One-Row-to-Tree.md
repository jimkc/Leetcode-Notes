## Question
Given the root of a binary tree, then value v and depth d, you need to add a row of nodes with value v at the given depth d. The root node is at depth 1.<br>

The adding rule is: given a positive integer depth d, for each NOT null tree nodes N in depth d-1, create two tree nodes with value v as N's left subtree root and right subtree root. And N's original left subtree should be the left subtree of the new left subtree root, its original right subtree should be the right subtree of the new right subtree root. If depth d is 1 that means there is no depth d-1 at all, then create a tree node with value v as the new root of the whole original tree, and the original tree is the new root's left subtree.

**Example :**   
<pre>
Input: 
A binary tree as following:
       4
     /   \
    2     6
   / \   / 
  3   1 5   

v = 1

d = 2

Output: 
       4
      / \
     1   1
    /     \
   2       6
  / \     / 
 3   1   5   


Input: 
A binary tree as following:
      4
     /   
    2    
   / \   
  3   1    

v = 1

d = 3

Output: 
      4
     /   
    2
   / \    
  1   1
 /     \  
3       1
</pre>

Note:<br>
The given d is in range [1, maximum depth of the given tree + 1].<br>
The given binary tree has at least one tree node.

## Thinking


## Coding
Time: O(n);<br> 
Space: O()
```python3
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def addOneRow(self, root, v, d):
        """
        :type root: TreeNode
        :type v: int
        :type d: int
        :rtype: TreeNode
        """
        if d == 1:
            new = TreeNode(v)
            new.left = root
            return new
        
        self.addRow(root, 1, v, d)
        return root
    
    def addRow(self, node, level, v, d):
        if node == None:
            return
        
        if level == d-1:
            if node.left:
                tmpl = node.left
                new = TreeNode(v)
                node.left = new
                new.left = tmpl
            else:
                node.left = TreeNode(v)
                
            if node.right:
                tmpr = node.right
                new = TreeNode(v)
                node.right = new
                new.right = tmpr
            else:
                node.right = TreeNode(v)
            return
        
        self.addRow(node.left, level+1, v, d)
        self.addRow(node.right, level+1, v, d)
```

