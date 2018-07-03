## Question
Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.<br>

Note:<br>

All letters in hexadecimal (a-f) must be in lowercase.<br>
The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.<br>
The given number is guaranteed to fit within the range of a 32-bit signed integer.<br>
You must not use any method provided by the library which converts/formats the number to hex directly.<br>

**Example :**
<pre>
Input:
26

Output:
"1a"



Input:
-1

Output:
"ffffffff"
</pre>


## Thinking
Flipping the negative number.

## Coding
Time: O(1) Only 8 hex digits;  </br>
Space: O(1).
```python3
class Solution:
    def toHex(self, num):
        """
        :type num: int
        :rtype: str
        """
        ans = []
        dig = {10:'a',11:'b',12:'c',13:'d',14:'e',15:'f'}
        
        if num == 0:
            return '0'
        elif num < 0:
            num = num + 2**32 # flip the negative number to positive
        
        while num > 0:
            digit = num % 16
            if digit in dig:
                ans.append(dig[digit])
            else:
                ans.append(str(digit))
            num = int((num - digit)/16)
        return ''.join(ans[::-1])
```

