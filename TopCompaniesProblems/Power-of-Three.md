## Question
Given an integer, write a function to determine if it is a power of three.

**Example :**   
<pre>
Input: 27
Output: true

Input: 0
Output: false

Input: 9
Output: true
</pre>

Follow up:
Could you do it without using any loop / recursion?


## Thinking
3**(log MaxInt) == 3**19.56

## Coding
Time: O(1); <br>
Space: O(1)
```python3
class Solution:
    def isPowerOfThree(self, n):
        """
        :type n: int
        :rtype: bool
        """
        return  n > 0 and 3**19 % n==0
           
        
```

