## Question
Given the coordinates of four points in 2D space, return whether the four points could construct a square.

The coordinate (x,y) of a point is represented by an integer array with two integers.

**Example :**   
<pre>
Input: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]
Output: True
</pre>

Note:

All the input integers are in the range [-10000, 10000].
A valid square has four equal sides with positive length and four equal angles (90-degree angles).
Input points have no order.

## Thinking


## Coding
Time: O(1); <br>
Space: O(1)
```python3
class Solution:
    def validSquare(self, p1, p2, p3, p4):
        """
        :type p1: List[int]
        :type p2: List[int]
        :type p3: List[int]
        :type p4: List[int]
        :rtype: bool
        """
        p1,p2,p3,p4 = sorted([p1,p2,p3,p4]) # Sorted with x coordinate
        print(p1,p2,p3,p4)
        return self.distance(p1,p2)!=0 and self.distance(p1,p2) == self.distance(p2,p4) and self.distance(p2,p4) == self.distance(p4,p3) and self.distance(p4,p3) == self.distance(p3,p1) and self.distance(p2,p3) == self.distance(p1,p4)
        
    def distance(self, a, b):
        return (a[0]-b[0])**2+(a[1]-b[1])**2
```

