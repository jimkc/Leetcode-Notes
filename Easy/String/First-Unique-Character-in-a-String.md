## Question
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.<br>

**Example :**
<pre>
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
</pre>

Note: You may assume the string contain only lowercase letters.

## Thinking

## Coding
Time: O(N); O(N) for collection and go throught s once. </br>
Space: O(N).
```python3
class Solution:
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: int
        """
        if s == '':
            return -1
        else:
            cnt = collections.Counter(s)
            for i,char in enumerate(s):
                if cnt[char] == 1:
                    return i
            return -1
```

