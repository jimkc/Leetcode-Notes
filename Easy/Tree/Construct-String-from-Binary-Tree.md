## Question
You need to construct a string consists of parenthesis and integers from a binary tree with the preorder traversing way.<br>

The null node needs to be represented by empty parenthesis pair "()". And you need to omit all the empty parenthesis pairs that don't affect the one-to-one mapping relationship between the string and the original binary tree.<br>

**Example :**
<pre>
Input: Binary tree: [1,2,3,4]
       1
     /   \
    2     3
   /    
  4     

Output: "1(2(4))(3)"

Explanation: Originallay it needs to be "1(2(4)())(3()())", 
but you need to omit all the unnecessary empty parenthesis pairs. 
And it will be "1(2(4))(3)".


Input: Binary tree: [1,2,3,null,4]
       1
     /   \
    2     3
     \  
      4 

Output: "1(2()(4))(3)"

Explanation: Almost the same as the first example, 
except we can't omit the first parenthesis pair to break the one-to-one mapping relationship between the input and the output.
</pre>


## Thinking


## Coding
Time: O(n);  </br>
Space: O(logn).
```python3
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def tree2str(self, t):
        """
        :type t: TreeNode
        :rtype: str
        """
        self.string = ''
        
        def toStr(t):
            if t == None:
                return None
            
            self.string += str(t.val)
            if t.left != None:
                self.string += '('
                toStr(t.left)
                self.string += ')'
            if t.right != None:
                if t.left == None:     
                    self.string += '()' # While left is None and right is not None, we need to append () so the right node wont                                             be mistaken as the left node 
                self.string += '('
                toStr(t.right)
                self.string += ')'
            
            
        
        toStr(t)
        return (self.string)
    
```

