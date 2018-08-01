## Question
Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.<br>

**Example :**   
<pre>
Input: [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.


Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
</pre>

## Thinking
To find the maximum length, we need a dict to store the value of count (as the key) and its associated index (as the value). We only need to save a count value and its index at the first time, when the same count values appear again, we use the new index subtracting the old index to calculate the length of a subarray. A variable max_length is used to to keep track of the current maximum length.
Picture: https://leetcode.com/problems/contiguous-array/discuss/99655/Python-O(n)-Solution-with-Visual-Explanation


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def findMaxLength(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        count = 0
        max_length=0
        table = {0: 0} 
        for index, num in enumerate(nums,1): # index start counting from 1, but enumerate starts from the first item
            if num == 0:
                count -= 1
            else:
                count += 1
            
            if count in table:
                max_length = max(max_length, index - table[count])
            else:
                table[count] = index
            print (index,num,count)
        return max_length
```

