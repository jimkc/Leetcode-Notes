## Question
Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.<br>

This is case sensitive, for example "Aa" is not considered a palindrome here.<br>

Note:<br>
Assume the length of given string will not exceed 1,010.


**Example :**
<pre>
Input:
"abccccdd"

Output:
7

Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
</pre>

## Thinking


## Coding
Time: O(n); . </br>
Space: O(n).
```python3
class Solution:
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        seen = {}
        odd_times = 0
        odd_sum = 0
        even_sum = 0
        
        for c in s:
            if c not in seen:
                seen[c] = 1
            else:
                seen[c] += 1
                
        for k,v in seen.items():
            if v % 2 == 0:
                even_sum += v #Even numbers of char can promise to be part of the palindrome
            else:
                odd_sum += v 
                odd_times += 1
        if odd_times > 0:    
            return odd_sum - odd_times + 1 + even_sum # minus odd_times because we can only take even numbers except the middle one
        else:
            return even_sum
```
