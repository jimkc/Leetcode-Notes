## Question
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).<br>

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).<br>

How many possible unique paths are there?

**Example :**   
<pre>
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right


Input: m = 7, n = 3
Output: 28
</pre>

## Thinking


## Coding
Time: O(mn); <br>
Space: O(mn)
```python3
class Solution:
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        grid = [[0]*n for i in range(m)]
        
        for i in range(n):
            grid [0][i] = 1
        for i in range(m):
            grid[i][0] = 1
            
    
        for i in range(1,m):
            for j in range(1,n):
                grid[i][j] = grid[i-1][j] + grid[i][j-1]
        
        return grid[-1][-1]
```

