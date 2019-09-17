## Question
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

**Example :**   
<pre>
For example, given n = 3, a solution set is:

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
</pre>

## Thinking


## Coding
Time: O(); <br>
Space: O(n)
```python3
class Solution:
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        
        stack = 0 # Represents the number of '(' that is not paired
        ans = []
        self.dfs('',n,n,stack,ans)
        return ans
    
    # l,r are '(' and ')' that haven't been put to s 
    def dfs(self,s,l,r,stack,ans):
        
        if l == 0 and r == 0:
            ans.append(s)
            return None
        
        if l > 0: self.dfs(s+'(',l-1,r,stack+1,ans) 
        if r > 0 and stack > 0: self.dfs(s+')',l,r-1,stack-1,ans) # stack-1 for '(' being paired by one ')'
```

