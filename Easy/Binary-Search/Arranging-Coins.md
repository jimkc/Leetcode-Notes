## Question
You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.</br>

Given n, find the total number of full staircase rows that can be formed.</br>

n is a non-negative integer and fits within the range of a 32-bit signed integer.</br>
**Example 1:**
<pre>
n = 5

The coins can form the following rows:
¤
¤ ¤
¤ ¤

Because the 3rd row is incomplete, we return 2.
</pre>

**Example 2:**
<pre>
n = 8

The coins can form the following rows:
¤
¤ ¤
¤ ¤ ¤
¤ ¤

Because the 4th row is incomplete, we return 3.
</pre>

## Thinking
Find what the target value should be. The target value should be between tmp1 and tmp2.</br>
## Coding
Time: O(logn); Binary search. </br>
Space: O(1) 
```python3
class Solution:
    def arrangeCoins(self, n):
        """
        :type n: int
        :rtype: int
        """
        r = n
        l = 1
        
        while l <= r :
            mid = int((r+l)/2)
            tmp1 = (mid + 1)*mid/2
            tmp2 = (mid + 1 + 1) *(mid + 1)/2
            if tmp1 <= n and tmp2 > n:
                return mid
            elif tmp1 < n :
                l = mid + 1
            else:
                r = mid -1
        return 0
```

