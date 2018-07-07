## Question
Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes.<br>

Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.<br>

If no such second minimum value exists, output -1 instead.<be>

**Example :**
<pre>
Input: 
    2
   / \
  2   5
     / \
    5   7

Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.


Input: 
    2
   / \
  2   2

Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
</pre>

Note:
You may assume that both strings contain only lowercase letters.

## Thinking


## Coding
Time: O(n); . </br>
Space: O(n).
```python3
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def findSecondMinimumValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        seen = set()
        
        def find(node):
            if node == None:
                return None
            
            seen.add(node.val)
            
            find(node.left)
            find(node.right)

        if root != None:
            find(root)
        
        if len(seen) < 2:
            return -1
        else:
            least = min(list(seen))
            seen.remove(least)
            secondleast = min(list(seen))
            return secondleast
```
