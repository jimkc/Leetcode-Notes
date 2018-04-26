## Question
Given an array of 2n integers, your task is to group these integers into n pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible. </br>

**Example :**   
Input: [1,4,3,2] </br>
Output: 4 </br>
Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).
## Thinking
It seems to me that by pairing the neighbor numbers in the sorted list will result the maximum value.
## Coding
Time: O(nlogn); The sorting aldorithm of python is **Timsort** which is average O(nlogn), and the adding process goes through the whole array once. </br>
Space: O(1)
```python3
class Solution:
            def arrayPairSum(self, nums):
                """
                :type nums: List[int]
                :rtype: int
                """
                nums.sort()
                max_sum_pair = 0
                for i in range(0,len(nums),2):
                    max_sum_pair += nums[i] 
                return max_sum_pair
```

