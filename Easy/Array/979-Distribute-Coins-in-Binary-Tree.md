## Question
Given the root of a binary tree with N nodes, each node in the tree has node.val coins, and there are N coins total.<br>

In one move, we may choose two adjacent nodes and move one coin from one node to another.  (The move may be from parent to child, or from child to parent.)<br>

Return the number of moves required to make every node have exactly one coin.<br>

**Example :**   
<pre>
see pictures: https://leetcode.com/problems/distribute-coins-in-binary-tree/
</pre>

## Thinking


## Coding
Time: O(n); n nodes</br>
Space: O(h) height of the tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def distributeCoins(self, root: TreeNode) -> int:
        
        moves = [0]
        
        def coinsToAncesstor(node: TreeNode):
            
            if not node:
                return 0
            
            leftCoins, rightCoins = coinsToAncesstor(node.left), coinsToAncesstor(node.right)
            
            # even of the coins to ancesstor is negative it just means the direction is opposite
            moves[0] += abs(leftCoins) + abs(rightCoins)
            
            # left and right coins to ancesstor can be negative
            # means that they need one from ancesstor
            return leftCoins + rightCoins + node.val -1
        
        coinsToAncesstor(root)
        return moves[0]
        
```

