## Question
Given a binary tree, return the values of its boundary in anti-clockwise direction starting from root. Boundary includes left boundary, leaves, and right boundary in order without duplicate nodes.<br>

Left boundary is defined as the path from root to the left-most node. Right boundary is defined as the path from root to the right-most node. If the root doesn't have left subtree or right subtree, then the root itself is left boundary or right boundary. Note this definition only applies to the input binary tree, and not applies to any subtrees.<br>

The left-most node is defined as a leaf node you could reach when you always firstly travel to the left subtree if exists. If not, travel to the right subtree. Repeat until you reach a leaf node.<br>

The right-most node is also defined by the same way with left and right exchanged.

**Example :**   
<pre>
Input:
  1
   \
    2
   / \
  3   4

Ouput:
[1, 3, 4, 2]

Explanation:
The root doesn't have left subtree, so the root itself is left boundary.
The leaves are node 3 and 4.
The right boundary are node 1,2,4. Note the anti-clockwise direction means you should output reversed right boundary.
So order them in anti-clockwise without duplicates and we have [1,3,4,2].


Input:
    ____1_____
   /          \
  2            3
 / \          / 
4   5        6   
   / \      / \
  7   8    9  10  
       
Ouput:
[1,2,4,7,8,9,10,6,3]

Explanation:
The left boundary are node 1,2,4. (4 is the left-most node according to definition)
The leaves are node 4,7,8,9,10.
The right boundary are node 1,3,6,10. (10 is the right-most node).
So order them in anti-clockwise without duplicate nodes we have [1,2,4,7,8,9,10,6,3].
</pre>

## Thinking


## Coding
Time: O(n);
Space: O(n)
```python3
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def boundaryOfBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: Lisnt[int]
        """
        if not root:
            return []
        elif not root.left and not root.right:
            return [root.val]
        
        leaves = []
        left_bound = []
        right_bound = []
        self.getLeaf(root,leaves)
        
        
        # If left bound exisits, record left bounnd except root
        node = root.left
        while node :
            left_bound.append(node.val)
            node = node.left if node.left else node.right
        
        # If right ...
        node = root.right
        while node:
            right_bound.append(node.val)
            node = node.right if node.right else node.left
            
        # Remove duplicate left leaf and right leaf value
        return [root.val]+left_bound[:-1] + leaves + right_bound[::-1][1:]
        
        
    # Get values of all leaves
    def getLeaf(self, node, leaves):
        
        if not node:
            return 
        
        if not node.left and not node.right:
            leaves.append(node.val)

        self.getLeaf(node.left, leaves)
        self.getLeaf(node.right, leaves)
```

