## Question
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

**Example :**   
<pre>
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]

Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
</pre>

## Thinking


## Coding
Time: O(m*n); Size of matrix <br>
Space: O(m*n)
```python3
class Solution:
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
       
        
        
        ans = []
        
        while matrix:
            if matrix and matrix[0]:
                ans+=matrix[0]
                del matrix[0]

            
            for i in range(len(matrix)):
                ans.append(matrix[i].pop())
            
            for i in range(len(matrix)-1,-1,-1):
                if not matrix[i]:
                    del matrix[i]
            
            
            if matrix and matrix[-1]:
                matrix[-1].reverse()
                ans+= matrix[-1]
                del matrix[-1]
            

            for i in range(len(matrix)-1,-1,-1):
                ans.append(matrix[i][0])
                del matrix[i][0]
                if not matrix[i]:
                    del matrix[i]

        return ans
```

Method without mutation: (Not understanded)
```python3
class Solution:
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        numList = []
        m = len(matrix)
        if m == 0:
            return []
        n = len(matrix[0])
        i = 0
        j = 0
        while(m > i and n > j):
            numList.extend(matrix[i][j:n])
            i += 1

            for b in range(i, m):
                numList.append(matrix[b][n-1])
            n -= 1
            if j >=n or i >= m:
                break
            for c in range(n-1, j-1, -1):
                numList.append(matrix[m-1][c])
            m -= 1
            for d in range(m-1, i-1, -1):
                numList.append(matrix[d][j])
            j += 1
            print(i,m,j,n)
                
                
        return numList
```
