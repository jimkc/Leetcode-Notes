## Question
Given a positive integer, return its corresponding column title as appear in an Excel sheet.<br>

**Example :**   
<pre>
1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
    
    
Input: 1
Output: "A"

Input: 28
Output: "AB"

Input: 701
Output: "ZY"
<pre>

## Thinking
It is similar to bit manipulation, but we use 26 as base.

## Coding
Time: O(logn); With 26 as the base.<br>
Space: O(logn)
```python3
class Solution:
    def convertToTitle(self, n):
        """
        :type n: int
        :rtype: str
        """
        
        remains = []
        ans = ''
        while n != 0:
            remains.append((n-1) % 26) # n-1 -> n: 1~26 stands for a to z, but we need '0'~25 for carry 
            n = (n-1)//26
        for num in remains[::-1]:
            ans += chr((num+1) + 96).upper() # num + 1 to reverse back a-z to 1-26 
        return ans
        
```

