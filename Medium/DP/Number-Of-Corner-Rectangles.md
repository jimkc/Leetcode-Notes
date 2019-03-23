## Question
Given a grid where each entry is only 0 or 1, find the number of corner rectangles.<br>

A corner rectangle is 4 distinct 1s on the grid that form an axis-aligned rectangle. Note that only the corners need to have the value 1. Also, all four 1s used must be distinct.<br>

**Example :**   
<pre>
Input: grid = 
[[1, 0, 0, 1, 0],
 [0, 0, 1, 0, 1],
 [0, 0, 0, 1, 0],
 [1, 0, 1, 0, 1]]
Output: 1
Explanation: There is only one corner rectangle, with corners grid[1][2], grid[1][4], grid[3][2], grid[3][4].


Input: grid = 
[[1, 1, 1],
 [1, 1, 1],
 [1, 1, 1]]
Output: 9
Explanation: There are four 2x2 rectangles, four 2x3 and 3x2 rectangles, and one 3x3 rectangle.
 
 
Input: grid = 
[[1, 1, 1, 1]]
Output: 0
Explanation: Rectangles must have four distinct corners.
</pre>


Note:<br>

The number of rows and columns of grid will each be in the range [1, 200].<br>
Each grid[i][j] will be either 0 or 1.<br>
The number of 1s in the grid will be at most 6000.

## Thinking


## Coding
Time: O(m*n); <br>
Space: O(m*n^2)
```python
class Solution:
    def countCornerRectangles(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        end_edges = {}
        cnt = 0
        
        for row in grid:
            for l in range(len(row)):
                for r in range(l+1, len(row)):
                    if row[l] and row[r]: # Both are 1
                        if (l,r) not in end_edges:
                            end_edges[(l,r)] = 1
                        else:
                            cnt += end_edges[(l,r)] # if n edges exists before we will add n rectangels 
                            end_edges[(l,r)] += 1
        return cnt
```

Faster one 
```python
class Solution:
    def countCornerRectangles(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        ans = 0
        dp_sets = [] # every row stores set of columns
        
        for row in range(len(grid)):
            dp_sets.append(set([col for col,val in enumerate(grid[row]) if val==1]))
            for prev_row in range(row):
                matches = self.match_two_rows(dp_sets[row], dp_sets[prev_row])
                # match actually can use python built in function &
                # matches = len(dp_sets[row] & dp_sets[prev_rows])
                if matches > 0:
                    ans += matches*(matches-1)//2 # choose 2 among n items
        return ans
    
    def match_two_rows(self,row1, row2):
        match = 0
        for col in row1:
            if col in row2:
                match +=1
        return match            
    
        
        
                    
```
