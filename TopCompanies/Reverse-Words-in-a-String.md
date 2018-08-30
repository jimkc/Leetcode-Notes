## Question
Given an input string, reverse the string word by word.

**Example :**   
<pre>
Input: "the sky is blue",
Output: "blue is sky the".
</pre>

Note:<br>

A word is defined as a sequence of non-space characters.<br>
Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.<br>
You need to reduce multiple spaces between two words to a single space in the reversed string.<br>
Follow up: For C programmers, try to solve it in-place in O(1) space.

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution(object):
    def reverseWords(self, s):
        """
        :type s: str
        :rtype: str
        """
        
        
        string = s.split()
        return ' '.join(string[::-1])
                
```

