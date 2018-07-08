## Question
Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.<br>

**Example :**
<pre>
Given tree s:

     3
    / \
   4   5
  / \
 1   2
Given tree t:
   4 
  / \
 1   2
Return true, because t has the same structure and node values with a subtree of s.


Given tree s:

     3
    / \
   4   5
  / \
 1   2
    /
   0
Given tree t:
   4
  / \
 1   2
Return false.
</pre>

## Thinking


## Coding
Time: O();  </br>
Space: O().
```python3
class Solution:
    def isSubtree(self, s, t):
        """
        :type s: TreeNode
        :type t: TreeNode
        :rtype: bool
        """
        def check(s,t):
            # helper function that does the actual subtree check
            if (s is None) and (t is None):
                return True
            if (s is None) or (t is None):
                return False
            return (s.val == t.val and check(s.left, t.left) and check(s.right, t.right)) #If s.val!=t.val, check left and right wont be called
        
        # need to do a pre-order traversal and do a check
        # for every node we visit for the subtree
        if s == None:
            return False
        if check(s,t):
            return True
        if self.isSubtree(s.left,t) or self.isSubtree(s.right,t):
            return True
        return False
```

