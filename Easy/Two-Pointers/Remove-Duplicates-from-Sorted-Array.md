## Question
Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.</br>

**Example 1:**
<pre>
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
</pre>

**Example 2:**
<pre>
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
</pre>

## Thinking
Using two pointers and move elements to the front without duplicates. Then remove the uneeded elements once together.</br>

## Coding
Time: O(n); Go through the array once. </br>
Space: O(1) 
```python3
class Solution:
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        length = 1
        i = 1
        for  j in range(1,len(nums)):
            if nums[j] != nums[j-1]:
                nums[i] = nums[j]
                i += 1
                length += 1
        nums = nums[:length]
        return len(nums)
```

