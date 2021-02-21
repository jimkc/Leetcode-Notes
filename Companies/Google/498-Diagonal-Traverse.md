## Question
Given a matrix of M x N elements (M rows, N columns), return all elements of the matrix in diagonal order as shown in the below image. </br>

**Example :**   
<pre>
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]

Output:  [1,2,4,7,5,3,6,8,9]

Explanation:
https://leetcode.com/problems/diagonal-traverse/description/
</pre>

## Thinking
Simple two step approach:<br>
1- Group numbers according to diagonals. Sum of row+col in same diagonal is same.<br>
2- Reverse numbers in odd diagonals before adding numbers to result list.

## Coding
Time: O(mn);  </br>
Space: O(mn)
```python
class Solution:
    def findDiagonalOrder(self, matrix: List[List[int]]) -> List[int]:
        
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return []
        
        ans = []
        diagonals = collections.defaultdict(list)
        
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                diagonals[i+j].append(matrix[i][j])
        
        # go through all the i+j sums
        for k in range(len(matrix)+len(matrix[0])-1):
            nums = diagonals[k]
            if k % 2 != 0:
                for n in nums:
                    ans.append(n)
            else:
                for i in range(len(nums)-1, -1, -1):
                    ans.append(nums[i])
        return ans
```

