## Question
Return any binary tree that matches the given preorder and postorder traversals.<br>

Values in the traversals pre and post are distinct positive integers.

**Example :**   
<pre>
Input: pre = [1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
Output: [1,2,3,4,5,6,7]
</pre>

Note:<br>
<br>
1 <= pre.length == post.length <= 30<br>
pre[] and post[] are both permutations of 1, 2, ..., pre.length.<br>
It is guaranteed an answer exists. If there exists multiple answers, you can return any of them.

## Thinking
**Approach 1**: Recursion<br>
**Intuition**<br>
<br>
A preorder traversal is:<br>
<br>
(root node) (preorder of left branch) (preorder of right branch)<br>
While a postorder traversal is:<br>
<br>
(postorder of left branch) (postorder of right branch) (root node)<br>
For example, if the final binary tree is [1, 2, 3, 4, 5, 6, 7] (serialized), then the preorder traversal is [1] + [2, 4, 5] + [3, 6, 7], while the postorder traversal is [4, 5, 2] + [6, 7, 3] + [1].<br>
<br>
If we knew how many nodes the left branch had, we could partition these arrays as such, and use recursion to generate each branch of the tree.<br>
<br>
Algorithm<br>
<br>
Let's say the left branch has LL nodes. We know the head node of that left branch is pre[1], but it also occurs last in the postorder representation of the left branch. So pre[1] = post[L-1] (because of uniqueness of the node values.) Hence, L = post.indexOf(pre[1]) + 1.<br>
<br>
Now in our recursion step, the left branch is represnted by pre[1 : L+1] and post[0 : L], while the right branch is represented by pre[L+1 : N] and post[L : N-1].<br>
<br>
**Approach 2**: Recursion (Space Saving Variant)<br>
Explanation<br>
<br>
We present a variation of Approach 1 that uses indexes to refer to the subarrays of pre and post, instead of passing copies of those subarrays. Here, (i0, i1, N) refers to pre[i0:i0+N], post[i1:i1+N].<br>

## Coding
Method 1:<br>
Time: O(n^2); n is number of nodes</br>
Space: O(n^2)<br>
<br>
Method 2:<br>
Time: O(n^2); n is number of nodes</br>
Space: O(n)<br>



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def constructFromPrePost(self, pre: List[int], post: List[int]) -> TreeNode:
        if not pre: return None
        
        root = TreeNode(pre[0])
        if len(pre) == 1: return root
        
        # pre[1] is the root of left tree 
        # and also the last node to traverse for post order in left tree
        L = post.index(pre[1])+1
        root.left = self.constructFromPrePost(pre[1:L+1], post[:L])
        root.right = self.constructFromPrePost(pre[L+1:], post[L:-1])
        return root
```

```python
class Solution(object):
    def constructFromPrePost(self, pre, post):
        def make(i0, i1, N):
            if N == 0: return None
            root = TreeNode(pre[i0])
            if N == 1: return root

            for L in xrange(N):
                if post[i1 + L - 1] == pre[i0 + 1]:
                    break

            root.left = make(i0 + 1, i1, L)
            root.right = make(i0 + L + 1, i1 + L, N - 1 - L)
            return root

        return make(0, 0, len(pre))
```

