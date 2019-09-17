## Question
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.<br>

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).<br>

The replacement must be in-place and use only constant extra memory.

**Example :**   
<pre>
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
</pre>

## Thinking


## Coding
Time: O(n^2); 
Space: O(1)
```python3
class Solution:
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        have_per = False 
        
        for i in range(len(nums)-1,-1,-1):
            min_of_larger = float('inf')
            idx = -1
            
            # Check which bit is larger than the current bit, but also the smallest bit among the bits larger than cur bit
            # So that bit will be the next per
            for j in range(i+1,len(nums)):
                if nums[j] > nums[i] and nums[j] < min_of_larger:
                    min_of_larger = nums[j]
                    idx = j
             
            # After changing A'B'CDE to A'D'CBE the digit after the changed bit has to be sorted from small to big
            # EX: 15'4'832 -> 15'8'432, for next per 158abc has to be the first per for 158___ so abc needs to be small to big
            if idx != -1:
                tmp = nums[i]
                nums[i] = nums[idx]
                nums[idx] = tmp
                nums[i+1:] = sorted(nums[i+1:]) # Must use sorted instead of sort for partition list sorting
                have_per = True
                break
                            
        if not have_per:
            nums.sort()
```

