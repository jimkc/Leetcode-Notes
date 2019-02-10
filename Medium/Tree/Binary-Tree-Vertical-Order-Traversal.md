## Question
Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).<br>

If two nodes are in the same row and column, the order should be from left to right.

**Example :**   
<pre>
Input: [3,9,20,null,null,15,7]

   3
  /\
 /  \
 9  20
    /\
   /  \
  15   7 

Output:

[
  [9],
  [3,15],
  [20],
  [7]
]


Input: [3,9,8,4,0,1,7]

     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7 

Output:

[
  [4],
  [9],
  [3,0,1],
  [8],
  [7]
]


Input: [3,9,8,4,0,1,7,null,null,null,2,5] (0's right child is 2 and 1's left child is 5)

     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
    /\
   /  \
   5   2

Output:

[
  [4],
  [9,5],
  [3,0,1],
  [8,2],
  [7]
]
</pre>

## Thinking
1.  Use BFS instead of DFS because the record values in an index have to traverse from top to down.<br>
2.  Use a dequeue to traverse node idx and value<br>
3.  Use a dictionary to record node for each index


## Coding
Time: O(n); <br>
Space: O(n)
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def verticalOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        
        if root == None:
            return []
        q = collections.deque([(root, 0)])
        record = collections.defaultdict(list)
        
        # BFS
        while q:
            node, ind = q.popleft()
            
            record[ind].append(node.val)

            if node.left:
                q.append((node.left, ind-1))
            if node.right:
                q.append((node.right, ind+1))
                
        
        return [v for k,v in sorted(record.items())] 
```

