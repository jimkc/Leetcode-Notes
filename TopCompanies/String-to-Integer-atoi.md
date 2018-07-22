## Question
mplement atoi which converts a string to an integer.<br>

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.<br>

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.<br>

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.<br>

If no valid conversion could be performed, a zero value is returned.<br>

Note:<br>

Only the space character ' ' is considered as whitespace character.<br>
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.

**Example :**   
<pre>
Input: "42"
Output: 42

Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.

Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.

Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.

Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def myAtoi(self, str):
        """
        :type str: str
        :rtype: int
        """
        chars = list(str)[::-1]
        if not chars:
            return 0
        ans = []
        sign = 1
        
        while chars and chars[-1] == ' ':
            chars.pop()
            
        if chars :
            if chars[-1] == '-':
                sign = -1
                chars.pop()
            elif chars[-1] == '+':
                chars.pop()
            
        
        while chars and chars[-1].isdigit():
            c = chars.pop()
            ans.append(c)
            
        if not ans:
            return 0
        res = sign * int(''.join(ans))
        if res > 2**31-1:
            return 2**31-1
        elif res < -(2**31):
            return -(2**31)
        else:
            return res
```

