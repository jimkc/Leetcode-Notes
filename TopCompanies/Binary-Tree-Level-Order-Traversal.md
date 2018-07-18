## Question
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

**Example :**   
<pre>
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]
</pre>

## Thinking


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
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        self.level = []
        
        def BFS(root, lv):
             
            if root == None:
                return None
            
            if lv > len(self.level) -1 :
                self.level.append([root.val])
            else:
                self.level[lv].append(root.val)
            BFS(root.left, lv+1)
            BFS(root.right, lv+1)

            
            
        BFS(root,0)
        
        return  (self.level)
```
