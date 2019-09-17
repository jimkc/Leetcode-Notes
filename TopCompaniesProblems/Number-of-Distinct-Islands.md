## Question
Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.<br>

Count the number of distinct islands. An island is considered to be the same as another if and only if one island can be translated (and not rotated or reflected) to equal the other.

**Example :**   
<pre>
11000
11000
00011
00011

Given the above grid map, return 1.

11011
10000
00001
11011

Given the above grid map, return 3.


11
1

and

 1
11
are considered different island shapes, because we do not consider reflection / rotation.
</pre>


## Thinking


## Coding
Time: O(n); n for grid size<br>
Space: O(n)
```python3
class Solution:
    def numDistinctIslands(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
    
        shape = set()
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] != 1:
                    continue
                    
                island = self.getIsland(grid, i, j)
                
                if len(island) > 0:
                    # Actually we dont need to sort to count relative pos val
                    # Because dfs always walk with the same pattern
                    # So the val append to the island will always be the same order
                    #island.sort(key = lambda x: (x[0], x[1])) 
                    relative_pos = self.getRelative(island)
                    
                    if relative_pos not in shape:
                        shape.add(relative_pos)

        return len(shape)
        
        
    # island is sorted
    def getRelative(self, island):
        tmp = []
        for i in range(len(island)-1):
            tmp.append((island[i][0]-island[i+1][0], island[i][1]-island[i+1][1]))
        return tuple(tmp) # Tuple could be put in set
            
    # Get island positions
    def getIsland(self, grid, i, j):
        tmp = []
        
        if grid[i][j] != 1:
            return []
        else:
            grid[i][j] = 2 # Prevent stepping on the counted 1s
            tmp.append((i,j))
        
        if i+1 < len(grid): tmp += self.getIsland(grid, i+1, j) 
        if j+1 < len(grid[0]): tmp += self.getIsland(grid, i, j+1) 
        if i-1 >= 0: tmp += self.getIsland(grid, i-1, j) 
        if j-1 >= 0: tmp += self.getIsland(grid, i, j-1) 
        
        return tmp
```

Faster without counting relative position:
```python3
class Solution:
    def numDistinctIslands(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
#                 # record the dfs path of each island
#         def dfs(grid, row, col, cur, path):
#             grid[row][col] = 0
#             path.append(cur)
#             # up
#             if row and grid[row-1][col]==1:
#                 dfs(grid, row-1, col, "u" ,path)
#             # down
#             if row<len(grid)-1 and grid[row+1][col]==1:
#                 dfs(grid, row+1, col, "d", path)
#             # left
#             if col and grid[row][col-1]==1:
#                 dfs(grid, row, col-1, "l", path)
#             # right
#             if col<len(grid[0])-1 and grid[row][col+1]==1:
#                 dfs(grid, row, col+1, "r", path)
#             path.append("e")
        
#         islands = set()
#         for i in range(len(grid)):
#             for j in range(len(grid[0])):
#                 if grid[i][j]==1:
#                     path = []
#                     dfs(grid, i, j, "s", path)
#                     #print(path)
#                     islands.add("".join(path))
#         #print(islands)
#         return len(islands)
    
        if not grid or not grid[0]:
            return 0
        
        shape = set()
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1:
                    path = []
                    self.dfs(grid, i, j, 's', path)
                    shape.add(''.join(path))
        return len(shape)
        
        
    def dfs(self, grid, x, y, cur, path):
        path.append(cur)
        grid[x][y] = 0
        if x > 0 and grid[x-1][y] == 1:
            self.dfs(grid, x-1, y, 'u', path)
        if x < len(grid)-1 and grid[x+1][y] == 1:
            self.dfs(grid, x+1, y, 'd', path)
        if y > 0 and grid[x][y-1] == 1:
            self.dfs(grid, x, y-1, 'l', path)
        if y < len(grid[0])-1 and grid[x][y+1] == 1:
            self.dfs(grid, x, y+1, 'r', path)
        path.append('e')
```
