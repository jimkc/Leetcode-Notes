## Question
Given a 01 matrix M, find the longest line of consecutive one in the matrix. The line could be horizontal, vertical, diagonal or anti-diagonal.<br>

**Example :**   
<pre>
Input:
[[0,1,1,0],
 [0,1,1,0],
 [0,0,0,1]]
Output: 3
</pre>

## Thinking
Use dp to count from left upper to right lower.<br>
For each element we keep 4 counters -<br>
[consec ones in that row Left to Right till that elem]<br>
[consec ones in that col Top to Bottom till that elem]<br>
[consec ones in that anti-diag TopRight to LeftBottom till that elem]<br>
[consec ones in that diag TopLeft to BottomRight till that elem]<br>
and iterate from 1st element to last element in the matrix once.

## Coding
Time: O(nm); </br>
Space: O(nm)
```python
class Solution:
    def longestLine(self, M: List[List[int]]) -> int:
        
        if len(M)==0 or len(M[0]) == 0: 
            return 0
        
        n, m = len(M), len(M[0])
        
        dp = {}
        
        for i in range(n):
            for j in range(m):
                if M[i][j] == 0 :
                    for direction in range(4):
                        dp[(i,j,direction)] = 0
                    continue
                    
                # beacuse it's 1 so initialize dp value as 1
                for direction in range(4):
                    dp[(i,j,direction)] = 1
                    
                # left
                if j > 0:
                    dp[(i,j,0)] += dp[(i,j-1,0)]
                # left upper
                if i>0 and j>0:
                    dp[(i,j,1)] += dp[(i-1,j-1,1)]
                # upper
                if i>0:
                    dp[(i,j,2)] += dp[(i-1,j,2)]
                # right upper
                if i>0 and j <m-1:
                    dp[(i,j,3)] += dp[(i-1,j+1,3)]
                
        return max(dp.values()) if dp else 0
                
```

