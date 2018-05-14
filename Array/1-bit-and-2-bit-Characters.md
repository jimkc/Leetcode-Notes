## Question
We have two special characters. The first character can be represented by one bit 0. The second character can be represented by two bits (10 or 11).

Now given a string represented by several bits. Return whether the last character must be a one-bit character or not. The given string will always end with a zero.
</br>
**Example 1:**
<pre>
Input: 
bits = [1, 0, 0]
Output: True
Explanation: 
The only way to decode it is two-bit character and one-bit character. So the last character is one-bit character.
</pre>

**Example 2:**
<pre>
Input: 
bits = [1, 1, 1, 0]
Output: False
Explanation: 
The only way to decode it is two-bit character and two-bit character. So the last character is NOT one-bit character.
</pre>

Note:

1 <= len(bits) <= 1000.
bits[i] is always 0 or 1.

## Thinking
Ignore the last bit, the only thing we have to make sure is whether index 0 to len(bits)-1 can be well arranged as complete chars.
To be sure if this, we go from the begining of the bits, if it's 0 then the char must be one bit, if it's 1 the the char must be 2 bits.

## Coding
Time: O(n); Go through the array once only. </br>
Space: O(1) 
```python3
class Solution:
    def isOneBitCharacter(self, bits):
        """
        :type bits: List[int]
        :rtype: bool
        """
        pointer = 0
        while pointer < len(bits) -1:
            pointer += bits[pointer] + 1 #If its 0 it must be a one bit char, if its 1 it would be 2 bit char
        return pointer == len(bits) -1 
```

