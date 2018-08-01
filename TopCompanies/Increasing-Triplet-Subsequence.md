## Question
Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.<br>

Formally the function should:<br>

Return true if there exists i, j, k <br>
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.<br>
Note: Your algorithm should run in O(n) time complexity and O(1) space complexity.

**Example :**   
<pre>
Input: [1,2,3,4,5]
Output: true

Input: [5,4,3,2,1]
Output: false
</pre>

## Thinking
It seems to me that by pairing the neighbor numbers in the sorted list will result the maximum value.

## Coding
Time: O(n); <br>
Space: O(1)
```python3
class Solution:
    def increasingTriplet(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        
        first = second = float('inf')
        # The first and second only gets smaller, which means the if previous first and second is a correct case, the later one will be correct too
        for n in nums:
            if n <= first: # Although it didnt update the 2nd, if we find the one bigger than 2nd, the pre case still is right
                first = n
            elif n <= second:
                second = n
            else:
                return True
        return False
```
