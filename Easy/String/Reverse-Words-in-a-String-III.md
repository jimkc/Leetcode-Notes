## Question
Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.<br>

**Example :**
<pre>
Input: "Let's take LeetCode contest"
Output: "s'teL ekat edoCteeL tsetnoc"
</pre>

Note: In the string, each word is separated by single space and there will not be any extra space in the string.

## Thinking


## Coding
Time: O(n); Go through the array twice. </br>
Space: O(n) 
```python3
class Solution:
    def reverseWords(self, s):
        """
        :type s: str
        :rtype: str
        """
        if s == '':
            return ''
        ans = ''
        token = s.split()
        for i in range(len(token)):
            token[i] = token[i][::-1]
        
        for i  in range(len(token)-1):
            ans += token[i] + ' '
        ans += token[-1]
            
        return ans
```

