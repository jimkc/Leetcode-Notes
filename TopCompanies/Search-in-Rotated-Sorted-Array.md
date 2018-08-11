## Question
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.<br>

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).<br>

You are given a target value to search. If found in the array return its index, otherwise return -1.<br>

You may assume no duplicate exists in the array.<br>

Your algorithm's runtime complexity must be in the order of O(log n).

**Example :**   
<pre>
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
</pre>

## Thinking


## Coding
Time: O(logn); <br>
Space: O(1)
```python3
class Solution:
            def arrayPairSum(self, nums):
                #:type nums: List[int]
                #:rtype: int

                nums.sort()
                max_sum_pair = 0
                for i in range(0,len(nums),2):
                    max_sum_pair += nums[i] 
                return max_sum_pairclass Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        
        low = 0
        high = len(nums)-1
        while low <= high:
            mid = (low+high)//2
            
            if nums[mid] == target:
                return mid
            
            if nums[low] <= nums[mid]: 
                if nums[low] <= target <= nums[mid]:
                    high = mid - 1
                else:
                    low = mid + 1
            else:
                if nums[mid] <= target <= nums[high]:
                    low = mid + 1
                else:
                    high = mid - 1
            
        return -1
```

