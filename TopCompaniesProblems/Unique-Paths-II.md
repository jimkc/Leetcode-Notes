## Question
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).<br>

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).<br>

Now consider if some obstacles are added to the grids. How many unique paths would there be?

**Example :**   
<pre>
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(1)
```python3
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        
        if obstacleGrid[0][0] == 1:
            return 0
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        
        # Might encouter obstacles after obstacle
        setValue = 1
        for i in range(m):
            if obstacleGrid[i][0] == 1:
                setValue = 0
            obstacleGrid[i][0] = setValue
            
        setValue = 1
        for j in range(1,n):
            if obstacleGrid[0][j] == 1:
                setValue = 0
            obstacleGrid[0][j] = setValue
        
        for i in range(1,m):
            for j in range(1,n):
                if obstacleGrid[i][j] == 1:
                    obstacleGrid[i][j] = 0 # Obstacle has 0 path to go through
                else:
                    obstacleGrid[i][j] = obstacleGrid[i-1][j]+obstacleGrid[i][j-1]
        return obstacleGrid[-1][-1]
```

