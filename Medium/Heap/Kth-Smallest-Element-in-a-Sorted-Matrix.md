## Question
Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.<br>

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

**Example :**   
<pre>
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
</pre>

Note: <br>
You may assume k is always valid, 1 ≤ k ≤ n2

## Thinking
用一個min heap來取，但是不用一次把所有東西都放進去，可以先放一個col，然後拿一個在放一個進去就好，這種方式要記錄row和col。

## Coding
Time: O(n); <br>
Space: O(col)
```python3
class Solution:
    def kthSmallest(self, matrix, k):
        # only put one column inside heap, save time because dont need to go over whole matrix
        heap = [(matrix[0][i], 0, i) for i in range(len(matrix[0]))]
        heapq.heapify(heap)
        for i in range(k-1):
            v, r, c = heapq.heappop(heap)
            if r+1 < len(matrix):
                # add the next item in next row of same column to the heap 
                heapq.heappush(heap, (matrix[r+1][c], r+1, c))
        return heapq.heappop(heap)[0]
```

