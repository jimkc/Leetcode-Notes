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

For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.


**Example :**
<pre>
Input: "III"
Output: 3
</pre>

**Example :**
<pre>
Input: "IV"
Output: 4
</pre>

**Example :**
<pre>
Input: "IX"
Output: 9
</pre>

**Example :**
<pre>
Input: "LVIII"
Output: 58
Explanation: C = 100, L = 50, XXX = 30 and III = 3.
</pre>

**Example :**
<pre>
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
</pre>


## Thinking
For those Roman chars, we start counting from the right. <br>
For XY as example, if Y is larger than X, the value will be Y-X.<br>
Otherwise, the value will be Y+X.


## Coding
Time: O(n); Go through the array once. </br>
Space: O(1). 
```python3
class Solution:
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        table = {'I':1, 'V':5, 'X':10, 'L':50, 'C':100, 'D':500, 'M':1000}
        ans = table[s[-1]]
        
        for i in range(len(s)-2, -1, -1):
            if table[s[i]] < table[s[i+1]]:
                ans -= table[s[i]]
            else:
                ans += table[s[i]]
        return ans
```

