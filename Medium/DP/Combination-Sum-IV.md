## Question
Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

**Example :**   
<pre>
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
</pre>

Follow up:<br>
What if negative numbers are allowed in the given array?<br>
How does it change the problem?<br>
What limitation we need to add to the question to allow negative numbers?

## Thinking


## Coding
Time: O(target*n); <br>
Space: O(target)
```python3
class Solution:
    def combinationSum4(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        dp = [0]*(target+1)
        for n in nums:
            if n <= target:
                dp[n] += 1 
        
        for i in range(1,len(dp)):
            if dp[i] != 0:    # if it's 0 (combinations) then we cant use it as dp to combine other nums to a new num
                for n in nums:
                    if i+n <= target:
                        dp[i+n] += dp[i]
            
        return dp[-1]
```
