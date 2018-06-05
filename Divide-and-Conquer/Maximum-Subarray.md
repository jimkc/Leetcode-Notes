## Question
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.<br>

**Example :**
<pre>
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
</pre>


## Thinking
The key of this question is to see it as a dynamic programming problem. Every step we calculate a little step 
and save the optimal choice of the little step.<br>

## Coding
Time: O(n); Go through the array once. </br>
Space: O(n) 
```python3
class Solution:
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        
        for i in range(1,len(nums)):
            nums[i] = max(nums[i-1] + nums[i], nums[i])
        return max(nums)
```

