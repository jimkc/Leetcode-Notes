## Question
Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.<br>

Each letter in the magazine string can only be used once in your ransom note.

**Example :**
<pre>
anConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
</pre>

Note:
You may assume that both strings contain only lowercase letters.

## Thinking


## Coding
Time: O(R+M+R); Add items to collection takes O(R+M). </br>
Space: O(R+M).
```python3
class Solution:
    def canConstruct(self, ransomNote, magazine):
        """
        :type ransomNote: str
        :type magazine: str
        :rtype: bool
        """
        cnt_ransom = collections.Counter(ransomNote)
        cnt_mag = collections.Counter(magazine)
        
        for tup in cnt_ransom.most_common():
            if tup[1] > cnt_mag[tup[0]]:
                return False
        return True
```

