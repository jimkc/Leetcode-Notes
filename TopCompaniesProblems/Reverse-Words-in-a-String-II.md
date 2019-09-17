## Question
Given an input string , reverse the string word by word. 

**Example :**   
<pre>
Input:  ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]
</pre>

Note: 

A word is defined as a sequence of non-space characters.<br>
The input string does not contain leading or trailing spaces.<br>
The words are always separated by a single space.<br>
Follow up: Could you do it in-place without allocating extra space?

## Thinking
    

## Coding
Time: O(n); <br>
Space: O(1)
```python3
class Solution(object):
    def reverseWords(self, str):
        """
        :type str: List[str]
        :rtype: void Do not return anything, modify str in-place instead.
        """
        
        str.reverse() # Cant use str = str[::-1] wont change original one
        i = 0
        
        while i < len(str):
            j = i
            while j < len(str) and str[j] != ' ' :
                j+=1
            str[i:j] = str[i:j][::-1]
            i = j+1

```

