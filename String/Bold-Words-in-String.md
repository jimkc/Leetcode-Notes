## Question
Given a set of keywords words and a string S, make all appearances of all keywords in S bold. Any letters between <b> and </b> tags become bold.

The returned string should use the least number of tags possible, and of course the tags should form a valid combination.

**Example :**
<pre>
For example, given that words = ["ab", "bc"] and S = "aabcd", we should return "a<b>abc</b>d". Note that returning "a<b>a<b>b</b>c</b>d" would use more tags, so it is incorrect.
</pre>

Note:

words has length in range [0, 50].
words[i] has length in range [1, 10].
S has length in range [0, 500].
All characters in words[i] and S are lowercase letters.


## Thinking
Merge interval concept.

## Coding
Time: O(S*W); S is the length of S and W is the length of words. </br>
Space: O(S) 
```python3
class Solution:
    def boldWords(self, words, S):
        """
        :type words: List[str]
        :type S: str
        :rtype: str
        """
        intervals = []
        new_inter = []
        ans = []
        
        for i in range(len(S)):
            for w in words:
                if S[i] == w[0]:
                    if S[i:i+len(w)] == w:
                        intervals.append((i,i+len(w)-1))
        if not intervals:
            return S
        
        
        pre_start = intervals[0][0]
        pre_end = intervals[0][1]
        new_inter.append((pre_start, pre_end))
        
        for i in range(len(intervals)-1):
            if pre_start <= intervals[i+1][0] and intervals[i+1][0] <= pre_end+1:
                new_inter[-1] = (pre_start, max(pre_end, intervals[i+1][1]))
            else:
                new_inter.append((intervals[i+1][0], intervals[i+1][1]))

            pre_start = new_inter[-1][0]
            pre_end = new_inter[-1][1]
            
        start = 0
        for intvl in  new_inter:
            ans.append(S[start:intvl[0]])
            ans.append('<b>')
            ans.append(S[intvl[0]:intvl[1]+1])
            ans.append('</b>')
            start = intvl[1]+1
            
        ans.append(S[new_inter[-1][1]+1:])
        
        return ''.join(ans)
        
        
```

