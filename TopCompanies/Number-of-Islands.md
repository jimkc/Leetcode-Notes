## Question
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example :**   
<pre>
Input:
11110
11010
11000
00000

Output: 1
</pre>

## Thinking

## Coding
Time: O(n); Go through all coordinates once.<br>
Space: O(1) No additional space used.
```python3
class Solution:
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        
        
        cnt = 0
        
        def walk(grid,i,j):
            
            grid[i][j] = '2'
            #print (i,j,grid)
            if i+1 < len(grid) and grid[i+1][j] == '1':walk(grid,i+1,j)
            if j+1 < len(grid[0]) and grid[i][j+1] == '1':walk(grid,i,j+1)
            if i-1 >= 0 and grid[i-1][j] == '1':walk(grid,i-1,j)
            if j-1 >= 0 and grid[i][j-1] == '1':walk(grid,i,j-1)
            
            
        
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    walk(grid,i,j)
                    cnt += 1
                    
                
        return cnt
```

