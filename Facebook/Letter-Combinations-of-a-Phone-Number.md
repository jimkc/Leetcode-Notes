## Question
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.<br>

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

**Example :**   
<pre>
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
</pre>

## Thinking

## Coding
Time: O();<br>
Space: O()
```python3
class Solution:
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        self.dic = {'2':['a','b','c'],
                    '3':['d','e','f'],
                    '4':['g','h','i'],
                    '5':['j','k','l'],
                    '6':['m','n','o'],
                    '7':['p','q','r','s'],
                    '8':['t','u','v'],
                    '9':['w','x','y','z'] }
        
        
        def combine(dig): # While passing list it in recurrsion, it is an object, so it will be modify from anywhere
            
            if not dig:
                return ''
            
            n = dig[0]
            ans = []
            words = combine(dig[1:])
            
            if words:
                for c in self.dic[n]: 
                    for w in words:
                        ans.append(c+w)
            else:
                for c in self.dic[n]:
                    ans.append(c)

            return ans
        
        if not digits:
            return []
        else:
            return combine(digits)
        
```
