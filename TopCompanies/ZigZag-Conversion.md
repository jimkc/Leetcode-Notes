## Question
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
<pre>
P   A   H   N
A P L S I I G
Y   I   R
</pre>
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:
<pre>
    string convert(string s, int numRows);
</pre>

**Example :**
<pre>
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
</pre>

<pre>
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
</pre>


## Thinking
The better method walks zig zag through the array and record the chars in the string.

## Coding
My method<br>
Time: O(n);  </br>
Space: O(n) 
```python3
class Solution:
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        
        
        ans = []
        oblique = numRows-2 if numRows > 2 else 0 # nums of oblique in one cycle
        cycle = oblique+numRows # num of chars in a cycle
        
        if numRows ==1 or len(s) <= numRows:
            return s
        
        # First row
        idx = 0
        while idx < len(s):
            ans.append(s[idx])
            idx+=cycle
        
        
        # middle rows
        for i in range(1,numRows-1):
            pointer = i
            ans.append(s[i])
            
            while True:
                pointer += (cycle-2*i) # the index of the next oblique items in the ith row
                
                if pointer < len(s):
                    ans.append(s[pointer])
                else:
                    break
                
                pointer += (2*i)
                
                if pointer < len(s):
                    ans.append(s[pointer])
                else:
                    break
        
        # last row
        idx = numRows-1
        while idx < len(s):
            ans.append(s[idx])
            idx+=cycle
        
        return ''.join(ans)
```
Better method:
```python3
class Solution(object):
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        if numRows == 1 or numRows >= len(s):
            return s

        L = [''] * numRows
        index, step = 0, 1

        for x in s:
            L[index] += x
            if index == 0:
                step = 1
            elif index == numRows -1:
                step = -1
            index += step

        return ''.join(L)
```
