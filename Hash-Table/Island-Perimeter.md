## Question
You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

**Example :**
<pre>
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

Answer: 16
</pre>

## Thinking
Add 0s around for easier coding (Should try to solve without adding 0s).

## Coding
Time: O(MN); </br>
Space: O(1).
```python3
class Solution:
    def islandPerimeter(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        perim = 0
        for i in range(len(grid)):
            grid[i].insert(0,0)
            grid[i].append(0)
        grid.insert(0,[0]*len(grid[0]))
        grid.append([0]*len(grid[0]))
        
        for j in range(1,len(grid)-1):
            for i in range(1,len(grid[1])-1):
                if grid[j][i] == 1:
                    land_near = grid[j-1][i] + grid[j][i-1] + grid[j+1][i] + grid[j][i+1]
                    perim += (4 - land_near)
        return perim
```

    
