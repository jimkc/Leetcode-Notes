## Question
Given a binary tree, find the length of the longest consecutive sequence path.<br>

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

**Example :**   
<pre>
Input:

   1
    \
     3
    / \
   2   4
        \
         5

Output: 3

Explanation: Longest consecutive sequence path is 3-4-5, so return 3.


Input:

   2
    \
     3
    / 
   2    
  / 
 1

Output: 2 

Explanation: Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.
</pre>

## Thinking
1. Use dfs<br>
2. While walking a path from root to leaf, record the longest sequence to the current node, then keep tracing the left node and right node.<br>
3. Compare the left , right(not always longer, might restart from 1 if not parent+1 value) and cur return which is longer and return

## Coding
Time: O(n);<br>
Space: O(n)
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def longestConsecutive(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        return self.walk(root, 0, 0)
    
    def walk(self,node, path, parentval):
        
        if node == None:
            return path
        
        if parentval+1 == node.val: 
            path += 1
        else:
            path = 1 # Restart counting of path length
        left = self.walk(node.left, path, node.val) # Return the largest path length under left
        right = self.walk(node.right, path, node.val)  # Return the largest path length under right
        return max(path,left,right)
```

