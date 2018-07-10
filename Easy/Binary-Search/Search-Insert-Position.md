## Question
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example :**
<pre>
Input: [1,3,5,6], 5
Output: 2

Input: [1,3,5,6], 2
Output: 1

Input: [1,3,5,6], 7
Output: 4

Input: [1,3,5,6], 0
Output: 0
</pre>

## Thinking
Be aware which pointer is the target.
## Coding
Time: O(logn); Binary Search. </br>
Space: O(1) 
```python3
class Solution:
    def searchInsert(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        left = 0
        right = len(nums)-1
        middle = int((right+left)/2)
        while left <= right:
            if nums[middle] > target:
                right = middle-1
            elif nums[middle] < target:
                left = middle+1
            else:
                return middle
            middle = int((right+left)/2)
    
        if target >= nums[-1]:
            return len(nums)
        else:
            return left
```

