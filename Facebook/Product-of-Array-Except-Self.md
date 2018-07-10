## Question
Given an array nums of n integers where n > 1,  return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].<br>

**Example :**   
<pre>
Input:  [1,2,3,4]
Output: [24,12,8,6]
<pre>

Note: Please solve it without division and in O(n).<br>

Follow up:<br>
Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)

## Thinking


## Coding
Time: O(n);  </br>
Space: O(1)
```python3
class Solution:
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        p = 1 
        n = len(nums)
        output = []
        
        # Times nums[:i] for output[i]
        for i in range(0,n):
            output.append(p) # The first is set to 1 because we times it when we start from right to left
            p = p * nums[i]  # p carries the current result and times the new one
            
        p = 1
        # Times nums[i+1:] for output[i]
        for i in range(n-1,-1,-1):
            output[i] = output[i] * p
            p = p * nums[i]
        return output
```

