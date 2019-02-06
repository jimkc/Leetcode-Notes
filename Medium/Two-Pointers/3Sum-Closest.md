## Question
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example :**   
<pre>
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
</pre>

## Thinking
這題的觀念是用三個指標，其中一個是current，會go through整個array，另外兩個分別以current+1和last開始往內夾擠。<br>
第二個方式是每次在夾擠前先檢查current和最小的兩個有沒有比target大，或最大的兩個有沒有比target小，這樣就可以直接跳過。

## Coding
Time: O(n^2);
Space: O(1)
```python3
class Solution:
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        ans = nums[0] + nums[1] + nums[2]
        
        for i in range(len(nums)-2):
            j, k = i+1, len(nums)-1
            
            # Use two pointers to avoid unnecessary cases
            while j < k:
                val = nums[i] + nums[j] + nums[k] 
                
                if abs(val-target) < abs(ans-target):
                    ans = val
                
                if val < target:
                    j+=1
                elif val > target:
                    k-=1
                else:
                    return target
        return ans
```

Faster:<br>
In each loop,<br>
I judge if current number plus two largest numbers less than target,<br>
or current number plus two lowest numbers greater than target.
```python3
 nums.sort()
        length = len(nums)
        closest = []
        
        for i, num in enumerate(nums[0:-2]):
            l,r = i+1, length-1
						
            # different with others' solution
						
            if num+nums[r]+nums[r-1] < target:
                closest.append(num+nums[r]+nums[r-1])
            elif num+nums[l]+nums[l+1] > target:
                closest.append(num+nums[l]+nums[l+1])
            else:
                while l < r:
                    closest.append(num+nums[l]+nums[r])
                    if num+nums[l]+nums[r] < target:
                        l += 1
                    elif num+nums[l]+nums[r] > target:
                        r -= 1
                    else:
                        return target
                    
        closest.sort(key=lambda x:abs(x-target))
        return closest[0]
```
