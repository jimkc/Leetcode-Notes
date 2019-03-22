## Question
You are given a license key represented as a string S which consists only alphanumeric character and dashes. The string is separated into N+1 groups by N dashes.<br>

Given a number K, we would want to reformat the strings such that each group contains exactly K characters, except for the first group which could be shorter than K, but still must contain at least one character. Furthermore, there must be a dash inserted between two groups and all lowercase letters should be converted to uppercase.<br>

Given a non-empty string S and a number K, format the string according to the rules described above.
**Example :**   
<pre>
Input: S = "5F3Z-2e-9-w", K = 4

Output: "5F3Z-2E9W"

Explanation: The string S has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.

Input: S = "5F3Z-2e-9-w", K = 4

Output: "5F3Z-2E9W"

Explanation: The string S has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.
</pre>

## Thinking
Be aware of corner cases.

## Coding
Time: O(n); </br>
Space: O(n)
```python
class Solution:
    def licenseKeyFormatting(self, S: str, K: int) -> str:
        
        
        
        seq = "".join(S.split("-")).upper()
        if (K>len(seq)):
            return seq
        
        
        ans = []
        idx = 0
        len1 = len(seq)%K
        
        # prevent corner case
        if len1 != 0:
            ans.append(seq[:len1])
            idx = len1
        
        while(idx < len(seq)):
            ans.append(seq[idx:idx+K])
            idx+=K
        return "-".join(ans)
            
```

