## Question
You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.<br>

Find out how many ways to assign symbols to make sum of integers equal to target S.

**Example :**   
<pre>
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
</pre>

Note:<br>
The length of the given array is positive and will not exceed 20.<br>
The sum of elements in the given array will not exceed 1000.<br>
Your output answer is guaranteed to be fitted in a 32-bit integer.
## Thinking

## Coding
Time: O();<br>
Space: O()
```python3
class Solution:
    def findTargetSumWays(self, nums, S):
        """
        :type nums: List[int]
        :type S: int
        :rtype: int
        """
        if not nums:
            return 0
        dic = {nums[0]: 1, -nums[0]: 1} if nums[0] != 0 else {0: 2}
        for i in range(1, len(nums)):
            tdic = {}
            for d in dic:
                # the original ways to sum d+nums[i] + ways to sum d (add nums[i] becomes d+nums[i]) 
                tdic[d + nums[i]] = tdic.get(d + nums[i], 0) + dic.get(d, 0) 
                tdic[d - nums[i]] = tdic.get(d - nums[i], 0) + dic.get(d, 0)
            dic = tdic
        return dic.get(S, 0)
```

