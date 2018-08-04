## Question
S and T are strings composed of lowercase letters. In S, no letter occurs more than once.<br>

S was sorted in some custom order previously. We want to permute the characters of T so that they match the order that S was sorted. More specifically, if x occurs before y in S, then x should occur before y in the returned string.<br>

Return any permutation of T (as a string) that satisfies this property.

**Example :**   
<pre>
Input: 
S = "cba"
T = "abcd"
Output: "cbad"
Explanation: 
"a", "b", "c" appear in S, so the order of "a", "b", "c" should be "c", "b", and "a". 
Since "d" does not appear in S, it can be at any position in T. "dcba", "cdba", "cbda" are also valid outputs.
</pre>

Note:<br>

S has length at most 26, and no character is repeated in S.<br>
T has length at most 200.<br>
S and T consist of lowercase letters only.

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def customSortString(self, S, T):
        """
        :type S: str
        :type T: str
        :rtype: str
        """
        dic_s = collections.Counter(S)
        dic_t = collections.Counter(T)
        ans = ''
        
        for c in S:
            if c in dic_t:
                ans+=c*dic_t[c]
                dic_t.pop(c)
                
        
                
        
        for k,v in dic_t.items():
            ans += k*v
        return ans
```

