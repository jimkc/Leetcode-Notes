## Question
You are given a m x n 2D grid initialized with these three possible values.<br>

-1 - A wall or an obstacle.<br>
0 - A gate.<br>
INF - Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.<br>
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

**Example :**   
<pre>
Given the 2D grid:

INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
After running your function, the 2D grid should be:

  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
</pre>

## Thinking


## Coding
Time: O(mn); <br>
Space: O(1)
```python3
class Solution:
    def wallsAndGates(self, rooms):
        """
        :type rooms: List[List[int]]
        :rtype: void Do not return anything, modify rooms in-place instead.
        """
        if not rooms:
            return 
        
        length = len(rooms)
        width = len(rooms[0])
        
        for i in range(length):
            for j in range(width):
                if rooms[i][j] == 0:
                    queue = collections.deque([(i+1,j,1),(i-1,j,1),(i,j+1,1),(i,j-1,1)])
                    
                    while queue:
                        x,y,distance = queue.popleft()
                        if x < 0 or x > length-1 or y < 0 or y > width-1 or rooms[x][y] < distance:
                            continue
                        
                        rooms[x][y] = distance
                        queue.extend([(x+1,y,distance+1),(x-1,y,distance+1),(x,y+1,distance+1),(x,y-1,distance+1)])
        
```

