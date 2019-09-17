## Question
Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.<br>

Note: <br>
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Example :**   
<pre>
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1

Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
</pre>

Follow up:<br>
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

## Thinking


## Coding
Time: O(k);<br> 
Space: O(1)
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def kthSmallest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """
        inorder = []
        self.inorderTraversal(root,inorder,k)
        return inorder[-1]
        
    # BST is an inorder traversal
    def inorderTraversal(self, root, inorder, k):
        if root:
            self.inorderTraversal(root.left, inorder, k) 
            if len(inorder) == k:
                return
            inorder.append(root.val)
            self.inorderTraversal(root.right, inorder, k)

```

```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def kthSmallest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """
        
        curr = root
        stack = []
        lst = []
        while (curr is not None or len(stack)>0):
            while curr is not None:
                stack.append(curr)
                curr = curr.left
            curr = stack.pop()
            lst.append(curr.val)
            if len(lst)==k:
                return lst[-1]
            curr = curr.right
```
