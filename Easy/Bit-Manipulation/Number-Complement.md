## Question
Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation.<br>

Note:<br>
The given integer is guaranteed to fit within the range of a 32-bit signed integer.<br>
You could assume no leading zero bit in the integer’s binary representation.<br>

**Example :**
<pre>
Input: 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.

Input: 1
Output: 0
Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.
</pre>


## Thinking


## Coding
Time: O(n); n is the length of the bit transformation of num </br>
Space: O(n).
```python3
class Solution:
    def findComplement(self, num):
        """
        :type num: int
        :rtype: int
        """
        b = bin(num)[2:] #bin returns 0bxxxxxxx 0b is not what we want
        y = '1'*len(b) # xor -> if 1 exists return 1 else return 0
        return int(b,2)^int(y,2)
```

