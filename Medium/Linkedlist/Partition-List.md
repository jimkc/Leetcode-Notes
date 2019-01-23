## Question
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.<br>

You should preserve the original relative order of the nodes in each of the two partitions.

**Example :**   
<pre>
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
</pre>

## Thinking
和sort list 那題的quicksort有點像，用兩個額外的list，一個放比x大或等於，一個放比x小的。最後在合起來。

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

