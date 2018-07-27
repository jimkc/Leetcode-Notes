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
```python3
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
```python3
class Solution:
    def countCornerRectangles(self, grid):
        ret = 0
        prevs = [] # Record each idx that is one for each previous rows
        for row in grid:
            ones = {idx for idx, val in enumerate(row) if val}
            for prev in prevs:
                matches = len(ones & prev)
                ret += matches * (matches - 1) // 2 # Use math calculation to count the matches idx with one in previous
            prevs.append(ones)
        
        return ret
```
