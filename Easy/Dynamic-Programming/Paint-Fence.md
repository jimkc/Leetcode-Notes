## Question
There is a fence with n posts, each post can be painted with one of the k colors.<br>

You have to paint all the posts such that no more than two adjacent fence posts have the same color.<br>

Return the total number of ways you can paint the fence.<br>

**Example :**
<pre>
Input: n = 3, k = 2
Output: 6
Explanation: Take c1 as color 1, c2 as color 2. All possible ways are:

            post1  post2  post3      
 -----      -----  -----  -----       
   1         c1     c1     c2 
   2         c1     c2     c1 
   3         c1     c2     c2 
   4         c2     c1     c1  
   5         c2     c1     c2
   6         c2     c2     c1
</pre>

Note:
n and k are non-negative integers.

## Thinking
Dp problem doesn't always have to have a dp for memorization, sometimes it can be only few
variables. The key is to decompose the problem into small problems and suitable for doing it iteratively.

## Coding
Time: O(n); Go through the array once. </br>
Space: O(1) 
```python3
class Solution:
    def numWays(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: int
        """
        if n == 0:
            return 0
        elif n == 1:
            return k
        elif n == 2:
            return k + k*(k-1)
        
        same, diff = k, k*(k-1) # Situation while k = 2
        
        for i in range(2,n):
            same, diff = diff*1, (same+diff)*(k-1)
        return same+diff

```

