## Question
A matrix is Toeplitz if every diagonal from top-left to bottom-right has the same element.

Now given an M x N matrix, return True if and only if the matrix is Toeplitz.

**Example 1:**   
Input: matrix = [[1,2,3,4],[5,1,2,3],[9,5,1,2]]</br>
Output: True</br>
Explanation:</br>
1234</br>
5123</br>
9512</br>

In the above grid, the diagonals are "[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]", and in each diagonal all elements are the same, so the answer is True. </br>

**Example 2:**   
Input: matrix = [[1,2],[2,2]]</br>
Output: False</br>
Explanation:The diagonal "[1, 2]" has different elements.
    
**Note:**
matrix will be a 2D array of integers.
matrix will have a number of rows and columns in range [1, 20].
matrix[i][j] will be integers in range [0, 99].
## Thinking
Each index of a matrix only has to compare with the right left to see if they are the same. (Make the problem small.)
## Coding
Time: O(m*n). m*n matrix.
Space: O(1)
```python3
class Solution:
    def isToeplitzMatrix(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: bool
        """
        for i in range(len(matrix)-1):
            for j in range(len(matrix[0])-1):
                #print (matrix[i][j],matrix[i+1][j+1])
                if matrix[i][j] != matrix[i+1][j+1]:
                    return False
        return True
```

