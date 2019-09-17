## Question
Given a sorted integer array without duplicates, return the summary of its ranges.

**Example :**   
<pre>
Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.

Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(1)
```python3
class Solution:
    def summaryRanges(self, nums):
        """
        :type nums: List[int]
        :rtype: List[str]
        """
        if not nums:
            return []
        
        start = nums[0]
        ans = []
        
        for i in range(0,len(nums)-1):
            if nums[i]+1 != nums[i+1]: # Meet the break point
                if start != nums[i]:
                    ans.append(str(start) + '->' + str(nums[i]))
                else:
                    ans.append(str(start))
                start = nums[i+1]
           
        if start != nums[-1]:
            ans.append(str(start) + '->' + str(nums[-1]))
        else:
            ans.append(str(start))
        
        return ans
```

