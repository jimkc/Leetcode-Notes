## Question
Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.<br>

Note: You may not slant the container and n is at least 2.<br>

pic: https://leetcode.com/problems/container-with-most-water/description/<br>
**Example :**   
<pre>
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
</pre>

## Thinking
https://leetcode.com/problems/container-with-most-water/solution/<br>
Two pointers starts from outside to inside, since the amount of water depends on the lower line, so while moving pointers toward middle only by moving the shorter one can have the chance to improve the amount of water.<br>



## Coding
Time: O(n);<br>
Space: O(1)
```python3
class Solution:
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        maxarea = 0
        
        i, j = 0, len(height)-1
        
        while i < j:
            area = min(height[i],height[j])*(j-i)
            
            if area > maxarea:
                maxarea = area
            
            # Move the lower height inside , which has the possibility to benefit the area
            if height[i] < height[j]:
                i+=1
            else:
                j-=1
        return maxarea
```
