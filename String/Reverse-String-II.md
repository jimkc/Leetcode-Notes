## Question
Given a string and an integer k, you need to reverse the first k characters for every 2k characters counting from the start of the string. If there are less than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and left the other as original.

**Example :**
<pre>
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
</pre>

Restrictions:
The string consists of lower English letters only.
Length of the given string and k will in the range [1, 10000]

## Thinking


## Coding
Time: O(n); Go through the word once. </br>
Space: O(n) 
```python3
class Solution:
    def reverseStr(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: str
        """
        ans = []
        
        def rev(s):
            return s[::-1]
        
        for i in range(0,len(s),2*k):
            if len(s) > i+k :
                ans.append(rev(s[i:i+k]))
                if len(s) > i+2*k:
                    ans.append(s[i+k:i+2*k])
                else:
                    ans.append(s[i+k:])
            else:
                ans.append(rev(s[i:]))
        return "".join(ans)

```

