## Question
Given a non-empty array of numbers, a0, a1, a2, … , an-1, where 0 ≤ ai < 231.<br>

Find the maximum result of ai XOR aj, where 0 ≤ i, j < n.<br>

Could you do this in O(n) runtime?

**Example :**   
<pre>
Input: [3, 10, 5, 25, 2, 8]

Output: 28

Explanation: The maximum result is 5 ^ 25 = 28.
</pre>

## Thinking
To find the maximum XOR we want two nums to have different num for each bit, 0 to match 1
and 1 to match 0.

## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def findMaximumXOR(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        root = {}
        trie = root
        output = float("-inf")
        
        for num in nums:
            
            # Build the current num into the trie tree
            biStr = format(num,'b').zfill(31) # At most 31 digits, zfill append zero in front of the string
            
            for bit in biStr:
                if bit not in trie:
                    trie[bit] = {} # Build the branch of the tree
                trie = trie[bit]
            
            trie['value'] = num # Record the cur num in the leaf
            trie = root # Reset the starting point
            
            # Search for the num that xor the most now, the future num will match the nums too
            for bit in biStr:
                if bit == '1' and trie.get('0'):
                    trie = trie['0']
                elif bit == '0' and trie.get('1'):
                    trie = trie['1']
                else:
                    trie = trie[bit]
            
            output = max(trie['value']^num, output)
            trie = root
        
        return output
```

