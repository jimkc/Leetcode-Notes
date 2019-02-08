## Question
Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

**Example :**   
<pre>
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
</pre>

## Thinking
Always go left first, and we have to do dfs, so that we can make sure we go through to the deepest node.<br>
Record  the node only when we see it at the first sight.


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
    def rightSideView(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        ans = []
        self.seeRightest(root, ans, 1)
        return ans
        
    def seeRightest(self,node,ans,depth):
            
        if node == None:
            return None

        if depth > len(ans):
            ans.append(node.val)

        self.seeRightest(node.right, ans, depth+1)
        self.seeRightest(node.left, ans, depth+1)    
          
```
