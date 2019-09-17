## Question
Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Example :**   
<pre>
Input: 121
Output: true

Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
</pre>

Follow up:<br>

Coud you solve it without converting the integer to a string?

## Thinking


## Coding
Time: O(1);<br>
Space: O(1)
```python3
class Solution:
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        
        s = str(x)
        mid = len(s)//2
        
        if len(s) % 2 == 0:
            if s[:mid] == s[mid:][::-1]:
                return True
            else:
                return False
        else:
            if s[:mid] == s[mid+1:][::-1]:
                return True
            else:
                return False
```

