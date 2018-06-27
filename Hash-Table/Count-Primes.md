## Question
Count the number of prime numbers less than a non-negative number, n.

**Example :**
<pre>
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
</pre>

## Thinking
Whenever we visit a number we change all multiple times of itself as False in the true table.<br>
The reason we do this until n^0.5 is beacause all numbers k can be factorized to be q*p
and one of p,q will always be smaller than n^0.5

## Coding
Time: O(n); At most visit all number once </br>
Space: O(n).
```python3
class Solution:
    def countPrimes(self, n):
        """
        :type n: int
        :rtype: int
        """   
        
        if n <= 2:
            return 0

        trueTable = [True]*n
        trueTable[0:2] = [False]*2
        
        for i in range(2,int(n**0.5)+1): 
            #print (i,trueTable)
            if trueTable[i] == True:
                if i*2 < n:
                    trueTable[i*2:n:i] = [False]*(int((n-1-i*2)/i) +1)
        return sum(trueTable)
```

