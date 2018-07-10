## Question
Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

**Example :**
<pre>
Input:
    3
   / \
  9  20
    /  \
   15   7
Output: [3, 14.5, 11]
Explanation:
The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].
</pre>

Note:
You may assume that both strings contain only lowercase letters.

## Thinking


## Coding
Time: O(n);  </br>
Space: O(n).
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def averageOfLevels(self, root):
        """
        :type root: TreeNode
        :rtype: List[float]
        """
        
    
        def addLevel(sumlist, countlist, height, root):
            if not root:
                return 0
            
            if height >= len(sumlist): #In case there isnt a place to store height
                sumlist.append(0)
            if height >= len(countlist):
                countlist.append(0)
                
            sumlist[height] += root.val
            countlist[height] += 1
            addLevel(sumlist, countlist, height+1, root.left)
            addLevel(sumlist, countlist, height+1, root.right)
        
        countlist = []
        sumlist = []
        addLevel(sumlist, countlist, 0, root)
        
        return [a/b for a,b in zip(sumlist, countlist)]
```

