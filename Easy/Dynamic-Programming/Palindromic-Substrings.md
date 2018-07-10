## Question
Given a string, your task is to count how many palindromic substrings in this string.<br>

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

**Example :**   
<pre>
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".

Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
<pre>

## Thinking

## Coding
Time: O(n^2);  </br>
Space: O(n^2)
```python3
class Solution:
    def countSubstrings(self, s):
        """
        :type s: str
        :rtype: int
        """
        if not s:
            return 0
        
        l = len(s)
        cnt = 0 # Nums of palindromic substring
        
        # table[i][j] means that from s[i] to s[j] is palindrome or not
        table = [ [False for i in range(l)] for j in range(l) ]
        
        # Every char is palindrome
        for i in range(l):
            table[i][i] = True
            cnt += 1
            
        # Window size = 2
        for i in range(l-1):
            if s[i] == s[i+1]:
                table[i][i+1] = True
                cnt += 1
            else:
                table[i][i+1] = False
        
        # Window size = 3 ~ l
        for k in range(3,l+1):
            for i in range(l-k+1): # i stands for start of the window
                j = i+k-1          # j is the end of window
                if table[i+1][j-1] and s[i] == s[j]: #
                    table[i][j] = True
                    cnt += 1
        return cnt
```
