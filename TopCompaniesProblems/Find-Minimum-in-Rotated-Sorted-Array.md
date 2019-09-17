## Question
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.<br>

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).<br>

Find the minimum element.<br>

You may assume no duplicate exists in the array.

**Example :**   
<pre>
Input: [3,4,5,1,2] 
Output: 1


Input: [4,5,6,7,0,1,2]
Output: 0
</pre>

## Thinking


## Coding
Time: O(logn); The sorting aldorithm of python is **Timsort** which is average O(nlogn), and the adding process goes through the whole array once. </br>
Space: O(1)
```python3
class Solution:
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        low = 0
        high = len(nums)-1
        
        # In case no rotation
        if nums[low]<= nums[(low+high)//2] <= nums[high]:
            return nums[low]
        
        while low <= high:
            mid = (low+high)//2
            
            if nums[mid] > nums[mid+1]:
                return nums[mid+1]
            elif nums[mid] < nums[mid-1]:
                return nums[mid]
            
            
            if nums[low] <= nums[mid]: # mid in the left half
                low = mid + 1
            else:
                high = mid - 1 # mid in the right
```

Faster without checking every time:
```python3
class Solution:
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        left = 0
        right = len(nums) - 1
        while left < right:
            mid = (left + right) // 2
            if nums[mid] > nums[right]:
                left = mid + 1
            elif nums[mid] < nums[right]:
                right = mid
            else:
                right -= 1
        return nums[left]
```

