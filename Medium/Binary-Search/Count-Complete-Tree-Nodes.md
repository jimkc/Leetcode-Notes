## Question
Given a complete binary tree, count the number of nodes.<br>

Note:<br>

Definition of a complete binary tree from Wikipedia:<br>
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.


**Example :**   
<pre>
Input: 
    1
   / \
  2   3
 / \  /
4  5 6

Output: 6
</pre>

## Thinking
complete樹高度h，node的數量2^(h+1)-1. (root 高度為0時)

## Coding
Time: O((logn)^2);  </br>
Space: O(logn)
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def countNodes(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        # root_height = h
        h  = self.get_leftest_height(root)
        if h == -1:
            return 0
        else:
            # means that the last node is in the right
            # total node numbers = 2**h-1(left full subtree with height h-1) + 1(root) +right subtree
            if self.get_leftest_height(root.right) == h-1:
                return 2**h + self.countNodes(root.right)
            else:
                # means that the last node is in the left
                # 2**(h-1)-1 (right full subtree with height h-2)+1+left subtree
                return 2**(h-1) + self.countNodes(root.left)
    
    def get_leftest_height(self, node):
        
        # -1 so that height starts from 0 as root
        if node == None:
            return -1
        else:
            return 1+ self.get_leftest_height(node.left)
        
   
```

