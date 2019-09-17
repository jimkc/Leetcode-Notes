## Question
Given two sparse matrices A and B, return the result of AB.<br>

You may assume that A's column number is equal to B's row number.

**Example :**   
<pre>
Input:

A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]

Output:

     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
</pre>

## Thinking


## Coding
Time: O(cA*rA*cB);<BR>
Space: O(cA*rB)
```python3
class Solution:
    def multiply(self, A, B):
        """
        :type A: List[List[int]]
        :type B: List[List[int]]
        :rtype: List[List[int]]
        """
        if not A or not B: return []
        
        # the number of rows and cols of the product matrix.
        ROW, COL = len(A), len(B[0])
        ans = [[0] * COL for _ in range(ROW)]

        for i in range(ROW):
            for k in range(len(A[0])):
                if A[i][k] != 0:         # Saves a lot of time to deal with sparse matrix
                    for j in range(COL): # The direction is quite special, but helps to identify A[i][k] is 0 to save time
                        if B[k][j] != 0: # Saves a lot of time 
                            ans[i][j] += A[i][k] * B[k][j]
        return ans
```

