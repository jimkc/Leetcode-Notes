## Question
Given a 32-bit signed integer, reverse digits of an integer.

**Example :**   
<pre>
Input: 123
Output: 321

Input: -123
Output: -321

Input: 120
Output: 21
</pre>

Note:<br>
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## Thinking


## Coding
Time: O(n);<br>
Space: O(1)

```python
class Solution:
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        rev = 0 # Reverse num
        while x != 0:
            pop = x - int(x/10)*10 # The last digit, negative number does not allow // and % operator
            x = int(x/10)

            if (rev > 2**31//10) or (rev == 2**31//10 and pop > 7):
                return 0
            if (rev < int(-2**31/10)) or (rev == int(-2**31/10) and pop < -8):
                return 0
            rev = rev*10 + pop
            
        return rev
```

###Solution 2
Algorithm

Reversing an integer can be done similarly to reversing a string.<br>

We want to repeatedly "pop" the last digit off of x and "push" it to the back of the rev. In the end, rev will be the reverse of the x.<br>

To "pop" and "push" digits without the help of some auxiliary stack/array, we can use math.<br>

Time: `O(log(x))`. There are roughly log(x) digits in x.<br>
Space: `O(1)`.

```Java
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while(x != 0)
        {
            int pop = x % 10;
            x /= 10;
            if(rev > Integer.MAX_VALUE/10 || (rev == Integer.MAX_VALUE/10 && pop > 7)) return 0;
            if(rev < Integer.MIN_VALUE/10 || (rev == Integer.MIN_VALUE/10 && pop < -8)) return 0;
            rev = rev*10 + pop;
        }
        return rev;
    }
}
```
