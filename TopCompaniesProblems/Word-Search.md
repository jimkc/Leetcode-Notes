## Question
Given a 2D board and a word, find if the word exists in the grid.<br>

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.


**Example :**   
<pre>
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
</pre>

## Thinking

## Coding
Time: O(n^2);<br> 
Space: O(n)
```python3
class Solution:
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        def dfs(board, i, j, word):
            if len(word) == 0: # all the characters are checked
                return True
            if i<0 or i>=len(board) or j<0 or j>=len(board[0]) or word[0]!=board[i][j]:
                return False
            
            tmp = board[i][j]  # first character is found, check the remaining part
            board[i][j] = "#"  # avoid visit agian 
            
            # check whether can find "word" along one direction
            res = dfs(board, i+1, j, word[1:]) or dfs(board, i-1, j, word[1:]) or dfs(board, i, j+1, word[1:]) or dfs(board, i, j-1, word[1:])
            
            board[i][j] = tmp
            return res
        
        for i in range(len(board)):
            for j in range(len(board[0])):
                if dfs(board,i,j,word):
                    return True
        return False
```

