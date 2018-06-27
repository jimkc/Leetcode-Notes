## Question
Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.

**Example :**
<pre>
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

Input: S = "3z4"
Output: ["3z4", "3Z4"]

Input: S = "12345"
Output: ["12345"]
</pre>

Note:<br>

S will be a string with length at most 12.<br>
S will consist only of letters or digits.<br>

## Thinking
When solving this kind of problems, try to think from the smallest cases.

## Coding
Time: O(2^n * n);  </br>
Space: O(n).
```python3
class Solution:
    
    def letterCasePermutation(self, S):
        """
        :type S: str
        :rtype: List[str]
        """
        
        if len(S) == 0:
            return ['']
        elif len(S) == 1:
            if S.isalpha():
                return [S.upper(), S.lower()]
            else:
                return [S]
            
        ans = []
        permutations = self.letterCasePermutation(S[1:])
        for per in permutations:
            if S[0].isalpha():
                ans.append(S[0].upper()+per)
                ans.append(S[0].lower()+per)
            else:
                ans.append(S[0]+per)
        return ans
```

