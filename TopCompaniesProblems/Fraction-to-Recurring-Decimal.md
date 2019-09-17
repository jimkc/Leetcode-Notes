## Question
Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.

If the fractional part is repeating, enclose the repeating part in parentheses.


**Example :**   
<pre>
Input: numerator = 1, denominator = 2
Output: "0.5"

Input: numerator = 2, denominator = 1
Output: "2"

Input: numerator = 2, denominator = 3
Output: "0.(6)"
</pre>

## Thinking


## Coding
Time: O(); <br>
Space: O()
```python3
class Solution:
    def fractionToDecimal(self, numerator, denominator):
        """
        :type numerator: int
        :type denominator: int
        :rtype: str
        """
        
        n, remain = divmod(abs(numerator), abs(denominator))
        sign = '-' if numerator*denominator < 0 else ''
        result = [sign + str(n)]
        
        if remain == 0:
            return ''.join(result)
        else:
            result.append('.')
            
        seen = {} # To record the position of each remain, each num of remainder wont appear twice, which means encounter cycle 
        idx = 0 # Start counting idx after '.'
        
        while remain not in seen:
            seen[remain] = idx
            idx += 1
            n, remain = divmod(remain*10, abs(denominator))
            result.append(str(n))
            
        result.insert(seen[remain]+2, '(') # Add 2 for the 'n' and '.' in the begining of the list
        result.append(')')
        return ''.join(result).replace('(0)', '').rstrip('.')
```

