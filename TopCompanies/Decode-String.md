## Question
Given an encoded string, return it's decoded string.<br>

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.<br>

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.<br>

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

**Example :**   
<pre>
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
</pre>

## Thinking


## Coding
Time: O(n);<br>
Space: O(n)
```python3
class Solution:
    def decodeString(self, s):
        """
        :type s: str
        :rtype: str
        """
        
        stack = []
        stack.append(["", 1])
        num = ""
        for ch in s:
            if ch.isdigit():
                num += ch
            elif ch == '[':
                stack.append(["", int(num)])
                num = ""
            elif ch == ']':
                st, k = stack.pop()
                stack[-1][0] += st*k
            else:
                stack[-1][0] += ch
        return stack[0][0]        
```

