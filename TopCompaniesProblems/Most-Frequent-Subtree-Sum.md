## Question
Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.<br>

**Example :**   
<pre>
Input:

  5
 /  \
2   -3
return [2, -3, 4], since all the values happen only once, return all of them in any order.

Input:

  5
 /  \
2   -5
return [2], since 2 happens twice, however -5 only occur once.
</pre>

Note: You may assume the sum of values in any subtree is in the range of 32-bit signed integer.

## Thinking


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
    def findFrequentTreeSum(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        
        sums = [] # Sum of subtree for each node
        self.walk(root,sums)
        cnt = collections.Counter(sums)
        max_freq = max(cnt.values())
        return [s for s in cnt.keys() if cnt[s] == max_freq]
        
    # Get sum of each nodes subtree and record in sums
    def walk(self, node, sums):
        
        if node == None:
            return 0
        
        getSum = node.val
        getSum += self.walk(node.left, sums)
        getSum += self.walk(node.right, sums)
        
        sums.append(getSum)
        return getSum
        
```
