## Question
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:<br>

Each row must contain the digits 1-9 without repetition.<br>
Each column must contain the digits 1-9 without repetition.<br>
Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

**Example :**   
<pre>
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true


Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being 
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.    
</pre>


Note:<br>

A Sudoku board (partially filled) could be valid but is not necessarily solvable.<br>
Only the filled cells need to be validated according to the mentioned rules.<br>
The given board contain only digits 1-9 and the character '.'.<br>
The given board size is always 9x9.

## Thinking


## Coding
Time: O(n);n for board size <br>
Space: O(n)
```python3
class Solution:
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        x = [set() for _ in range(9)] # Dont use [set()]*9, all indices will held the same set
        y = [set() for _ in range(9)]
        square = [[set() for _ in range(3)] for _ in range(3)]
        
        for i in range(len(board)):
            for j in range(len(board[0])):
                n = board[i][j]
                
                if n == '.':
                    continue
                
                if n not in x[i]:
                    x[i].add(n)
                else:
                    return False
                
                if n not in y[j]:
                    y[j].add(n)
                else:
                    return False
                
                if n not in square[i//3][j//3]:
                    square[i//3][j//3].add(n)
                else:
                    return False
        return True
```

