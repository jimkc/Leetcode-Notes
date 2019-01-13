## Question
Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list. (last element to head too)

see picture in leetcode.



## Thinking
All process is built on the inorder traverse of BST, and the four steps we do recursively are as below: <br> 
1. 記錄 dfs path到stack<br>


## Coding
Time: O(n); Go through all node once</br>
Space: O(1)
```python3
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left, right):
        self.val = val
        self.left = left
        self.right = right
"""
class Solution:
    def treeToDoublyList(self, root):
        """
        :type root: Node
        :rtype: Node
        """
        
        if not root:return
        dummy = Node(0,None,None)
        prev = dummy # predecessor
        stack = [] # to record the path dfs has walked
        node = root 
        
        while stack or node:
            # record the path to the leftest node we havent been through
            while node:
                stack.append(node)
                node = node.left
            
            # when the loop stops means node is None, so we get one from the stack
            node = stack.pop()    
            # because node moves right, so we need to make new connections (double link) for the prev and the new node
            node.left, prev.right, prev = prev, node, node 
            
            node = node.right # if node.right is none, the stack will just pop a new node
        
        # prev is the last not None node, not node, because the line node=node.right can be None
        dummy.right.left, prev.right = prev, dummy.right
        return dummy.right
            
```

