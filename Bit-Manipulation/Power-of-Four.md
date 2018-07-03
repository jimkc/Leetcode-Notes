## Question
Given an integer (signed 32 bits), write a function to check whether it is a power of 4.<br>

**Example :**
<pre>
Given num = 16, return true. Given num = 5, return false.
</pre>


## Thinking


## Coding
Time: O(logn);</br>
Space: O(1).
```python3
class Solution:
    def isPowerOfFour(self, n):
        test = 1
        while test < n:
            test = test << 2
        return test == n
```

Without loop:
```python3
"""
        :type num: int
        :rtype: bool
        """
        return num > 0 and (math.log2(num) / math.log2(4)).is_integer()
```




