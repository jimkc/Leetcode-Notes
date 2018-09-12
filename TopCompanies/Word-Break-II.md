## Question
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.<br>

Note:<br>

The same word in the dictionary may be reused multiple times in the segmentation.<br>
You may assume the dictionary does not contain duplicate words.<br>

**Example :**   
<pre>
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]

Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.

Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
</pre>

## Thinking


## Coding
Time: O();
Space: O(n)
```python3
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: Set[str]
        :rtype: List[str]
        """
        return self.helper(s, wordDict, {})

    def helper(self, s, wordDict, memo):
        if s in memo: # Dont do the same things again
            return memo[s]
        if not s: return []
        
        
        res = [] 
        
        for word in wordDict:
            if not s.startswith(word):
                continue
            if len(word) == len(s): 
                res.append(word)
            else:
                resultOfTheRest = self.helper(s[len(word):], wordDict, memo) #Combination of the rest of the words as a list
                for item in resultOfTheRest:
                    item = word + ' ' + item
                    res.append(item)
        memo[s] = res # Record all possible combinations for s
        return res
```

