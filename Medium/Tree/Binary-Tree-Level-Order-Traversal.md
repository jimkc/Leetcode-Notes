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
We use len(q) because we only want to pop out all current level nodes and add them to the level list.<br>
Whenever we pop out a node we add its children to the queue, and saved it to pop in the next loop of while.

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

from collections import deque
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        q, result = deque(), []
        if root:
            q.append(root)
        while len(q):
            level = []
            for _ in range(len(q)):
                x = q.popleft()
                level.append(x.val)
                if x.left:
                    q.append(x.left)
                if x.right:
                    q.append(x.right)
            result.append(level)
        return result
```
