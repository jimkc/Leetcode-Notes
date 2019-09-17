## Question
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.<br>

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Example :**   
<pre>
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
</pre>

Clarification: The above format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.<br>

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
       
        
        def dfs(node, path):
            if node:
                path.append(str(node.val))
                dfs(node.left,path)
                dfs(node.right,path)
            else:
                path.append('#')
                
        path = []
        dfs(root, path)
        #print ('['+','.join(path)+']')
        return ' '.join(path)
                                
                                
                                
                
            
        

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        
        
        def dfs(nodev, idx):
            idx[0] = idx[0]+1
            if nodev[idx[0]] == '#':
                return None
            node = TreeNode(int(nodev[idx[0]]))
            node.left = dfs(nodev,idx)
            node.right = dfs(nodev,idx)
            return node
        
        nodev = data.split(' ')
        idx = [-1]
        return dfs(nodev, idx)
        
        
        

# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))
```

