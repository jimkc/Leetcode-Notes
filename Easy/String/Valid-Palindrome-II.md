## Question
Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.<br>

**Example :**
<pre>
Input: "aba"
Output: True

Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
</pre>

Note:
The string will only contain lowercase characters a-z. The maximum length of the string is 50000.


## Thinking
Compare word parts with slice is faster than compareing chars one by one.

## Coding
Time: O(S); S is the length of s. </br>
Space: O(1) 
```python3
class Solution:
    def validPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if s == s[::-1]:
            return True
        for i in range(len(s)-1):
            if s[i] != s[-i-1]:
                return s[i: -i-2] == s[i+1: -i-1][::-1] or s[i+1: -i-1] == s[i+2: len(s)-i][::-1]
        return True
```

