## Question
Given a 32-bit signed integer, reverse digits of an integer.

**Example :**   
<pre>
Input: 123
Output: 321

Input: -123
Output: -321

Input: 120
Output: 21
</pre>

Note:<br>
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## Thinking


## Coding
Time: O(n);<br>
Space: O(1)
```python3
class Solution:
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        rev = 0 # Reverse num
        while x != 0:
            pop = x - int(x/10)*10 # The last digit, negative number does not allow // and % operator
            x = int(x/10)

            if (rev > 2**31//10) or (rev == 2**31//10 and pop > 7):
                return 0
            if (rev < int(-2**31/10)) or (rev == int(-2**31/10) and pop < -8):
                return 0
            rev = rev*10 + pop
            
        return rev
```

