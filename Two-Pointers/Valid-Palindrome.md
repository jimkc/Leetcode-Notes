## Question
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Note: For the purpose of this problem, we define empty string as valid palindrome.

**Example 1:**
<pre>
Input: "A man, a plan, a canal: Panama"
Output: true
</pre>

**Example 2:**
<pre>
Input: "race a car"
Output: false
</pre>

## Thinking
Another method by using two pointers is in Liinked-List/Palindrome-Linked-List.md
## Coding
Time: O(n);(not sure about complexity of regex.sub)
Space: O(1) 
```python3
import re
class Solution:
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        s = s.lower()
        s = re.sub('[^a-zA-Z0-9]','',s)
        return s == s[::-1]
```

