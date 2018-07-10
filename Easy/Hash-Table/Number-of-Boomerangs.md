## Question
Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points (i, j, k) such that the distance between i and j equals the distance between i and k (the order of the tuple matters).<br>

Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range [-10000, 10000] (inclusive).<br>

**Example :**
<pre>
Input:
[[0,0],[1,0],[2,0]]

Output:
2

Explanation:
The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]
</pre>


## Thinking
Boomerang means to select three points every time.<br>
Imagine there are 3 points (p2,p3,p4) at a distance 10 from point p1. How many boomerangs do these contribute? For p2, p3, we have (p1,p2,p3) and (p1,p3,p2). Similarly for p3,p4 and p1,p4 - so a total of 6 boomerangs. Let us generalize it.<br>
Imagine we have k points which are at a distance d1 from point p. How many ways can we choose two items from k items? Answer: (k * (k-1)/2 ). Now each choice yield 2 boomerangs (i.e. (p2, p3) and (p3,p2)). So we have k * (k-1).<br>

## Coding
Time: O(n^2);. </br>
Space: O(n).
```python3
class Solution:
    def numberOfBoomerangs(self, points):
        """
        :type points: List[List[int]]
        :rtype: int
        """
        
        cnt = 0
        for i in range(len(points)):
            distance = {}
            for j in range(len(points)):
                if i != j:
                    x, y = points[i][0]-points[j][0], points[i][1]-points[j][1]
                    d = x*x + y*y
                    
                    if d not in distance:
                        distance[d] = 1
                    else:
                        distance[d] += 1
                        
            for k,v in distance.items():
                cnt += distance[k]*(distance[k]-1)
        return int(cnt)
```

