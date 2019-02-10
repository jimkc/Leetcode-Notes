## Question
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).<br>

**Example :**   
<pre>
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
return its zigzag level order traversal as:
[
  [3],
  [20,9],
  [15,7]
]
</pre>

## Thinking


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
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        record = {}
        ans = []
        self.dfs(root, 1, record)
        
        for k,v in sorted(record.items()):
            if k % 2 == 0:
                ans.append(v[::-1])
            else:
                ans.append(v)
        return ans
            
    def dfs(self, node, level, record):
        
        if not node:
            return 
        
        if level not in record:
            record[level] = [node.val]
        else:
            record[level].append(node.val)
        
        self.dfs(node.left, level+1, record)
        self.dfs(node.right, level+1, record)
```

