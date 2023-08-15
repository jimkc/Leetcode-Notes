## Question
Given an m x n matrix, return all elements of the matrix in spiral order.
https://leetcode.com/problems/spiral-matrix/description/

**Example 1:**
<pre>
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
</pre>

**Example 2:**
<pre>
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
</pre>

## Thought Process
1. We start by initializing the result list to store the elements in spiral order.
2. We perform some basic checks to handle edge cases where the matrix is null, empty, or has zero rows/columns. In such cases, we return the empty result list.
3. We define four boundaries: top, bottom, left, and right, which represent the edges of the current layer of the spiral.
4. We iterate while the top boundary is less than or equal to the bottom boundary and the left boundary is less than or equal to the right boundary.
5. We traverse the top row from left to right, adding each element to the result list. After traversing the row, we increment the top boundary.
6. We traverse the right column from top to bottom, adding each element to the result list. After traversing the column, we decrement the right boundary.
7. We check if the top boundary is less than or equal to the bottom boundary to avoid duplicate traversals. If true, we traverse the bottom row from right to left, adding each element to the result list. After traversing the row, we decrement the bottom boundary.
8. We check if the left boundary is less than or equal to the right boundary to avoid duplicate traversals. If true, we traverse the left column from bottom to top, adding each element to the result list. After traversing the column, we increment the left boundary.
9. We repeat steps 5-8 until all elements have been traversed

## Coding
Time: O(mxn); </br>
Space: O(mxn)

### Solution
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix)
    {
        List<Integer> result = new ArrayList<>();

        if(matrix == null || matrix.length == 0 || matrix[0].length == 0)
        {
            return result;
        }

        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;

        while(top <= bottom && right >= left)
        {
            // Traverse top row
            for (int i = left; i <= right; i++)
            {
                result.add(matrix[top][i]);
            }
            top++;

            // Traverse right column
            for (int i=top; i<=bottom; i++)
            {
                result.add(matrix[i][right]);
            }
            right--;

            // Traverse bottom row, add check to avoid duplicate
            if (top <= bottom)
            {
                for (int i=right; i>=left; i--)
                {
                    result.add(matrix[bottom][i]);
                }
                bottom--;
            }

            // Traverse left column

            if (right >= left)
            {
                for (int i=bottom; i>=top; i--)
                {
                    result.add(matrix[i][left]);
                }
                left++;
            }
        }

        return result;
    }
}
```


