## Question
Given a string s, partition s such that every substring of the partition is a palindrome.<br>

Return all possible palindrome partitioning of s.

**Example :**   
<pre>
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
</pre>

## Thinking


## Coding
Time: O(n!);?<br>
Space: O(s)
```python3
class Solution:
    def partition(self, s):
        res = [] # All substrings that is palindrome
        self.dfs(s, [], res)
        return res

    def dfs(self, s, pal_subs, res):
        if not s:
            res.append(pal_subs)
            return
        for i in range(1, len(s)+1):
            # Dont worry if s[:i]'s sub is not check, because iter i iterates through each idx, the left part will be checked and               the right will be iter again in the dfs
            # Every time we only check the left part and leave the right to the dfs to slice and check the left again
            # No worry for the left is not palindrome and the right part is, because at least a single char will be a pal then                 the right will be throw in the dfs 
            if self.isPal(s[:i]): 
                self.dfs(s[i:], pal_subs+[s[:i]], res)

    def isPal(self, s):
        return s == s[::-1]
```

