## Question
Given a binary tree, determine if it is a complete binary tree.<br>
<br>
Definition of a complete binary tree from Wikipedia:<br>
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.</br>

**Example :**   
<pre>
https://leetcode.com/problems/check-completeness-of-a-binary-tree/description/
</pre>

## Thinking


## Coding
Time: O(n); </br>
Space: O(n)
```python
class Solution:
            def arrayPairSum(self, nums):
                #:type nums: List[int]
                #:rtype: int

                nums.sort()
                max_sum_pair = 0
                for i in range(0,len(nums),2):
                    max_sum_pair += nums[i] 
                return max_sum_pair
```

