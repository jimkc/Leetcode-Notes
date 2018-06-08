## Question
A sentence S is given, composed of words separated by spaces. Each word consists of lowercase and uppercase letters only.<br>

We would like to convert the sentence to "Goat Latin" (a made-up language similar to Pig Latin.)<br>

The rules of Goat Latin are as follows:<br>

If a word begins with a vowel (a, e, i, o, or u), append "ma" to the end of the word.<br>
For example, the word 'apple' becomes 'applema'.<br>
 
If a word begins with a consonant (i.e. not a vowel), remove the first letter and append it to the end, then add "ma".<br>
For example, the word "goat" becomes "oatgma".<br>
 
Add one letter 'a' to the end of each word per its word index in the sentence, starting with 1.<br>
For example, the first word gets "a" added to the end, the second word gets "aa" added to the end and so on.<br>
Return the final sentence representing the conversion from S to Goat Latin. <br>

**Example 1:**
<pre>
Input: "I speak Goat Latin"
Output: "Imaa peaksmaaa oatGmaaaa atinLmaaaaa"
</pre>

**Example 2:**
<pre>
Input: "The quick brown fox jumped over the lazy dog"
Output: "heTmaa uickqmaaa rownbmaaaa oxfmaaaaa umpedjmaaaaaa overmaaaaaaa hetmaaaaaaaa azylmaaaaaaaaa ogdmaaaaaaaaaa"
</pre>

Notes:

S contains only uppercase, lowercase and spaces. Exactly one space between each word.
1 <= S.length <= 150.

## Thinking

## Coding
Time: O(n); Go through the array once. </br>
Space: O(1) 
```python3
class Solution:
    def toGoatLatin(self, S):
        """
        :type S: str
        :rtype: str
        """
        tokens = S.split(' ')
        i = 1
        for j in range(len(tokens)):
            if tokens[j][0] in ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U']:
                tokens[j] = ''.join([tokens[j],'ma','a'*i])
            else:
                tokens[j] = ''.join([tokens[j][1:], tokens[j][0],'ma','a'*i])
            i+=1
        return ' '.join(tokens)
            
```

