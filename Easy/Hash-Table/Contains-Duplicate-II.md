## Question
Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.<br>

**Example :**
<pre>
Input: nums = [1,2,3,1], k = 3
Output: true

Input: nums = [1,0,1,1], k = 1
Output: true

Input: nums = [1,2,3,1,2,3], k = 2
Output: false
</pre>

Note:
You may assume that both strings contain only lowercase letters.

## Thinking


## Coding
Time: O(n); </br>
Space: O(n).
```python3
class Solution:
    def containsNearbyDuplicate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        dic = {}
        for i, v in enumerate(nums):
            if v in dic and i - dic[v] <= k: # whether there are means that we only need to find one example
                return True
            dic[v] = i
        return False
```

