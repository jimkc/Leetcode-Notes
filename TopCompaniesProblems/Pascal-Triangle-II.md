## Question
Given a non-negative index k where k ≤ 33, return the kth index row of the Pascal's triangle.<br>

Note that the row index starts from 0.

**Example :**   
<pre>
         1
        11
       121
      1331
     14641
    
Input: 3
Output: [1,3,3,1]
</pre>

## Thinking


## Coding
Time: O(n^2);<br>
Space: O(n)
```python3
class Solution:
    def getRow(self, rowIndex):
        """
        :type rowIndex: int
        :rtype: List[int]
        """
        row = [1]
        for i in range(1,rowIndex+1):
            tmp = [0]*(i+1)
            tmp[0],tmp[-1] = 1,1
            
            for j in range(1,len(tmp)-1):
                tmp[j] = row[j-1] + row[j]
            
            row = tmp 
        return  (row)
```

More elegant method:
```python
class Solution(object):
    def getRow(self, rowIndex):
        """
        :type rowIndex: int
        :rtype: List[int]
        """
        row = [1]
        for _ in range(rowIndex):
            row = [x + y for x, y in zip([0]+row, row+[0])]
        return row
```

