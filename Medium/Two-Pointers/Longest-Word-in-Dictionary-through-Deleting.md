## Question
Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.<br>

**Example :**   
<pre>
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output: 
"apple"

Input:
s = "abpcplea", d = ["a","b","c"]

Output: 
"a"
</pre>

## Thinking
Because it is contained string not substring, we cant use "in", so use two pointers to match.

## Coding
Time: O(nm); n is the length of s, and m is the length of d </br>
Space: O(n)
```python
from collections import defaultdict
class Solution:
    def findLongestWord(self, s: str, d: List[str]) -> str:
        cnt = defaultdict(list)
        
        # if word contains in s, add len(word) as key and append it to value
        for word in d:
            if self.has_match(s, word):
                cnt[len(word)].append(word)
        
        if not cnt:
            return ""
        
        # get the longest matches
        max_len = max(cnt.keys())
        
        # return the first one according to lexical order
        return sorted(cnt[max_len])[0]
        
    
    def has_match(self, target, substr):
        t = 0
        s = 0
        
        for i in range(len(target)):
            if target[i] == substr[s]:
                s+=1
                
                if s == len(substr):
                    return True
        
        
        return False
        
        
```

