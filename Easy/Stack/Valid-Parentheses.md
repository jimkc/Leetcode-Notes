## Question
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.<br>

An input string is valid if:<br>
    Open brackets must be closed by the same type of brackets.<br>
    Open brackets must be closed in the correct order.<br>

Note that an empty string is also considered valid.<br>

**Example :**
<pre>
Input: "()"
Output: true

Input: "()[]{}"
Output: true

Input: "(]"
Output: false

Input: "([)]"
Output: false

Input: "{[]}"
Output: true
</pre>


## Thinking
Use stack to set up the rules.

## Coding
Time: O(S); S is the length of S. </br>
Space: O(S) 
```python3
class Solution:
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        symbols = [] 
        pre = 0
        point = 0
        left_b = {'{','[','('}
        right_b = {'}':'{', ']':'[', ')':'('}
        
        for char in s:
            if char in left_b :
                symbols.append(char)
            else:
                if len(symbols) > 0:
                    if symbols[-1] == right_b[char]:
                        symbols.pop()
                    else:
                        return False
                else:
                    return False
        if len(symbols) == 0:
            return True
        else:
            return False
```

