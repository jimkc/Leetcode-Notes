## Question
Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

**Example :**   
<pre>
Input: "bcabc"
Output: "abc"


Input: "cbacdcbc"
Output: "acdb"
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def removeDuplicateLetters(self, s):
        """
        :type s: str
        :rtype: str
        """
        cnt = collections.Counter(s)
        stack = []
        seen = set() # Record if the char is in stack
        
        for i in range(len(s)):
            
            # If s[i] is already in seen, it means the previous char obeys the rules already
            while stack and s[i] not in seen:
                if s[i] <= stack[-1] and cnt[stack[-1]] > 1:
                    cnt[stack[-1]] -= 1
                    seen.remove(stack[-1])
                    stack.pop()
                    
                else:
                    break
            
            if s[i] not in seen:
                stack.append(s[i])
                seen.add(s[i])
            else:
                cnt[s[i]] -= 1 # Discard the current one
                
        return ''.join(stack)
```

