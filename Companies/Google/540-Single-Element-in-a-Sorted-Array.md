## Question
You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.</br>

**Example :**   
<pre>
Input: [1,1,2,3,3,4,4,8,8]
Output: 2

Input: [3,3,7,7,10,11,11]
Output: 10
</pre>

Note: Your solution should run in O(log n) time and O(1) space.

## Thinking
Binary search allows us to search in O(logn). Every time we search we have two kinds of cases, whether the mid is in an even index or an odd index. Then we look at which part the single element exists.

## Coding
Time: O(logn); </br>
Space: O(1)
```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        if len(nums) < 1:
            return None
        elif len(nums) == 1:
            return nums[0]
        
        l, r = 0, len(nums)-1
        
        while l < r:
            mid = l + (r - l)//2
            
            if (mid+1) % 2 == 0 :
                if nums[mid] == nums[mid-1]:
                    l = mid+1
                else:
                    # keep right part as even nums, left as odd nums
                    r = mid -1
            else: # mid % 2 = 0
                if nums[mid] == nums[mid+1]:
                    l = mid+2
                else:
                    # keep right part as even nums, left as odd nums
                    r = mid
        
        # think from the basic cases
        return nums[l]
```

