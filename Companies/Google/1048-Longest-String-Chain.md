## Question
Given a list of words, each word consists of English lowercase letters.<br>
<br>
Let's say word1 is a predecessor of word2 if and only if we can add exactly one letter anywhere in word1 to make it equal to word2.  For example, "abc" is a predecessor of "abac".<br>
<br>
A word chain is a sequence of words [word_1, word_2, ..., word_k] with k >= 1, where word_1 is a predecessor of word_2, word_2 is a predecessor of word_3, and so on.<br>
<br>
Return the longest possible length of a word chain with words chosen from the given list of words.</br>

**Example :**   
<pre>
Input: ["a","b","ba","bca","bda","bdca"]
Output: 4
Explanation: one of the longest word chain is "a","ba","bda","bdca".
</pre>

Note:<br>
<br>
1 <= words.length <= 1000<br>
1 <= words[i].length <= 16
words[i] only consists of English lowercase letters.

## Thinking


## Coding
Time: O(); sort takes nlog(n), </br>
Space: O()
```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        
        dp = {} # the predecessor word chain length
        
        # key=len is important, otherwise sort will use alphabatical order to sort
        words.sort(key=len)
        
        for w in words:
            predecessors = [dp.get(w[:i]+w[i+1:],0) for i in range(len(w))]
            # only keep the predecessor that has longest word chain
            dp[w] = max(predecessors)+1 
            
        return max(dp.values())
```

