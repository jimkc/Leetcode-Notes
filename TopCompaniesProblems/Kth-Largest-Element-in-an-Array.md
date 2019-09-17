## Question
Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example :**   
<pre>
Input: [3,2,1,5,6,4] and k = 2
Output: 5

Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
</pre>

## Thinking

## Coding
Time: We convert nums to a heap list in-place which is O(n) time and then pop len(nums) - k items off the heap (O(log n)) until the k largest elements remain.<br>
Space: O(n)
```python3
import heapq

class Solution:
    def findKthLargest(self, nums, k):
        heapq.heapify(nums)
        
        for _ in range(len(nums)-k):
            a = heapq.heappop(nums)
    
        return nums[0]
```
