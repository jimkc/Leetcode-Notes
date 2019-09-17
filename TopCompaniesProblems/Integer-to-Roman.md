## Question
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.
 
<pre>
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
</pre>

For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.<br>

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:<br>

I can be placed before V (5) and X (10) to make 4 and 9. <br>
X can be placed before L (50) and C (100) to make 40 and 90. <br>
C can be placed before D (500) and M (1000) to make 400 and 900.<br>
Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

**Example :**   
<pre>
Input: 3
Output: "III"

Input: 4
Output: "IV"

Input: 4
Output: "IV"

Input: 58
Output: "LVIII"
Explanation: C = 100, L = 50, XXX = 30 and III = 3.

Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
</pre>

## Thinking


## Coding
Time: O(n);<br>
Space: O(n)
```python3
class Solution:
    def intToRoman(self, num):
        """
        :type num: int
        :rtype: str
        """
        symbols = { 1:"I", 5:"V", 10: "X", 50:"L", 100:"C", 500:"D", 1000:"M"}
        
        position = 1
        
        romans = []
        
        while num > 0:
            digit = num % 10
            
            if digit < 4:
                for _ in range(digit): romans.append( symbols[position])
            elif digit == 4 or digit == 9: 
                romans.append(symbols[(digit+1)*position]);romans.append(symbols[position])
            elif digit == 5:
                romans.append(symbols[digit*position])
            else:
                for _ in range(digit-5): 
                    romans.append(symbols[position])
                romans.append( symbols[5*position])
                
            position *= 10 # Go one digit toward left
            num = num //10     
        return "".join(romans[::-1])
```

