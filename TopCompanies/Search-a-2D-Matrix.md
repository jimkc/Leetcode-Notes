## Question
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:<br>

Integers in each row are sorted from left to right.<br>
The first integer of each row is greater than the last integer of the previous row.

**Example :**   
<pre>
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true


Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
</pre>

## Thinking
Search row and then column.

## Coding
Time: O(logn); <br>
Space: O(1)
```python3
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if not matrix or not matrix[0]:
            return False
        low = 0
        high = len(matrix)-1
        
        
        while low <= high:
            mid = (low + high)//2
            
            if matrix[mid][0] == target:
                return True
            
            if matrix[mid][0] > target:
                high = mid - 1
            else:
                low = mid + 1
        
        row = low-1 # The start or row is closest to the target
        
        l = 0
        r = len(matrix[row])-1
        
        while l <= r:
            mid = (l + r)//2
            
            if matrix[row][mid] == target:
                return True
            
            if matrix[row][mid] > target:
                r = mid - 1
            else:
                l = mid + 1
        
        return False
```
