## Question
Reverse bits of a given 32 bits unsigned integer.

**Example :**
<pre>
Input: 43261596
Output: 964176192
Explanation: 43261596 represented in binary as 00000010100101000001111010011100, 
             return 964176192 represented in binary as 00111001011110000010100101000000.
</pre>

Note:
You may assume that both strings contain only lowercase letters.

## Thinking


## Coding
Time: O(1); </br>
Space: O(1).
```python
class Solution:
    # @param n, an integer
    # @return an integer
    def reverseBits(self, n):
        
        ans = 0
        for i in range(32):
            ans = (ans<<1) + (n & 1) # n&1 gets the last bit
            n >>= 1
        return ans
```

