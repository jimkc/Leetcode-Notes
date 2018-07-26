## Question
According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."<br>

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):<br>

Any live cell with fewer than two live neighbors dies, as if caused by under-population.<br>
Any live cell with two or three live neighbors lives on to the next generation.<br>
Any live cell with more than three live neighbors dies, as if by over-population..<br>
Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.<br>
Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

**Example :**   
<pre>
Input: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
</pre>

Follow up:<br>

Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.<br>
In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?<br>

## Thinking


## Coding
Time: O(n);<br>
Space: O(1)
```python3
class Solution:
    def gameOfLife(self, board):
        """
        :type board: List[List[int]]
        :rtype: void Do not return anything, modify board in-place instead.
        """
        
        r = len(board)
        c = len(board[0])
        
        for i in range(r):
            for j in range(c):
                cnt = self.Count(board,i,j,r,c)
                board[i][j] = self.Check(cnt, board[i][j])
                
        for i in range(r):
            for j in range(c):
                if board[i][j] == 2: board[i][j] = 0
                if board[i][j] == 3: board[i][j] = 1
                    
    # Count the numbers of lived cells around
    def Count(self, board, i, j, r, c):
        cnt = 0
        if i-1 >= 0 :
            if board[i-1][j] in (1,2):  cnt+=1
            if j-1 >= 0 and board[i-1][j-1] in (1,2): cnt+=1
            if j+1 < c  and board[i-1][j+1] in (1,2): cnt+=1
        if i+1 < r:
            if board[i+1][j] in (1,2): cnt+=1
            if j-1 >= 0 and board[i+1][j-1] in (1,2): cnt+=1
            if j+1 < c and board[i+1][j+1] in (1,2): cnt+=1
        
        if j-1 >= 0 and board[i][j-1] in (1,2): cnt+=1
        if j+1 < c and board[i][j+1] in (1,2): cnt+=1
        return cnt

    # 2 means original 1 and needs to change to 0
    # 3 means original 0 and needs to change to 1
    def Check(self,cnt,val):
        if val == 1:
            if cnt < 2: return 2 
            elif cnt <= 3: return 1
            else : return 2
        if val == 0:
            if cnt == 3: return 3
            else: return 0
```

