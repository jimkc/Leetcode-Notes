## Question
A rectangle is represented as a list [x1, y1, x2, y2], where (x1, y1) are the coordinates of its bottom-left corner, and (x2, y2) are the coordinates of its top-right corner.

Two rectangles overlap if the area of their intersection is positive.  To be clear, two rectangles that only touch at the corner or edges do not overlap.

Given two (axis-aligned) rectangles, return whether they overlap.

**Example :**   
<pre>
Input: rec1 = [0,0,2,2], rec2 = [1,1,3,3]
Output: true

Input: rec1 = [0,0,1,1], rec2 = [1,0,2,1]
Output: false
</pre>

Notes:<br>

Both rectangles rec1 and rec2 are lists of 4 integers.<br>
All coordinates in rectangles will be between -10^9 and 10^9.<br>

## Thinking


## Coding
Time: O(1);<br>
Space: O(1)
```python3
class Solution:
    def isRectangleOverlap(self, rec1, rec2):
        """
        :type rec1: List[int]
        :type rec2: List[int]
        :rtype: bool
        """
        
        x1, y1, x2, y2 = rec1[0], rec1[1], rec1[2], rec1[3]
        x3, y3, x4, y4 = rec2[0], rec2[1], rec2[2], rec2[3]
        
        w = min(x2,x4) - max(x1,x3) # only when overlap, w > 0
        h = min(y2,y4) - max(y1,y3) 
        
        if w <=0 or h <=0:
            return False
        else:
            return True
```

