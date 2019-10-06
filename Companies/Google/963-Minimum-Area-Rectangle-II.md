## Question
Given a set of points in the xy-plane, determine the minimum area of any rectangle formed from these points, with sides not necessarily parallel to the x and y axes.<br>

If there isn't any rectangle, return 0. </br>

**Example :**   
<pre>
Pic: https://leetcode.com/problems/minimum-area-rectangle-ii/description/

Input: [[1,2],[2,1],[1,0],[0,1]]
Output: 2.00000
Explanation: The minimum area rectangle occurs at [1,2],[2,1],[1,0],[0,1], with an area of 2.

Input: [[0,1],[2,1],[1,1],[1,0],[2,0]]
Output: 1.00000
Explanation: The minimum area rectangle occurs at [1,0],[1,1],[2,1],[2,0], with an area of 1.
</pre>

## Thinking
If the pair of dots have the same center and radius, then they can form  a rectangle.

## Coding
Time: O(nlogn); </br>
Space: O(1)
```python
class Solution:
    def minAreaFreeRect(self, points: List[List[int]]) -> float:
        
        # categorize according to center and radius of all the possible 2 point combination
        seen = collections.defaultdict(list)
        min_area = float("inf")
        
        
        for i in range(len(points)-1):
            for j in range(i+1, len(points)):
                p1, p2 = points[i], points[j]
                center = self.getCenter(p1, p2)
                radius = self.getDistance(center, p1)
                seen[(center, radius)].append((p1,p2))
                
        # count the rectangle sizes
        for (c, r), point_pairs in seen.items():
            for i in range(len(point_pairs)-1):
                for j in range(i+1,len(point_pairs)):
                    p1, p2 = point_pairs[i]
                    p3, p4 = point_pairs[j]
                    area = abs(self.getDistance(p1,p3))*abs(self.getDistance(p1,p4))
                    min_area = min(area, min_area)
                    
        return min_area if min_area < float("inf") else 0
    
    def getCenter(self, p1, p2):
        return (p1[0]+p2[0])/2, (p1[1]+p2[1])/2
    
    def getDistance(self, p1, p2):
        return ((p1[0]-p2[0])*(p1[0]-p2[0])+(p1[1]-p2[1])*(p1[1]-p2[1]))**(1/2)
        
```

