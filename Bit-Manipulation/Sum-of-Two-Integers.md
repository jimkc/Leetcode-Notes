## Question
Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

**Example :**
<pre>
Given a = 1 and b = 2, return 3.
</pre>

## Thinking
For python the add function only works when :<br>
    a*b>=0 , or<br>
    a < 0 and abs(a) > b > 0 (the negative number has a larger absolute value)<br>
    b < 0 and abs(b) > a > 0<br>
Modify it accordingly (use add(~n, 1) to turn n into -n):<br>

## Coding
Time: O();  </br>
Space: O().
```python3
class Solution:
    def getSum(self, a, b):
        def add(a, b): 
            if not a or not b:
                return a or b # every round we save the carry bit in b, we need it to be 0 at last
            return add(a^b, (a&b) << 1) # a^b returns the add bits, but not adding carry bits, (a&b)<<1 return carry bits that                                              should be added

        if a*b < 0: # assume a < 0, b > 0
            if a > 0:
                return self.getSum(b, a)
            if add(~a, 1) == b: # -a == b , ~a returns -a-1
                return 0
            if add(~a, 1) < b: # -a < b
                return add(~add(add(~a, 1), add(~b, 1)),1) # -add(-a, -b)

        return add(a, b) # a*b >= 0 or (-a) > b > 0 
```
