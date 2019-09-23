## Question
Given the root of a binary tree, each node in the tree has a distinct value.<br>

After deleting all nodes with a value in to_delete, we are left with a forest (a disjoint union of trees).<br>

Return the roots of the trees in the remaining forest.  You may return the result in any order.

**Example :**   
<pre>
Check pic: https://leetcode.com/problems/delete-nodes-and-return-forest/description/

Input: root = [1,2,3,4,5,6,7], to_delete = [3,5]
Output: [[1,2,null,4],[6],[7]]
</pre>

Constraints:

The number of nodes in the given tree is at most 1000.<br>
Each node has a distinct value between 1 and 1000.<br>
to_delete.length <= 1000 <br>
to_delete contains distinct values between 1 and 1000.<br>

## Thinking
**Question analysis**<br>
The question is composed of two requirements:<br>

To remove a node, the child need to **notify** its parent about the child's existance.
To determine whether a node is a root node in the final forest, we need to know [1] whether the node is removed (which is trivial), and [2] whether its parent is removed (which requires the parent to **notify** the child)
It is very obvious that a tree problem is likely to be solved by recursion. The two components above are actually examining interviewees' understanding to the two key points of recursion:

passing info downwards -- by arguments
passing info upwards -- by return value


## Coding
Time: O(n); n nodes in the tree </br>
Space: O(n)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def delNodes(self, root, to_delete):
        to_delete = set(to_delete)
        # only need to record the root node for each tree
        # the problem will get all tree nodes from the root
        res = []
        def walk(root, parent_exist):
            if root is None:
                return None
            if root.val in to_delete:
                root.left = walk(root.left, parent_exist=False)
                root.right = walk(root.right, parent_exist=False)
                return None
            else:
                if not parent_exist:
                    res.append(root)
                    
                root.left = walk(root.left, parent_exist=True)
                root.right = walk(root.right, parent_exist=True)
                return root
        walk(root, parent_exist=False)
        
        return res
```

