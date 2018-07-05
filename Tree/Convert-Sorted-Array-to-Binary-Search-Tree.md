## Question
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.<br>

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

**Example :**
<pre>
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
</pre>

Note:
You may assume that both strings contain only lowercase letters.

## Thinking
For a sorted array, the left half will be in the left subtree, middle value as the root, right half in the right subtree. This holds through for every node:<br>

[1, 2, 3, 4, 5, 6, 7] -> left: [1, 2, 3], root: 4, right: [5, 6, 7]<br>
[1, 2, 3] -> left: [1], root: 2, right: [3]<br>
[5, 6, 7] -> left: [5], root: 6, right: [7]<br>

Many of the approaches here suggest slicing an array recursively and passing them. However, slicing the array is expensive. It is better to pass the left and right bounds into recursive calls instead.<br>

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
    def sortedArrayToBST(self, nums):
        """
        :type nums: List[int]
        :rtype: TreeNode
        """
        def toBST(left,right):
            if left > right:
                return None
                        
            mid = (left + right)//2
            root = TreeNode(nums[mid])
            root.left = toBST(left,mid-1)
            root.right = toBST(mid+1,right)
        
            return root
        return toBST(0, len(nums)-1)
```
