## Question
Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.<br>

Return the quotient after dividing dividend by divisor.<br>

The integer division should truncate toward zero.

**Example :**   
<pre>
Input: dividend = 10, divisor = 3
Output: 3

Input: dividend = 7, divisor = -3
Output: -2
</pre>

Both dividend and divisor will be 32-bit signed integers.<br>
The divisor will never be 0.<br>
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.

## Thinking
假設a是被除數，b是除數，每次a都除以b乘以2的i次方，因為最多32位元所以最多只會除32次。<br>
從a的最左邊位數開始看，想像用手遮住右邊的bits，每次只看最左32-i個，如果夠減b帶表b<< i 超過目前a的值的一半，所以當我們扣掉b<< i時不用擔心下次相減a的最左邊的有數字位數會不會比b的最左邊有數字位數少，因為一但少了代表a現在的值不到b了所以之後都不會讓ans＋。<br>
另外b<< i 其實就是b＊2^i＝b＊（1<< i），所以ans才是加那樣  

## Coding
Time: O(nlogn); binary division <br>
Space: O(1)
```python3
class Solution:
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        #when overflow
        if (dividend == -2147483648 and divisor == -1): return 2147483647
        
        ans = 0
        a , b = abs(dividend), abs(divisor)
        for i in range(32)[::-1]:
            # means a - b<<i >=0
            if (a >> i) - b >= 0:
                # because binary are always 1 and 0, so if >=0, means the largest bit is 1-0 or 1-1
                ans += 1<<i
                a -= b << i
        return ans if (dividend > 0) == (divisor > 0) else -ans
```

