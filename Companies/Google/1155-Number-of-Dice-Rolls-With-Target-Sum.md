## Question
You have d dice, and each die has f faces numbered 1, 2, ..., f.<br>
<br>
Return the number of possible ways (out of fd total ways) modulo 10^9 + 7 to roll the dice so the sum of the face up numbers equals target. </br>

**Example :**   
<pre>
Input: d = 1, f = 6, target = 3
Output: 1
Explanation: 
You throw one die with 6 faces.  There is only one way to get a sum of 3.

Input: d = 2, f = 6, target = 7
Output: 6
Explanation: 
You throw two dice, each with 6 faces.  There are 6 ways to get a sum of 7:
1+6, 2+5, 3+4, 4+3, 5+2, 6+1.

Input: d = 2, f = 5, target = 10
Output: 1
Explanation: 
You throw two dice, each with 5 faces.  There is only one way to get a sum of 10: 5+5.

Input: d = 1, f = 2, target = 3
Output: 0
Explanation: 
You throw one die with 2 faces.  There is no way to get a sum of 3.

Input: d = 30, f = 30, target = 500
Output: 222616187
Explanation: 
The answer must be returned modulo 10^9 + 7.
</pre>

## Thinking
memorize the previous combinations with dp -> save time

## Coding
Time: O(d*t*f); </br>
Space: O(d*t)
```python
class Solution:
    def numRollsToTarget(self, d: int, f: int, target: int) -> int:
        mod = 10 ** 9 + 7
        # dp[dice counts][target value]
        dp = [[0 for j in range(target + 1)] for i in range(d + 1)]
        dp[0][0] = 1
        for i in range(d):
            for val in range(target):
                for face in range(1, f + 1):
                    if val + face <= target:
                        dp[i + 1][val + face] += dp[i][val]
                        dp[i + 1][val + face] %= mod
        return dp[d][target]
```

