## Question
You are climbing a stair case. It takes n steps to reach to the top.<br>

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?<br>

**Example 1:**
<pre>
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
</pre>

**Example 2:**
<pre>
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
</pre>

Note: Given n will be a positive integer.

## Thinking
Every i step consists of the posibility of i-1 step and i-2 steps, because they have only 
one possibility to come to this step from that step. As a result, dp[i] = dp[i-1] + dp[i-2].

## Coding
Time: O(n); Go through the array once. </br>
Space: O(n) 
```python3
class Solution:
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n < 3:
            return n
        dp = [0] * (n+1)
        dp[0], dp[1], dp[2] = 0, 1, 2
        for i in range(3, n+1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[-1]
```

