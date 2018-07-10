## Question
Given a pattern and a string str, find if str follows the same pattern.<br>

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.<br>

**Example :**
<pre>
Input: pattern = "abba", str = "dog cat cat dog"
Output: true

Input:pattern = "abba", str = "dog cat cat fish"
Output: false

Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false

Input: pattern = "abba", str = "dog dog dog dog"
Output: false
</pre>

Notes:<br>
You may assume pattern contains only lowercase letters, and str contains lowercase letters separated by a single space.


## Thinking


## Coding
Time: O(p); p for char in pattern s for words in str </br>
Space: O(p+s).
```python3
class Solution:
    def wordPattern(self, pattern, str):
        """
        :type pattern: str
        :type str: str
        :rtype: bool
        """
        words = str.split(' ')
        pair = {}
        seen = set() #Check the words in str that has already been paired with a char in pattern
        
        if len(pattern) != len(words):
            return False
        
        for i in range(len(pattern)):
            p = pattern[i]
            c = words[i]
            if p not in pair:
                if c in seen:
                    return False
                else:
                    pair[p] = c
                    seen.add(c)
            else:
                if pair[p] != c:
                    return False
        return True
```

