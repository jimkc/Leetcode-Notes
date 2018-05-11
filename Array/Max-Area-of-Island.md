## Question
Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

**Example 1:**
<pre>
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
</pre>
Given the above grid, return 6. Note the answer is not 11, because the island must be connected 4-directionally.</br>

**Example 2:**
<pre>
[[0,0,0,0,0,0,0,0]]
</pre>

Given the above grid, return 0.</br>
Note: The length of each dimension in the given grid does not exceed 50.

## Thinking
Recusively go through the four directions of each spot. The main point is the if statement and the return part of getarea().</br>
## Coding
Time Complexity: O(R*C), where R is the number of rows in the given grid, and C is the number of columns. We visit every square once. </br>
Space complexity: O(R*C), the space used by seen to keep track of visited squares, and the space used by the call stack during our recursion.
```python3
class Solution:
    def maxAreaOfIsland(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        seen = set()
        
        def GetArea(self,r,c):
            if (0 <= r < len(grid) and 0 <= c < len(grid[0]) 
                and (r,c) not in seen and grid[r][c]) :
                seen.add((r,c))
                return (1+GetArea(self,r-1,c)+GetArea(self,r+1,c)+GetArea(self,r,c-1)+GetArea(self,r,c+1))
            else:
                return 0
            
        return(max(GetArea(self,r,c)
                    for r in range(len(grid))
                    for c in range(len(grid[0]))
                  ))
        
```

