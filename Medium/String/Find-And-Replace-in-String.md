## Question
To some string S, we will perform some replacement operations that replace groups of letters with new ones (not necessarily the same size).<br>

Each replacement operation has 3 parameters: a starting index i, a source word x and a target word y.  The rule is that if x starts at position i in the original string S, then we will replace that occurrence of x with y.  If not, we do nothing.<br>

For example, if we have S = "abcd" and we have some replacement operation i = 2, x = "cd", y = "ffff", then because "cd" starts at position 2 in the original string S, we will replace it with "ffff".<br>

Using another example on S = "abcd", if we have both the replacement operation i = 0, x = "ab", y = "eee", as well as another replacement operation i = 2, x = "ec", y = "ffff", this second operation does nothing because in the original string S[2] = 'c', which doesn't match x[0] = 'e'.<br>

All these operations occur simultaneously.  It's guaranteed that there won't be any overlap in replacement: for example, S = "abc", indexes = [0, 1], sources = ["ab","bc"] is not a valid test case.


**Example :**   
<pre>
Input: S = "abcd", indexes = [0,2], sources = ["a","cd"], targets = ["eee","ffff"]
Output: "eeebffff"
Explanation: "a" starts at index 0 in S, so it's replaced by "eee".
"cd" starts at index 2 in S, so it's replaced by "ffff".

Input: S = "abcd", indexes = [0,2], sources = ["ab","ec"], targets = ["eee","ffff"]
Output: "eeecd"
Explanation: "ab" starts at index 0 in S, so it's replaced by "eee". 
"ec" doesn't starts at index 2 in the original S, so we do nothing.
</pre>

Notes:

0 <= indexes.length = sources.length = targets.length <= 100
0 < indexes[i] < S.length <= 1000
All characters in given inputs are lowercase letters.

## Thinking
1. The first method traverse from left to right, and remember how may index to shift. The second is better from right to left.<br> 
2. The second runs every item in zip without unzipping, faster.

## Coding
Time: O(n+m); n is the length of string, m is the len of source </br>
Space: O(1)
```python
class Solution:
    def findReplaceString(self, S: str, indexes: List[int], sources: List[str], targets: List[str]) -> str:
        
        add_index = 0
        
        zipped = zip(indexes, sources, targets)
        zipped = sorted(zipped, key=lambda t:t[0])
        
        indexes, sources, targets = zip(*zipped)
    
        
        for i in range(len(indexes)):
            ori_idx = indexes[i]
            source_str = sources[i]
            target_str = targets[i]
            
            idx = ori_idx+add_index

            original = S[idx:idx+len(source_str)]            
            
            if original == source_str:
                S = S[:idx]+target_str+S[idx+len(original):]
                add_index += len(target_str) - len(original)
        return S
            
```

better one:
```python
def findReplaceString(self, S, indexes, sources, targets):
        for i, s, t in sorted(zip(indexes, sources, targets), reverse=True):
            S = S[:i] + t + S[i + len(s):] if S[i:i + len(s)] == s else S
        return S
```

