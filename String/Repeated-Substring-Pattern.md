## Question
Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

**Example :**
<pre>
Input: "abab"

Output: True

Explanation: It's the substring "ab" twice.
</pre>

**Example :**
<pre>
Input: "aba"

Output: False
</pre>

**Example :**
<pre>
Input: "abcabcabcabc"

Output: True

Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)
</pre>

## Thinking


## Coding
Time: O(n^2); Go through s once and compare substring for each index takes n too. </br>
Space: O(1) 
```python3
class Solution:
    def repeatedSubstringPattern(self, s):
        """
        :type s: str
        :rtype: bool
        """
        for i in range(1, int(len(s)/2)+1):
            if len(s) % i == 0:
                if int(len(s)/i) * s[:i] == s:
                    return True
        return False
```

