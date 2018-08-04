## Question
Given a non-negative integer, you could swap two digits at most once to get the maximum valued number. Return the maximum valued number you could get.

**Example :**   
<pre>
Input: 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.

Input: 9973
Output: 9973
Explanation: No swap.
</pre>

## Thinking


## Coding
Time: O(n^2); 
Space: O(1)
```python3
class Solution:
    def maximumSwap(self, num):
        """
        :type num: int
        :rtype: int
        """
        
        nums = [int(n) for n in list(str(num))]
        
        # Switch idx i and j, the larger nums[j] is, the value will be bigger
        # i to be as left as possible, j to be as right as possible
        for i in range(len(nums)):
            maxv = nums[i]
            idx  = 0
            
            for j in range(i+1,len(nums)):
                if nums[j] > nums[i] and nums[j] >= maxv:
                    maxv = nums[j]
                    idx = j
                    
            if idx != 0:
                tmp = nums[i]
                nums[i] = maxv
                nums[idx] = tmp
                break
                
        ans = [str(n) for n in nums] 
        return int(''.join(ans))
```

Faster one:
```python3
class Solution:
    def maximumSwap(self, nums_):
        """
        :type num: int
        :rtype: int
        """
        nums = [ord(s)-48 for s in str(nums_)]
        i = 0
        p = None
        for i, num in enumerate(nums[:-1]):
            ma = max(nums[i+1:])
            if num < ma:
                p = i
                q = len(nums) - 1 - list(reversed(nums)).index(ma)
                break
        if p == None:
            return nums_
        def power(i):
            if i == 0:
                return 1
            return 10*power(i-1)
        return nums_ + (ma - nums[p])*(power(len(nums)-1-p)-power(len(nums)-1-q))
```
