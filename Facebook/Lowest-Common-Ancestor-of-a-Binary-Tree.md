## Question
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.<br>

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

**Example :**   
<pre>
Given the following binary tree:  root = [3,5,1,6,2,0,8,null,null,7,4]

        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4



Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of of nodes 5 and 1 is 3.


Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself
             according to the LCA definition.
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        remember_path = [] # Saves the first(lowest) node that meets the requirement 
        
        # This function counts how many target is in the root and subtrees
        def isParent(root, p, q,path):
            
            if root == None: 
                return 0
             
            get = 0 # Count how many target got
            
            if root.val == p.val or root.val == q.val:
                    get+=1
            
            get += isParent(root.left,p,q,path)
            get += isParent(root.right,p,q,path)
            
            if not path and get == 2:
                path.append(root)
            
            return get
            
        isParent(root,p,q,remember_path)
        return remember_path[0]
```

Second method
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if root == None:
            return None
        if root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if left != None and right != None:
            return root
        if left != None and right == None:
            return left
        elif left == None and right != None:
            return right
```

