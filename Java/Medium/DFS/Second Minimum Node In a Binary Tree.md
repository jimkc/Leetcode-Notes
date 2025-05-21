## Question
Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property root.val = min(root.left.val, root.right.val) always holds.  
  
Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.  
  
If no such second minimum value exists, output -1 instead.  

https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/description/?envType=company&envId=linkedin&favoriteSlug=linkedin-three-months  

**Example 1:**
<pre>
Input: root = [2,2,5,null,null,5,7]
Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.
</pre>

**Example 2:**
<pre>
Input: root = [2,2,2]
Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
</pre>

Constraints:
* The number of nodes in the tree is in the range [1, 25].
* 1 <= Node.val <= 231 - 1
* root.val == min(root.left.val, root.right.val) for each internal node of the tree.

## Thinking
https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/solutions/127571/second-minimum-node-in-a-binary-tree

## Coding
Time: O(n). 
Space: O(log(n));
### Solution
```java
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
    int min1;
    long min2 = Long.MAX_VALUE;
    public int findSecondMinimumValue(TreeNode root) {
        // as stated in the problem the root can only be <= root.child
        min1 = root.val;
        dfs(root);
        return min2 < Long.MAX_VALUE ? (int)min2: -1;
    }

    public void dfs(TreeNode root) {
        // 1. min1 < root.val, min2 is set as max in begining so min2 always > min1
        // 2. min1 == root.val, continue traverse
        // 3. min1 > root.val, can't happen because root will be the smallest among all

        if (root != null) {
            if (min1 < root.val && root.val < min2) {
                // stop condition
                min2 = root.val;
            } else if (min1 == root.val) {
                dfs(root.left);
                dfs(root.right);
            }
        }  
    }
 }
```