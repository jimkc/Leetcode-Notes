## Question
Given a non-empty array of integers, every element appears twice except for one. Find that single one.<br>

Note:<br>

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?<br>

**Example :**
<pre>
Input: [2,2,1]
Output: 1

Input: [4,1,2,1,2]
Output: 4
</pre>


## Thinking
Exclusive or.

## Coding
Time: O(n); . </br>
Space: O(1).
```python3
class Solution:
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        xor = 0 # 0 ^ anything = anything
        for n in nums:
            xor ^= n
        return xor
        
```

