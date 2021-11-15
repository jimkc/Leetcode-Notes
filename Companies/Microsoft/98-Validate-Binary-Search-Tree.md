## Question
Given the root of a binary tree, determine if it is a valid binary search tree (BST).  
  
A valid BST is defined as follows:  
  
* The left subtree of a node contains only nodes with keys less than the node's key.
* The right subtree of a node contains only nodes with keys greater than the node's key.
* Both the left and right subtrees must also be binary search trees.
  
**Example 1:**   
<pre>
Input: root = [2,1,3]
Output: true
</pre>
  
**Example 2:**   
<pre>
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
</pre>
  
**Constraints:**
The number of nodes in the tree is in the range [1, 104].  
-231 <= Node.val <= 231 - 1  

  
## Thinking
* We can easy to realise that this is a Dynamic Programming problem. In operation ith, we can choose to pick the left or the right side of nums.  
* The naive dp has 3 params which is dp(l, r, i), time complexity = O(m^3), l and r can be picked up to m numbers.
* We can optimize to 2 params which is dp(l, i), time complexity = O(m^2) , we can compute params r base on l and i:  

* Where:  
 * l, r is the index of the left side and the right side corresponding.
 * i is the number of elements that we picked.
 * leftPicked: is the number of elements picked on the left side, leftPicked = l.
 * rightPicked: is the number of elements picked on the right side, rightPicked = i - leftPicked = i - l.
 * Finally: r = n - rightPicked - 1 = n - (i-l) - 1.

## Coding
Time: O(n);  
Space: O(logn);

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isValidBST(TreeNode root) { 
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    public boolean isValidBST(
        final TreeNode root, final long minVal, final long maxVal)
    {
        if (root == null){
            return true;
        }
        
        if(root.val <= minVal || root.val >= maxVal){
            return false;
        }
        return isValidBST(root.left, minVal, root.val) && isValidBST(root.right, root.val, maxVal);
    }
}
```

