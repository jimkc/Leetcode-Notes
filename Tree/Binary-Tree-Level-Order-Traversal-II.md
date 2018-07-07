## Question
Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

**Example :**
<pre>
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7

return its bottom-up level order traversal as:

[
  [15,7],
  [9,20],
  [3]
]
</pre>


## Thinking


## Coding
Time: O(n);. </br>
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
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        ans = []
        self.classify = {}
        
        
        def order(root,height):        
            if root == None:
                return None
            
            if height not in self.classify:
                self.classify[height] = [root.val]
            else:
                self.classify[height].append(root.val)
            
            height += 1
            order(root.left,height)
            order(root.right,height)
                
        if root != None:
            order(root,0)
            maxh = max([x for x in self.classify.keys()])
            for i in range(maxh+1)[::-1]:
                ans.append(self.classify[i])
        
        #print (self.classify)
        
        return (ans)
```
