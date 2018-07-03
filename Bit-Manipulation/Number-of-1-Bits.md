## Question
Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

**Example :**
<pre>
Input: 11
Output: 3
Explanation: Integer 11 has binary representation 00000000000000000000000000001011 

Input: 128
Output: 1
Explanation: Integer 128 has binary representation 00000000000000000000000010000000
</pre>


## Thinking


## Coding
Time: O(n); Add items to collection takes O(R+M). </br>
Space: O(1).

```python3
class Solution(object):
    def hammingWeight(self, n):
        """
        :type n: int
        :rtype: int
        """
        sum1 = 0
        while n > 0:
            sum1+=1
            n = n & (n-1) # 110 & 101 = 100 erase the last 1
        return sum1
```

Faster:
```python
class Solution(object):
    def hammingWeight(self, n):
        """
        :type n: int
        :rtype: int
        """
        return bin(n).count('1')
```
