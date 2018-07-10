## Question
A self-dividing number is a number that is divisible by every digit it contains.<br>

For example, 128 is a self-dividing number because 128 % 1 == 0, 128 % 2 == 0, and 128 % 8 == 0.<br>

Also, a self-dividing number is not allowed to contain the digit zero.<br>

Given a lower and upper number bound, output a list of every possible self dividing number, including the bounds if possible.<br>

**Example :**
<pre>
Input: 
left = 1, right = 22
Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]
</pre>

Note:

The boundaries of each input argument are 1 <= left <= right <= 10000.

## Thinking
After transforming the numbers to string first, we can save a lot of effort to divide numbers
which took us more time.

## Coding
Time: O(n*m); Go through the array once times the longest length of digits m. </br>
Space: O(n). 
```python3
class Solution:
    def selfDividingNumbers(self, left, right):
        """
        :type left: int
        :type right: int
        :rtype: List[int]
        """
        result = []
        for i in range(left, right+1):
            if '0' in str(i):
                continue
            is_sd = True
            for char in str(i):
                if i % int(char) != 0:
                    is_sd = False
                    break
            if is_sd:
                result.append(i)
        return result
                    
```

