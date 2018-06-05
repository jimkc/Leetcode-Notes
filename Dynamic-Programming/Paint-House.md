## Question
There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.<br>

The cost of painting each house with a certain color is represented by a n x 3 cost matrix. For example, costs[0][0] is the cost of painting house 0 with color red; costs[1][2] is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.<br>

**Example :**
<pre>
Input: [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue. 
             Minimum cost: 2 + 5 + 3 = 10.
</pre>

Note:
All costs are positive integers.

## Thinking


## Coding
Time: O(n); Go through the array once. </br>
Space: O(1) 
```python3
class Solution:
    def minCost(self, costs):
        """
        :type costs: List[List[int]]
        :rtype: int
        """
        if not costs:
            return 0
        dp = costs[0] # Initial dp 
        for i in range(1,len(costs)):
            dp = [min(dp[1] + costs[i][0], dp[2] + costs[i][0]), min(dp[0] + costs[i][1], dp[2] + costs[i][1] ), min(dp[0] +                      costs[i][2], dp[1] + costs[i][2])]
        return min(dp)
        
        
```

