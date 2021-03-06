## Question
In MATLAB, there is a very useful function called 'reshape', which can reshape a matrix into a new one with different size but keep its original data.

You're given a matrix represented by a two-dimensional array, and two positive integers r and c representing the row number and column number of the wanted reshaped matrix, respectively.

The reshaped matrix need to be filled with all the elements of the original matrix in the same row-traversing order as they were.

If the 'reshape' operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

**Example 1:**   
<pre>
Input: 
nums = 
[[1,2],
 [3,4]]
r = 1, c = 4
Output: 
[[1,2,3,4]]
Explanation:
The row-traversing of nums is [1,2,3,4]. The new reshaped matrix is a 1 * 4 matrix, fill it row by row by using the previous list.
</pre>
**Example 2:**
<pre>
Input: 
nums = 
[[1,2],
 [3,4]]
r = 2, c = 4
Output: 
[[1,2],
 [3,4]]
Explanation:
There is no way to reshape a 2 * 2 matrix to a 2 * 4 matrix. So output the original matrix.
</pre>

Note:
1.The height and width of the given matrix is in range [1, 100].</br>
2.The given r and c are all positive.

## Thinking
Traverse through all the elements in the original matrix. Count the index for each element to put it in the new matrix.
## CodingInput: 
Time: O(m*n) 
Space: O(m*n)
```python3
class Solution:
    def matrixReshape(self, nums, r, c):
        """
        :type nums: List[List[int]]
        :type r: int
        :type c: int
        :rtype: List[List[int]]
        """
        if r*c != len(nums[0])*len(nums):
            return nums
        else:
            #print (r,c)
            #print (len(nums[0]))
            
            new_matrix = []
            cnt = 0
            l = []
            for i,v in enumerate(nums): #Enumerate through each row
                for index,value in enumerate(v):
                    l.append(value)
                    if len(l) == c:
                        temp = []
                        for item in l:
                            temp.append(item)
                        new_matrix.append(temp)
                        l.clear()
            return new_matrix
```

