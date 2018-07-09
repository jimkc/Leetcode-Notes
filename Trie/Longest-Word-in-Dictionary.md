## Question
Given a list of strings words representing an English Dictionary, find the longest word in words that can be built one character at a time by other words in words. If there is more than one possible answer, return the longest word with the smallest lexicographical order.<br>

If there is no answer, return the empty string.

**Example :**   
<pre>
Input: 
words = ["w","wo","wor","worl", "world"]
Output: "world"
Explanation: 
The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".

Input: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
Output: "apple"
Explanation: 
Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".
<pre>


Note:<br>

All the strings in the input will only contain lowercase letters.<br>
The length of words will be in the range [1, 1000].<br>
The length of words[i] will be in the range [1, 30].<br>

## Thinking


## Coding
Time: O(nlogn) n is number of words;  </br>
Space: O(n) 
```python3
class Solution:
    def longestWord(self, words):
        """
        :type words: List[str]
        :rtype: str
        """
        seen = set()
        lw = '' # Longest word
        
        words = sorted(words) # With length and lexicographical order
        print (words)
        
        for w in words:
            if len(w) == 1 or w[:-1] in seen:
                seen.add(w)
                if len(w) > len(lw):  
                    lw = w
        return lw
```

