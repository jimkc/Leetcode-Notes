## Question
Given an integer array sorted in ascending order, write a function to search target in nums.  If target exists, then return its index, otherwise return -1. However, the array size is unknown to you. You may only access the array using an ArrayReader interface, where ArrayReader.get(k) returns the element of the array at index k (0-indexed).<br>
<br>
You may assume all integers in the array are less than 10000, and if you access the array out of bounds, ArrayReader.get will return 2147483647.<br>


**Example :**   
<pre>
Input: array = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
</pre>

**Example2**
<pre>
Input: array = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
</pre>

Note:<br>
<br>
You may assume that all elements in the array are unique.<br>
The value of each element in the array will be in the range [-9999, 9999].


## Thinking
Solution: https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/solution/

## Coding
Time: O(logn); This is a faster way to find element in logn</br>
Space: O(1)
```python
class Solution:
    def search(self, reader, target):
        """
        :type reader: ArrayReader
        :type target: int
        :rtype: int
        """
        
        left, right = 0, 1
        
        # Define the boundaries
        while reader.get(right) < target:
            left = right
            # same as right*=2, but in a faster way
            right <<= 1 
        
        # Search in the boundary
        while left <= right:
            # in case right + left will overflow
            # same as left + right/2 - left/2
            middle = left + ((right-left) >> 1)
            
            if reader.get(middle) == target:
                return middle
            elif reader.get(middle) < target:
                left = middle+1
            else:
                right = middle-1
        return -1
            
```

