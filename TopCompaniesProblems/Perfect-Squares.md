## Question
Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

**Example :**   
<pre>
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.

Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
</pre>

## Thinking
BFS for each possible sqrt, each level means that we minus one possible sqrt.

## Coding
Time: O();<br>
Space: O(n)
```python3
class Solution:
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        
        toCheck = [n]
        cnt = 0
        
        while toCheck:
            cnt += 1 # Because we minus one sqrt so every num in the next toCheck have used one perfect sqr num
            tmp = []
            
            for num in toCheck:
                intSqrt = int(num**0.5) # The largest possible sqrt
                
                if intSqrt**2 == num:   
                    return cnt
                
                for sqrt in range(intSqrt,0,-1): 
                    remain = num-sqrt**2
                    if self.isSquare(remain): 
                        return cnt+1
                    else:
                        tmp.append(remain) # The next level of BST 
            toCheck = tmp
            
        return cnt        
                    
    def isSquare(self,n):
        a = int(n**0.5)
        if a**2 == n:
            return True
        else:
            return False
```

DP but TLE
```python3
class Solution:
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        
        dp = [i for i in range(n+1)]
        squares = [i**2 for i in range(1, int(n**0.5)+1)]
        
        for i in range(1,n+1):
            for sqr in squares:
                if i < sqr:
                    break
                
                dp[i] = min(dp[i], dp[i-sqr] + 1) # Split i in to two num i-sqr and sqr , look up i-sqr in the dp table
        return dp[n]
```

