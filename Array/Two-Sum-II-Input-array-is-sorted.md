## Question
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Example :**
<pre>
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
</pre>

Note:

Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.

## Thinking
Use two pointers to go from the left and right together. We start from the begining and the last index if the 
array, if the addition of the two nums is larger than the target, we move i2 1 index away from the bottom,
if it is smaller than the target, we move i1 1 index away from the begining.
## Coding
Time: O(n); Go through the array once. </br>
Space: O(1) 
```python3
class Solution:
    def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        i1 = 0
        i2 = len(numbers) - 1
        
        while True:
            value = numbers[i1] + numbers[i2]
            if value > target:
                i2 -= 1
            elif value < target:
                i1 += 1
            else:
                return [i1+1, i2+1]
```

