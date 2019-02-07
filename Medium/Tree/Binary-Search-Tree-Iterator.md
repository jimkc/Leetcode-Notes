## Question
Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.<br>

Calling next() will return the next smallest number in the BST.<br>

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.


## Thinking
1. Initial stat point is the leftest node<br>
2. nxt check only right child, because left is always smaller ones which we have already go through.<br>
3. If we have a right child, initialize start point to the right childs, leftest.<br>
4. This is an inorder traversal.

## Coding
Time: O(1) In average for next and has nxt ; 
Space: O(h) height. 

```python3
# Definition for a  binary tree node
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator(object):
    def __init__(self, root):
        """x
        :type root: TreeNode
        """
        self.stack = []
        # Store all the leftest path to the smallest node
        while root:
            self.stack.append(root)
            root = root.left
        

    def hasNext(self):
        """
        :rtype: bool
        """
        if self.stack:
            return True
        else:
            return False

    def next(self):
        """
        :rtype: int
        """
        nxt = self.stack.pop()
        ans = nxt.val
        nxt = nxt.right 
        # If the subtree of the popped node exists append the path to the next smallest node to the stack
        while nxt:
            self.stack.append(nxt)
            nxt = nxt.left
            
        return ans
        
        
        
        

# Your BSTIterator will be called like this:
# i, v = BSTIterator(root), []
# while i.hasNext(): v.append(i.next())
```

