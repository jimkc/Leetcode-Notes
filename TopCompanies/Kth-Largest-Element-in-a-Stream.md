## Question
Design a class to find the kth largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.<br>

Your KthLargest class will have a constructor which accepts an integer k and an integer array nums, which contains initial elements from the stream. For each call to the method KthLargest.add, return the element representing the kth largest element in the stream.<br>

**Example :**   
<pre>
int k = 3;
int[] arr = [4,5,8,2];
KthLargest kthLargest = new KthLargest(3, arr);
kthLargest.add(3);   // returns 4
kthLargest.add(5);   // returns 5
kthLargest.add(10);  // returns 5
kthLargest.add(9);   // returns 8
kthLargest.add(4);   // returns 8
</pre>

Note: <br>
You may assume that nums' length ≥ k-1 and k ≥ 1.

## Thinking


## Coding
Time: Init: O(nlogn) Add: O(logn); 
Space: O(1)
```python3
class KthLargest:

    def __init__(self, k, nums):
        """
        :type k: int
        :type nums: List[int]
        """
        self.pool = nums
        self.k = k
        heapq.heapify(self.pool) # min heap
        while len(self.pool) > k:
            heapq.heappop(self.pool)
        
        

    def add(self, val):
        """
        :type val: int
        :rtype: int
        """
        if len(self.pool) < self.k:
            heapq.heappush(self.pool,val)
        elif val > self.pool[0]: 
            heapq.heapreplace(self.pool, val) # pop smallest and push item inside, more efficient than heappush then heappop
        return self.pool[0]

# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```

