## Question
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:<br>

Integers in each row are sorted in ascending from left to right.<br>
Integers in each column are sorted in ascending from top to bottom.

**Example :**   
<pre>
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]

Given target = 5, return true.

Given target = 20, return false.
</pre>

## Thinking
Binary search each list.

## Coding
Time: O(nlogn); <br>
Space: O(1)
```python3
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        for array in matrix:
            if self.bisearch(array,target): return True
        return False
        
        
    def bisearch(self, array, target):
        l = 0
        r = len(array)-1 
        
        while l <= r:
            
            mid = (l+r)//2
           
            if array[mid] == target:
                return True
            elif array[mid] < target:
                l = mid+1
            else:
                r = mid-1
        
        return False
```

