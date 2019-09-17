## Question
Find the total area covered by two rectilinear rectangles in a 2D plane.<br>

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.


**Example :**   
<pre>
Input: A = -3, B = 0, C = 3, D = 4, E = 0, F = -1, G = 9, H = 2
Output: 45
</pre>

Note:<br>

Assume that the total area is never beyond the maximum possible value of int.

## Thinking


## Coding
Time: O(); <br>
Space: O()
```python3
class Solution:
    def computeArea(self, A, B, C, D, E, F, G, H):
        """
        :type A: int
        :type B: int
        :type C: int
        :type D: int
        :type E: int
        :type F: int
        :type G: int
        :type H: int
        :rtype: int
        """
        area1 = abs(C-A)*abs(B-D)
        area2 = abs(E-G)*abs(F-H)
        w = min(C,G)-max(A,E)
        h = min(D, H)-max(B,F)
        if w<=0 or h<=0:
            return area1 + area2
        else:
            return area1 + area2 - w*h
```

