## Question
Implement pow(x, n), which calculates x raised to the power n (xn).

**Example :**   
<pre>
Input: 2.00000, 10
Output: 1024.00000

Input: 2.10000, 3
Output: 9.26100

Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
</pre>


Note:<br>

-100.0 < x < 100.0<br>
n is a 32-bit signed integer, within the range [−231, 231 − 1]


## Thinking
Decompose n to be n = a*b*c....., in which a,b,c are all powers of 2, so we
can lower the complexity to be logn.


## Coding
Time: O(logn); <br>
Space: O(logn)
```python3
class Solution:
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        
        if n < 0:
            x = 1/x
            n = -n
        record = [] # Record each value of x in power of 2's multiple
        record.append((1,0))
        record.append((x,1))
        
        
        def cnt(x,k,record):
                
            k = k*2 
            if k > n:
                return None
            
            x = x*x
            record.append((x,k))
            cnt(x,k,record)
               
        cnt(x,1,record)
        
        ans = 1
        # Decompose n to multiple numbers a*b*c... = n
        while n != 0:
            while n < record[-1][1]: # The last power in record is too big to minus, so we try the smaller one
                record.pop()          
            n -= record[-1][1]
            ans*=record[-1][0] 
            
        
        return ans
```

