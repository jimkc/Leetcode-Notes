## Question
Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one. </br>

Note:<br>
<br>
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?<br>

**Example :**   
<pre>
Input: [2,2,3,2]
Output: 3

Input: [0,1,0,1,0,1,99]
Output: 99
</pre>

## Thinking
https://leetcode.com/problems/single-number-ii/solution/<br>
Explanation of the bitwise approach.<br>

Bitwise operation, ^ is xor, ~ is not (bitwise not).<br>
## Coding
Time: O(n);  </br>
Space: O(1)
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        seen_once = seen_twice = 0
        
        for num in nums:
            # first appearance: 
            # add num to seen_once 
            # don't add to seen_twice because of presence in seen_once
            
            # second appearance: 
            # remove num from seen_once 
            # add num to seen_twice
            
            # third appearance: 
            # don't add to seen_once because of presence in seen_twice
            # remove num from seen_twice
            seen_once = ~seen_twice & (seen_once ^ num)
            seen_twice = ~seen_once & (seen_twice ^ num)

        return seen_once
```

