## Question
Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.<br>

**Example :**
<pre>
Input: [3,0,1]
Output: 2

Input: [9,6,4,2,3,5,7,0,1]
Output: 8
</pre>

Note:<br>
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?


## Thinking
The first approach we use every element to xor each index, so while each element matches the index 
that is identify with itself it becomes zero (a^b^b = a, a^0 = a), the left element will be the ans.<br>

The second approach is faster by using the sum of the whole elements.


## Coding
Time: O(n);  </br>
Space: O(1).
```python3
class Solution:
    def missingNumber(self, nums):
        xor = 0

        for i, e in enumerate(nums):
            xor = xor ^ i ^ e
        
        return xor ^ len(nums)
```


Time: O(n);  </br>
Space: O(1).
```python3
class Solution(object):
    def missingNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        return int((n * (n+1))/2 - sum(nums))
```


