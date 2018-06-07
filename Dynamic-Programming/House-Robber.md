## Question
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.<br>

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.<br>

**Example 1:**
<pre>
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
</pre>

**Example 2:**
<pre>
Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
</pre>


Note:

Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.

## Thinking
Every step i seeks to find the largest amount of money before i-2 steps.

## Coding
Time: O(n); Go through the array once. </br>
Space: O(n) 
```python3
class Solution:
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 0
        elif len(nums) == 1:
            return nums[0]
        elif len(nums) == 2:
            return max(nums[1], nums[0])
            
        dp = [0]*len(nums)
        dp[0], dp[1] = nums[0], max(nums[1], nums[0])
        dp[2] = nums[2] + dp[0]
        
        for i in range(3,len(nums)):
            dp[i] = nums[i] + max(dp[:i-1])
        return max(dp[-1], dp[-2])
```

