## Question
There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up, down, left or right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.<br>

Given the ball's start position, the destination and the maze, determine whether the ball could stop at the destination.<br>

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.</br>

**Example :**   
<pre>
Pic: https://leetcode.com/problems/the-maze/description/

Input 1: a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

Input 2: start coordinate (rowStart, colStart) = (0, 4)
Input 3: destination coordinate (rowDest, colDest) = (4, 4)

Output: true

Explanation: One possible way is : left -> down -> left -> down -> right -> down -> right.

Input 1: a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

Input 2: start coordinate (rowStart, colStart) = (0, 4)
Input 3: destination coordinate (rowDest, colDest) = (3, 2)

Output: false

Explanation: There is no way for the ball to stop at the destination.
</pre>

Note:<br>
<br>
There is only one ball and one destination in the maze.<br>
Both the ball and the destination exist on an empty space, and they will not be at the same position initially.<br>
The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.<br>
The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.

## Thinking


## Coding
Time: O(mn); maze size</br>
Space: O(mn) visited 
```python
from collections import deque

class Solution(object):
    def hasPath(self, matrix, start, end):
        dirs = [(0, 1), (0, -1), (-1, 0), (1, 0)]  # up down left right
        visited = [[False] * len(matrix[0]) for _ in range(len(matrix))]
        visited[start[0]][start[1]] = True

        q = deque([start])

        while q:
            tup = q.popleft()
            if tup[0] == end[0] and tup[1] == end[1]:
                return True

            for dir in dirs:
                # Roll the ball until it hits a wall
                row = tup[0] + dir[0]
                col = tup[1] + dir[1]

                while 0 <= row < len(matrix) and 0 <= col < len(matrix[0]) and matrix[row][col] == 0:
                    row += dir[0]
                    col += dir[1]

                # x and y locates @ a wall when exiting the above while loop, so we need to backtrack 1 position
                (new_x, new_y) = (row - dir[0], col - dir[1])

                # Check if the new starting position has been visited
                if not visited[new_x][new_y]:
                    q.append((new_x, new_y))
                    visited[new_x][new_y] = True
        return False

```

