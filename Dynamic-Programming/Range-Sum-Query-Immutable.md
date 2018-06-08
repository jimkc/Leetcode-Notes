## Question
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.<br>

**Example :**
<pre>
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
</pre>

Note:
You may assume that the array does not change.
There are many calls to sumRange function.

## Thinking
Save the sum of [:i+1] for each index, then we can get sumrange(i,j) value = j-(i-1),
however, while i = 0 the value of vals[-1] is not the value we want, so we insert 0 in the first 
index and sum range value to be j+1 - i.

## Coding
Time: O(n); Go through the array once. </br>
Space: O(n) 
```python3
class NumArray:

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        
        self.vals = nums
        for i in range(1,len(nums)):
            self.vals[i] += self.vals[i-1]
        nums.insert(0,0) 
        #print (self.vals)
        
    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        return self.vals[j+1] - self.vals[i]
        
        


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(i,j)
```

