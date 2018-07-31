## Question
Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.<br>

Note:<br>
The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.

**Example :**   
<pre>
Input: nums = [1, -1, 5, -2, 3], k = 3
Output: 4 
Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.

Input: nums = [-2, -1, 2, 1], k = 1
Output: 2 
Explanation: The subarray [-1, 2] sums to 1 and is the longest.
</pre>

Follow Up:<br>
Can you do it in O(n) time?

## Thinking


## Coding
Time: O(n);<br>
Space: O(n)
```python3
class Solution:
    def maxSubArrayLen(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        
        ans, acc = 0,0 # Answer length and accumulative value of nums
        map_idx = {0:-1} # 0 is a special case, where the begining accumulative value is zero and will always be longer than other 0
        for i,n in enumerate(nums):
            acc += n
            if acc not in map_idx: # We don't update if we met the same value, because the smaller idx gets the longer length
                map_idx[acc] = i
            if acc-k in map_idx:
                ans = max(ans, i-map_idx[acc-k])
        return ans
```
