## Question
Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.<br>

For example,<br>
[2,3,4], the median is 3<br>

[2,3], the median is (2 + 3) / 2 = 2.5<br>

Design a data structure that supports the following two operations:<br>

void addNum(int num) - Add a integer number from the data stream to the data structure.<br>
double findMedian() - Return the median of all elements so far.

**Example :**   
<pre>
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
</pre>

Follow up:<br>

If all integer numbers from the stream are between 0 and 100, how would you optimize it?<br>
If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?<br>

## Thinking
Method2:<br>
I keep two heaps (or priority queues):<br>

Max-heap small has the smaller half of the numbers.<br>
Min-heap large has the larger half of the numbers.<br>
This gives me direct access to the one or two middle values (they're the tops of the heaps), so getting the median takes O(1) time. And adding a number takes O(log n) time.<br>

Supporting both min- and max-heap is more or less cumbersome, depending on the language, so I simply negate the numbers in the heap in which I want the reverse of the default order. To prevent this from causing a bug with -231 (which negated is itself, when using 32-bit ints), I use integer types larger than 32 bits.<br>

Using larger integer types also prevents an overflow error when taking the mean of the two middle numbers. I think almost all solutions posted previously have that bug.<br>

## Coding
Time: O(); <br>
Space: O()
```python3
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.nums = []
        

    def addNum(self, num):
        """
        :type num: int
        :rtype: void
        """
        if not self.nums:
            self.nums.append(num)
            return
            
        l = 0
        r = len(self.nums)-1
        
        while l <= r:
            mid = (l+r)//2
            
            if self.nums[mid] == num:
                self.nums.insert(mid,num)
                return
            elif self.nums[mid] < num:
                l = mid+1
            else:
                r = mid-1
        
        self.nums.insert(l,num)
                
            
    def findMedian(self):
        """
        :rtype: float
        """
        #print (self.nums)
        idx = len(self.nums)//2
        if len(self.nums)%2 == 0:
            return (self.nums[idx-1]+self.nums[idx])/2
        else:
            return self.nums[idx]
        


# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```

Method 2:
```python3
from heapq import *

class MedianFinder:

    def __init__(self):
        self.heaps = [], []

    def addNum(self, num):
        small, large = self.heaps
        heappush(small, -heappushpop(large, num))
        #maintain the number of len(large) >= len(small)  
        if len(large) < len(small):
            heappush(large, -heappop(small))

    def findMedian(self):
        small, large = self.heaps
        if len(large) > len(small):
            return float(large[0])
        return (large[0] - small[0]) / 2.0
```
