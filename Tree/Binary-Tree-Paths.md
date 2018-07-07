## Question
Given a binary tree, return all root-to-leaf paths.

**Example :**
<pre>
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3
</pre>

## Thinking


## Coding
Time: O(n);  </br>
Space: O(n).
```python3
class Solution:
    def canConstruct(self, ransomNote, magazine):
        """
        :type ransomNote: str
        :type magazine: str
        :rtype: bool
        """
        cnt_ransom = collections.Counter(ransomNote)
        cnt_mag = collections.Counter(magazine)
        
        for tup in cnt_ransom.most_common():
            if tup[1] > cnt_mag[tup[0]]:
                return False
        return True# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        ans = []
        def walk(root, path):
            
            path += str(root.val)
            if root.left == None and root.right == None:
                ans.append(path)
            else:
                path += '->'
                if root.left != None:
                    walk(root.left, path)
                if root.right != None:
                    walk(root.right, path)

            
        if root != None:
            walk(root, '')
            
        return ans
```
