## Question
We are given a binary tree (with root node root), a target node, and an integer value K.<br>

Return a list of the values of all nodes that have a distance K from the target node.  The answer can be returned in any order.


**Example :**   
<pre>
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2

Output: [7,4,1]

Explanation: 
The nodes that are a distance 2 from the target node (with value 5)
have values 7, 4, and 1.
</pre>

Note:<br>

The given tree is non-empty.:<br>
Each node in the tree has unique values 0 <= node.val <= 500.:<br>
The target node is a node in the tree.:<br>
0 <= K <= 1000

## Thinking
1. Recoed all the parents for the node, so we can do bfs for target node
2. Use a queus to do bfs
3. Check if the node has seen for bfs with set, so we dont traverse back (important part for bfs)

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
    def distanceK(self, root, target, K):
        """
        :type root: TreeNode
        :type target: TreeNode
        :type K: int
        :rtype: List[int]
        """
        self.getparent(root,None)
        q = collections.deque([[target,0]])
        seen = {target}
        ans = []
        
        # Do BFS from the target
        while q:
            if q[0][1] == K:
                ans.append(q[0][0].val)
            
            # queue    
            node, distance = q.popleft()
            
            for nei in [node.par, node.left, node.right]:
                if nei and nei not in seen: # so we dont walk back, and distance can always +1
                    q.append([nei, distance+1])
                    seen.add(nei)
        
        return ans
    
    # Record parent for each node
    def getparent(self,node, par):
        
        if not node:
            return 
        
        node.par = par
        self.getparent(node.left, node)
        self.getparent(node.right, node)
```
