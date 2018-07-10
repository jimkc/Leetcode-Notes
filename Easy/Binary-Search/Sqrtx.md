## Question
Implement int sqrt(int x).</br>

Compute and return the square root of x, where x is guaranteed to be a non-negative integer.</br>

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.</br>

**Example 1:**
<pre>
Input: 4
Output: 2
</pre>

**Example 2:**
<pre>
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
</pre>

## Thinking

## Coding
Time: O(logn); Binary Search. </br>
Space: O(1) 
```python3
class Solution:
    def mySqrt(self, x):
        """
        :type x: int
        :rtype: int
        """
        
        l = 0
        r = x
        
        while l <= r:
            mid = int((l+r)/2)
            
            if mid*mid < x:
                l = mid + 1
            elif mid*mid > x:
                r = mid -1
            else:
                return mid
        return r
```

