## Question
Write a function to find the longest common prefix string amongst an array of strings.<br>

If there is no common prefix, return an empty string "".<br>

**Example :**
<pre>
Input: ["flower","flow","flight"]
Output: "fl"

Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
</pre>

Note:

All given inputs are in lowercase letters a-z.

## Thinking


## Coding
Time: O(S*W); S is the length of S and W is the length of words. </br>
Space: O(W) 
```python3
class Solution:
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        comn_pre = ''
        
        if len(strs) == 0:
            return comn_pre
        length = min(len(x) for x in strs)
        tmp = strs[0]
        
        for i in range(length):
            for j in range(len(strs)-1):
                if strs[j][i] != strs[j+1][i]:
                    return comn_pre
            comn_pre += strs[0][i]
        return comn_pre
                            
        
```

