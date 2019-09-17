## Question
Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in-place.<br>

**Example :**   
<pre>
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]


Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
</pre>


Follow up:<br>

A straight forward solution using O(mn) space is probably a bad idea.<br>
A simple improvement uses O(m + n) space, but still not the best solution.<br>
Could you devise a constant space solution?<br>

## Thinking


## Coding
Time: O(n); <br>
Space: O(1)

```python3
class Solution:
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: void Do not return anything, modify matrix in-place instead.
        """
        m = len(matrix)
        n = len(matrix[0])
        for i in range(0, m):
            for j in range(0, n):
                if matrix[i][j] == 0:
                    for ii in range(0, n): # Set the path we have walked to 0
                        if ii < j:
                            matrix[i][ii] = 0
                        elif ii > j and matrix[i][ii] != 0:
                            matrix[i][ii] = None  # Set the future path to be None, so we wont mistaken original not 0
                    for jj in range(0, m):
                        if jj < i:
                            matrix[jj][j] = 0
                        elif jj > i and matrix[jj][j] != 0:
                            matrix[jj][j] = None
        for i in range(0, m):
            for j in range(0, n):
                if matrix[i][j] is None:
                    matrix[i][j] = 0
```
