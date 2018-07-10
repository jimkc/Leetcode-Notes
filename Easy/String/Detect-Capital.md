## Question
Given a word, you need to judge whether the usage of capitals in it is right or not.<br>

We define the usage of capitals in a word to be right when one of the following cases holds:<br>

All letters in this word are capitals, like "USA".<br>
All letters in this word are not capitals, like "leetcode".<br>
Only the first letter in this word is capital if it has more than one letter, like "Google".<br>
Otherwise, we define that this word doesn't use capitals in a right way.<br>

**Example 1:**
<pre>
Input: "USA"
Output: True
</pre>

**Example 2:**
<pre>
Input: "FlaG"
Output: False
</pre>

Note: The input will be a non-empty word consisting of uppercase and lowercase latin letters.

## Thinking
Dont use the funtion capitalize, instead using index searching is much faster.

## Coding
Time: O(S); Go through the letters once. </br>
Space: O(1) 
```python3
class Solution:
    def detectCapitalUse(self, word):
        """
        :type word: str
        :rtype: bool
        """
        return word.upper() == word or word.lower() == word or (word[0] == word[0].upper() and word[1:].lower() == word[1:])
        
```

