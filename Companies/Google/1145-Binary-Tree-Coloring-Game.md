## Question
Two players play a turn based game on a binary tree.  We are given the root of this binary tree, and the number of nodes n in the tree.  n is odd, and each node has a distinct value from 1 to n.<br>
<br>
Initially, the first player names a value x with 1 <= x <= n, and the second player names a value y with 1 <= y <= n and y != x.  The first player colors the node with value x red, and the second player colors the node with value y blue.<br>
<br>
Then, the players take turns starting with the first player.  In each turn, that player chooses a node of their color (red if player 1, blue if player 2) and colors an uncolored neighbor of the chosen node (either the left child, right child, or parent of the chosen node.)<br>
<br>
If (and only if) a player cannot choose such a node in this way, they must pass their turn.  If both players pass their turn, the game ends, and the winner is the player that colored more nodes.<br>
<br>
You are the second player.  If it is possible to choose such a y to ensure you win the game, return true.  If it is not possible, return false.

**Example :**   
<pre>
Pic: https://leetcode.com/problems/binary-tree-coloring-game/description/

Input: root = [1,2,3,4,5,6,7,8,9,10,11], n = 11, x = 3
Output: true
Explanation: The second player can choose the node with value 2.
</pre>

Constraints:<br>
<br>
root is the root of a binary tree with n nodes and distinct node values from 1 to n.<br>
n is odd.<br>
1 <= x <= n <= 100

## Thinking
**Intuition**
The first player colors a node,<br>
there are at most 3 nodes connected to this node.<br>
Its left, its right and its parent.<br>
Take this 3 nodes as the root of 3 subtrees.<br>
<br>
The second player just color any one root,<br>
and the whole subtree will be his.<br>
And this is also all he can take,<br>
since he cannot cross the node of the first player.<br>
<br>

**Explanation**
</b count> will recursively count the number of nodes,<br>
in the left and in the right.<br>
</b n - left - right> will be the number of nodes in the "subtree" of parent.<br>
Now we just need to compare *max(left, right, parent)* and *n / 2*<br>

## Coding
Time: O(n); </br>
Space: O(height)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def btreeGameWinningMove(self, root: TreeNode, n: int, x: int) -> bool:
        
        # cnt of left subtree nodes, right subtree nodes of x and the rest of the nodes in the tree
        # use list object because cant use variable to memorize
        # c[0] c[1] c[2] = left_nodes, right_nodes, other_nodes 
        c = [0, 0, 0]
        
        def countSubTree(node):
            if not node:
                return 0
            l, r = countSubTree(node.left), countSubTree(node.right)
            
            if node.val == x:
                c[0] = l
                c[1] = r
                c[2] = n-l-r-1
                
            # the count of all subtree nodes include itself
            return l+r+1
        
        countSubTree(root)
        
        # the second player can either get one of the c nodes
        return max(c) > n/2
```

