## Question
Given a binary tree, determine if it is height-balanced.<br>

For this problem, a height-balanced binary tree is defined as:<br>

a binary tree in which the depth of the two subtrees of every node never differ by more than 1.<br>

**Example :**
<pre>
Given the following tree [3,9,20,null,null,15,7]:

    3
   / \
  9  20
    /  \
   15   7
   
Return true.

Given the following tree [1,2,2,3,3,null,null,4,4]:

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
Return false.
</pre>


## Thinking
We can compare while running dfs.

## Coding
Time: O(n); . </br>
Space: O(n).
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        self.ans = True
        # Get the height of the node
        def getHeight(node):
            height = 0
            
            if node == None:
                return height
            else:
                height += 1
                
            a = height + getHeight(node.left)
            b = height + getHeight(node.right)
            
            if abs(a-b) > 1:
                self.ans = False

            return max(a,b)
        
        getHeight(root)
        return self.ans
        
```
