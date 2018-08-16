## Question
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.<br>

Your algorithm's runtime complexity must be in the order of O(log n).<br>

If the target is not found in the array, return [-1, -1].

**Example :**   
<pre>
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
</pre>

## Thinking


## Coding
Time: O(logn); 
Space: O(1)
```python3
class Solution:
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if not nums:
            return [-1,-1]
        
        idx = 0 
        idxr = -1 # Rightest index
        idxl = -1 # Leftest index
        
        l, r = 0, len(nums)-1
        # Find random one of the target
        while l <= r:
            mid = (l+r)//2
            
            if  nums[mid] == target:
                idx = mid
                break
            elif nums[mid] > target:
                r = mid - 1
            else:
                l = mid + 1
            

        l1, r1 = idx, len(nums)-1
        # Find the rightest target in right half 
        while l1 <= r1: 
            mid = (l1+r1)//2
            
            if nums[mid] == target:
                if mid == len(nums)-1 or nums[mid+1] != target: 
                    idxr = mid
                    break
                else:
                    l1 = mid+1
            else:
                r1 = mid-1
        
        
        l2, r2 = 0, idx
        # Find the leftest target in left half 
        while l2 <= r2:
            mid = (l2+r2)//2
            
            if nums[mid] == target:
                if  mid == 0 or nums[mid-1] != target:
                    idxl = mid
                    break
                else:
                    r2 = mid-1
            else:
                l2 = mid+1
        
        return [idxl,idxr]
```
