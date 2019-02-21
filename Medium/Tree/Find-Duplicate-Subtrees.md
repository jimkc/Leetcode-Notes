## Question
Given a binary tree, return all duplicate subtrees. For each kind of duplicate subtrees, you only need to return the root node of any one of them.

Two trees are duplicate if they have the same structure with same node values.

**Example :**   
<pre>
     1
       / \
      2   3
     /   / \
    4   2   4
       /
      4
The following are two duplicate subtrees:

      2
     /
    4
and

    4

Therefore, you need to return above trees' root in the form of a list.
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def findDuplicateSubtrees(self, root):
        """
        :type root: TreeNode
        :rtype: List[TreeNode]
        """
        seen = {}
        ans = []
        self.getSubtreePath(root,seen, ans)
        return ans
        
    def getSubtreePath(self,node, seen, ans):
        
        if not node:
            return '_' # Important!! to record null in left or right
        
        path = str(node.val)
        path += self.getSubtreePath(node.left, seen, ans)
        path += self.getSubtreePath(node.right, seen, ans)
        
        if path not in seen:
            seen[path] = 1
        else:
            if seen[path] == 1:
                ans.append(node)
            seen[path]+=1

        return path
```

