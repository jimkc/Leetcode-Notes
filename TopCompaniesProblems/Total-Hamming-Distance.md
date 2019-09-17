## Question
The Hamming distance between two integers is the number of positions at which the corresponding bits are different.<br>

Now your job is to find the total Hamming distance between all pairs of the given numbers.

**Example :**   
<pre>
Input: 4, 14, 2

Output: 6

Explanation: In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just
showing the four bits relevant in this case). So the answer will be:
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.
</pre>

Note:
Elements of the given array are in the range of 0 to 10^9
Length of the array will not exceed 10^4.

## Thinking
To know how may bits differ in total, we just need to check for each bit in all numbers, there
exists how many 0 and 1, then there will be x*y combinations of differ bits for all pair.

## Coding
Time: O(b); num of bits.<br>
Space: O(b)
```python3
class Solution:
    def totalHammingDistance(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """

        bits = []
        bits_diff = [[0,0] for x in range(32)]
        for i in range(len(nums)):
            bits.append(bin(nums[i])[2:].zfill(32))
            
        for  i in range(32):
            for b in bits:
                if b[i] == '1':
                    bits_diff[i][1] += 1
                else:
                    bits_diff[i][0] += 1
        return sum( x*y for x,y in bits_diff ) 
```
