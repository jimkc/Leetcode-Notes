## Question
A string S of lowercase letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

**Example :**   
<pre>
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
</pre>

Note:<br>

S will have length in range [1, 500].<br>
S will consist of lowercase letters ('a' to 'z') only.

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def partitionLabels(self, S):
        
        last = {c: i for i, c in enumerate(S)} # Record the last index of c that appears in S
        j = start = 0
        ans = []
        
        for i, c in enumerate(S):
            j = max(j, last[c]) 
            if i == j:
                ans.append(i - start + 1)
                start = i + 1 # reset start of partition
            
        return ans
```

