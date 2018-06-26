## Question
Given two strings s and t, determine if they are isomorphic.<br>

Two strings are isomorphic if the characters in s can be replaced to get t.<br>

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself<br>

**Example :**
<pre>
Input: s = "egg", t = "add"
Output: true

Input: s = "foo", t = "bar"
Output: false

Input: s = "paper", t = "title"
Output: true
</pre>

Note:
You may assume both s and t have the same length.

## Thinking
The characters in s may not map the same character in t, so we use a set to record.

## Coding
Time: O(n); </br>
Space: O(n).
```python3
class Solution:
    def isIsomorphic(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        pair = {}
        seen = set()
        for i in range(len(s)):
            if s[i] not in pair:
                if t[i] in seen:
                    return False
                else:
                    pair[s[i]] = t[i]
                    seen.add(t[i])
            else:
                if pair[s[i]] != t[i]:
                    return False
        return True
```

