## Question
Given an input string s, reverse the order of the words.  
  
A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.  
  
Return a string of the words in reverse order concatenated by a single space.  
  
Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.  
  
**Example 1:**   
<pre>
Input: s = "the sky is blue"
Output: "blue is sky the"
</pre>
  
**Example 2:**   
<pre>
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
</pre>
  
**Example 3:**   
<pre>
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
</pre>
  
**Example 4:**   
<pre>
Input: s = "  Bob    Loves  Alice   "
Output: "Alice Loves Bob"
</pre>
  
**Example 5:**   
<pre>
Input: s = "Alice does not even like bob"
Output: "bob like even not does Alice"
</pre>
  
Note:  
* 1 <= s.length <= 104
* s contains English letters (upper-case and lower-case), digits, and spaces ' '.
* There is at least one word in s.
  
## Thinking
   
## Coding
Time: O(n);  
Space: O(n)

```Java
class Solution {
    public String reverseWords(String s) {
        // trim removes leading and tail space
        // split takes O(n)
        List<String> words = Arrays.asList(s.trim().split("\\s+"));
        Collections.reverse(words);
        return String.join(" ", words);
    }
}
```

