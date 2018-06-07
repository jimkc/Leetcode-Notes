## Question
On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 indexed).<br>

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.<br>

**Example 1:**
<pre>
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
</pre>

**Example 2:**
<pre>
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
</pre>


Note:
**1.** cost will have a length in the range [2, 1000].
**2.** Every cost[i] will be an integer in the range [0, 999].

## Thinking
DP is usually the lenth of the choices in order to remember the best case in every choice. 
During the phase of selection, the options are the steps we can do at most.
## Coding
Time: O(n); Go through the array once. </br>
Space: O(n) 
```python3
class Solution:
    def minCostClimbingStairs(self, cost):
        """
        :type cost: List[int]
        :rtype: int
        """
        
        dp = [0]*len(cost)
        dp[0],dp[1] = cost[0], cost[1]
        for i in range(2,len(cost)):
            dp[i] = min(cost[i] + dp[i-1], cost[i] + dp[i-2])
        return min(dp[-1],dp[-2])
```

