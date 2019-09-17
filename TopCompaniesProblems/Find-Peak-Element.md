## Question
A peak element is an element that is greater than its neighbors.<br>

Given an input array nums, where nums[i] ≠ nums[i+1], find a peak element and return its index.<br>

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.<br>

You may imagine that nums[-1] = nums[n] = -∞.

**Example :**   
<pre>
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.

Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
             
</pre>

Note:<br>

Your solution should be in logarithmic complexity.

## Thinking


## Coding
Time: O(nlogn); <br>
Space: O(1)
```python3
class Solution(object):
    def findPeakElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        low=0
        high=len(nums)-1
        while(low<=high):
            mid=(low+high)//2
            # mid = 0 or len(nums)-1 for the boundary case
            if (mid == 0 or nums[mid-1]<nums[mid])  and (mid == len(nums)-1 or nums[mid]>nums[mid+1]):
                return mid
            elif nums[mid-1]>nums[mid]: #  '\' this case , the question says we may imagine that nums[-1] = nums[n] = -∞.
                high=mid-1
            elif nums[mid]<nums[mid+1]: #  '/' this case
                low=mid+1
        return 0 if nums[0]>nums[1] else 1  
```

