## Question
Write a function that takes a string as input and reverse only the vowels of a string.<br>

**Example :**
<pre>
Example 1:
Given s = "hello", return "holle".

Example 2:
Given s = "leetcode", return "leotcede".
</pre>

Note:
The vowels does not include the letter "y".

## Thinking


## Coding
Time: O(N); Go through the word once. </br>
Space: O(n) 
```python3
class Solution:
    def reverseVowels(self, s):
        """
        :type s: str
        :rtype: str
        """
        vowels = {'a','e','i','o','u','A','E','I','O','U'}
        fnd = []
        for i,char in enumerate(s) :
            if char in vowels:
                fnd.append((i,char))
        new = list(s)
        for i in range(len(fnd)):
            new[fnd[i][0]] = fnd[len(fnd)-1-i][1]
        return ''.join(new)
        
```

