## Question
Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

**Example :**   
<pre>
Input: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Output: 4
</pre>

## Thinking


## Coding
Time: O(mn); <br>
Space: O(mn)
```python3
class Solution:
    def maximalSquare(self, matrix):
        """
        :type matrix: List[List[str]]
        :rtype: int
        """
        if not matrix or not matrix[0]:
            return 0
        
        maxsidelength = 0
        m = len(matrix)
        n = len(matrix[0])
        
        grid = [[0 for _ in range(n+1)]  for _ in range(m+1)]
            
        # make row and column add 1, the boundary of matrix in dp will be the same
        for i in range(1,m+1):
            for j in range(1,n+1):
                if matrix[i-1][j-1] == '1':
                    grid[i][j] = min(grid[i-1][j-1], grid[i][j-1], grid[i-1][j])+1
                    maxsidelength = max(maxsidelength, grid[i][j])
        
        return maxsidelength**2
```

