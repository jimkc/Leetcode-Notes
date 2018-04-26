## Question
Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.
**Example :**   
Input:</br> 
<code>&nbsp;</code>Tree 1                     Tree 2                
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
Output: 
Merged tree:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
Note: The merging process must start from the root nodes of both trees.

## Thinking
It seems to me that by pairing the neighbor numbers in the sorted list will result the maximum value.
## Coding
Time: O(nlogn); The sorting aldorithm of python is **Timsort** which is average O(nlogn), and the adding process goes through the whole array once. </br>
Space: O(1)
```python3
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

