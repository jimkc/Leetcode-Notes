## Question
An image is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).<br>

Given a coordinate (sr, sc) representing the starting pixel (row and column) of the flood fill, and a pixel value newColor, "flood fill" the image.<br>

To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.<br>

At the end, return the modified image.

**Example :**
<pre>
Input: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2

Output: [[2,2,2],[2,2,0],[2,0,1]]

Explanation: 
From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected 
by a path of the same color as the starting pixel are colored with the new color.
Note the bottom corner is not colored 2, because it is not 4-directionally connected
to the starting pixel.
</pre>

Note:<br>

The length of image and image[0] will be in the range [1, 50].<br>
The given starting pixel will satisfy 0 <= sr < image.length and 0 <= sc < image[0].length.<br>
The value of each color in image[i][j] and newColor will be an integer in [0, 65535].<br>

## Thinking


## Coding
Time: O(n);  </br>
Space: O(n).
```python3
class Solution:
    def floodFill(self, image, sr, sc, newColor):
        """
        :type image: List[List[int]]
        :type sr: int
        :type sc: int
        :type newColor: int
        :rtype: List[List[int]]
        """
        self.pic = image
        self.color = image[sr][sc]
        R = len(image)
        C = len(image[0])
        if self.color == newColor: return image # Without this line dfs wont stop
        
        def flood(sr,sc,newColor):
            if self.pic[sr][sc] == self.color:
                self.pic[sr][sc] = newColor
            
                if sr + 1 < R : flood(sr+1,sc,newColor)    
                if sc + 1 < C : flood(sr,sc+1,newColor)
                if sc - 1 >= 0 : flood(sr,sc-1,newColor)
                if sr - 1 >= 0 : flood(sr-1,sc,newColor)
            
        flood(sr,sc,newColor)
        
        return self.pic
            
```

