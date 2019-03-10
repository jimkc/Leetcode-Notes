## Question
Given a set of points in the xy-plane, determine the minimum area of a rectangle formed from these points, with sides parallel to the x and y axes.<br>

If there isn't any rectangle, return 0.

**Example :**   
<pre>
Input: [[1,1],[1,3],[3,1],[3,3],[2,2]]
Output: 4

Input: [[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
Output: 2
</pre>

## Thinking


## Coding
Time: O(n^2); </br>
Space: O(1)
```python
class Solution:
    def minAreaRect(self, points: List[List[int]]) -> int:
        
        all_points = set()
        min_area = 0
        
        for x, y in points:
            all_points.add((x,y))
        
        # use diagonal points of the rectangle and check if other points exists
        for i in range(len(points)-1):
            for j in range(i+1,len(points)):
                x1, y1 = points[i]
                x3, y3 = points[j]
                
                # make sure two diagonal points is not in the same row or column
                if x1 != x3 and y1 != y3:
                    x2, y2 = x1, y3 
                    x4, y4 = x3, y1

                    if (x2, y2) in all_points and (x4,y4) in all_points:
                        rec_area = abs(x1-x3)*abs(y1-y3)
                        if min_area == 0:
                            min_area = rec_area
                        elif rec_area < min_area :
                            min_area = rec_area
        return min_area
```

