## Question
Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.</br>

**Example 1:**
<pre>
Input: haystack = "hello", needle = "ll"
Output: 2
</pre>

**Example 2:**
<pre>
Input: haystack = "aaaaa", needle = "bba"
Output: -1
</pre>

Clarification:

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().
## Thinking
Be aware to compare needle as a whole string instead of seperate chars while comparing.</br>
## Coding
Time: O(n); Go through the array once. </br>
Space: O(1)
Percent: 90.4%
```python3
class Solution:
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        
        index = -1
        if len(needle) == 0:
            return 0
        elif len(haystack) == 0 or len(needle) == 0:
            return index
        for i in range(len(haystack)-len(needle)+1):
            if haystack[i:i+len(needle)] == needle:
                return i
                
        return -1
```

