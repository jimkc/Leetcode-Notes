## Question
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

**Example :**   
<pre>
For example, given the following triangle

[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).
</pre>

Note:<br>

Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

## Thinking
Dp. Always choose the smaller sum from previos path.

## Coding
Time: O();<br>
Space: O()
```python3
class Solution:
    def minimumTotal(self, triangle):
        """
        :type triangle: List[List[int]]
        :rtype: int
        """
        n = len(triangle)
        for i in range(1 ,n):
            m = len(triangle[i])
            triangle[i][0] +=triangle[i-1][0]
            triangle[i][m-1] += triangle[i-1][m-2] # i-1th triangle is 1 smaller than ith triangle
            
            for j in range(1,m-1):
                triangle[i][j] += min(triangle[i-1][j-1], triangle[i-1][j])
            
        return min(triangle[-1])
```
