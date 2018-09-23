## Question
Given an integer array, find three numbers whose product is maximum and output the maximum product.

**Example :**   
<pre>
Input: [1,2,3]
Output: 6

Input: [1,2,3,4]
Output: 24
</pre>

Note:<br>
The length of the given array will be in range [3,104] and all elements are in the range [-1000, 1000].<br>
Multiplication of any three numbers in the input won't exceed the range of 32-bit signed integer.

## Thinking

## Coding
Time: O(n);<br>
Space: O(1)
```python3
class Solution(object):
    def maximumProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        max_1, max_2, max_3 = float('-inf'), float('-inf'), float('-inf') # The biggest 3 nums (max1 > 2 > 3)
        min_1, min_2 = float('inf'), float('inf') # The smallest 2 nums (min1 < min2 )
        
        for num in nums:
            if num > max_1:
                max_3, max_2, max_1 = max_2, max_1, num
            elif num > max_2:
                max_3, max_2 = max_2, num
            elif num > max_3:
                max_3 = num
                
            if num < min_1:
                min_2, min_1 = min_1, num
            elif num < min_2:
                min_2 = num
                
        return max(max_1*max_2*max_3, min_1*min_2*max_1)

```

