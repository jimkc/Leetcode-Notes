## Question
Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.<br>

**Example :**
<pre>
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True

Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 28

Output: False
</pre>

## Thinking


## Coding
Time: O(n); </br>
Space: O(n+logn).
```python3
class Solution:
    def findTarget(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: bool
        """
        
        seen = {}
        def find(root):
            
            if root == None:
                return None
            
            if root.val in seen:
                seen[root.val] += 1
            else:
                seen[root.val] = 0
            
            find(root.left)
            find(root.right)
            
        find(root)
        for n,t in seen.items():
            if k-n == n:
                if seen[n] >= 2:
                    return True
            elif k-n in seen:
                return True
        return False
```
