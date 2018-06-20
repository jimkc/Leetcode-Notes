## Question
Write an algorithm to determine if a number is "happy".<br>

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.<br>

**Example :**
<pre>
Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
</pre>


## Thinking
When num appears second time it is a loop.

## Coding
Time: O();. </br>
Space: O().
```python3
class Solution:
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        seen = set()
        sqrs = -1
        num = n
        while sqrs not in seen:
            seen.add(sqrs)
            sqrs = 0
            for item in list(str(num)):
                a = int(item)
                sqrs += a*a
            
            if sqrs == 1:
                return True
            num = sqrs
            
            
        return False
```

