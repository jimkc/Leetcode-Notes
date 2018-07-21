## Question
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

**Example :**   
<pre>
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.

Input: "cbbd"
Output: "bb"
</pre>

## Thinking


## Coding
Time: O(n^2) Compareing the words takes O(n); <br>
Space: O(1)
```python3
class Solution:
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """

        start_point = 0
        max_len = 1

        for i in range(len(s)):
            if i-max_len > 0 and s[i-max_len-1:i+1] == s[i-max_len-1:i+1][::-1]: # [ +, max_length, (i) ] add 2 to max_length
                start_point = i - max_len - 1
                max_len += 2

                
            if i-max_len >= 0 and s[i-max_len:i+1] == s[i-max_len:i+1][::-1]: # [ max_length, (i)]
                start_point = i - max_len
                max_len += 1
                

        return s[start_point : start_point + max_len]
```
