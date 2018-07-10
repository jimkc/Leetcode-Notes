## Question
Given a non-empty string s and an abbreviation abbr, return whether the string matches with the given abbreviation.<br>

A string such as "word" contains only the following valid abbreviations:<br>
<pre>
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
</pre>

Notice that only the above abbreviations are valid abbreviations of the string "word". Any other string is not a valid abbreviation of "word".<br>

Note:<br>
Assume s contains only lowercase letters and abbr contains only lowercase letters and digits.<br>

**Example :**
<pre>
Given s = "internationalization", abbr = "i12iz4n":

Return true.
</pre>

**Example :**
<pre>
Given s = "apple", abbr = "a2e":

Return false.
</pre>

## Thinking
The technique if add '0' first saves us many judgments.

## Coding
Time: O(N); N is the length of abbr. </br>
Space: O(N).
```python3
class Solution:
    def validWordAbbreviation(self, word, abbr):
        """
        :type word: str
        :type abbr: str
        :rtype: bool
        """
        cnt = 0
        digit_num = ''
        
        for i,char in enumerate(abbr):
            if char.isalpha():    
                cnt += 1+int('0' + digit_num)
                if cnt > len(word) or word[cnt-1] != char:
                        return False
                digit_num = ''
            else:
                if digit_num == '' and char == '0':
                    return False
                digit_num += char

        return len(word) == (cnt + int('0' + digit_num))
                
```

