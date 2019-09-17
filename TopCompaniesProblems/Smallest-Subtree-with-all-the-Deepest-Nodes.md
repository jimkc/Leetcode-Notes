## Question
Given a binary tree rooted at root, the depth of each node is the shortest distance to the root.<br>

A node is deepest if it has the largest depth possible among any node in the entire tree.<br>

The subtree of a node is that node, plus the set of all descendants of that node.<br>

Return the node with the largest depth such that it contains all the deepest nodes in its subtree.

**Example :**   
<pre>
Input: [3,5,1,6,2,0,8,null,null,7,4]
Output: [2,7,4]
Explanation:

       3
      / \
     5   1
    / \  /\
   6   2 0 8
      / \
     7   4
We return the node with value 2, colored in yellow in the diagram.
The nodes colored in blue are the deepest nodes of the tree.
The input "[3, 5, 1, 6, 2, 0, 8, null, null, 7, 4]" is a serialization of the given tree.
The output "[2, 7, 4]" is a serialization of the subtree rooted at the node with value 2.
Both the input and output have TreeNode type.
</pre>

Note:<br>

The number of nodes in the tree will be between 1 and 500.<br>
The values of each node are unique.

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

class Solution(object):
    def subtreeWithAllDeepest(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """        
        def markDepthsBelow(node):
            if not node: return 0
            
            r, l = markDepthsBelow(node.right), markDepthsBelow(node.left)
            
            node.depthBelow = max(r, l) # Add a new variable for each node
            return node.depthBelow + 1
            
        markDepthsBelow(root)
        
        cur = root
        while cur:
            if cur.left and cur.right:
                if cur.left.depthBelow == cur.right.depthBelow:
                    return cur
                elif cur.left.depthBelow > cur.right.depthBelow:
                    cur = cur.left
                else:
                    cur = cur.right
            elif cur.left:
                cur = cur.left
            elif cur.right:
                cur = cur.right
            else:
                return cur
```

Faster one:
```python3
class Solution:
    def subtreeWithAllDeepest(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        if not root:
            return None
        return self.helper(root)[1]
    
    def helper(self, root):
        if not root:
            return [0, None]
        left, right = self.helper(root.left), self.helper(root.right)
        if left[0] > right[0]:
            return [left[0] + 1, left[1]]
        elif left[0] < right[0]:
            return [right[0] + 1, right[1]]
        else:
            return [left[0] + 1, root]
```
