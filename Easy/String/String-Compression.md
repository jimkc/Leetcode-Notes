## Question
Given an array of characters, compress it in-place.<br>

The length after compression must always be smaller than or equal to the original array.<br>

Every element of the array should be a character (not int) of length 1.<br>

After you are done modifying the input array in-place, return the new length of the array.<br>

**Example :**
<pre>
Input:
["a","a","b","b","c","c","c"]

Output:
Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]

Explanation:
"aa" is replaced by "a2". "bb" is replaced by "b2". "ccc" is replaced by "c3".
</pre>

**Example :**
<pre>
Input:
["a"]

Output:
Return 1, and the first 1 characters of the input array should be: ["a"]

Explanation:
Nothing is replaced.
</pre>

**Example :**
<pre>
Input:
["a","b","b","b","b","b","b","b","b","b","b","b","b"]

Output:
Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].

Explanation:
Since the character "a" does not repeat, it is not compressed. "bbbbbbbbbbbb" is replaced by "b12".
Notice each digit has it's own entry in the array.
</pre>

Note:
All characters have an ASCII value in [35, 126].
1 <= len(chars) <= 1000.

## Thinking
Count the continuous exist times for each character and delete the indexes we dont need.

## Coding
Time: O(n^2); Since deleteing a slice in the list takes O(n), because the whole list has to shift. </br>
Space: O(1). No extra space. 
```python3
class Solution:
    def compress(self, chars):
        """
        :type chars: List[str]
        :rtype: int
        """
        
        def compress_little(cnt_con, start):
            num_digit = list(str(cnt_con))
            for n in range(len(num_digit)):
                chars[start + n+1] = num_digit[n]
            del chars[start+1+len(num_digit): start+cnt_con] 

        end = len(chars)-1
        start = -1
        cnt_con = 1
        for i in range(len(chars)-1,0,-1):
            if chars[i-1] == chars[i]:
                cnt_con += 1
            else:
                if cnt_con > 1:
                    compress_little(cnt_con, i)
                cnt_con = 1
                
        if cnt_con > 1:
            compress_little(cnt_con, 0)
    
        return len(chars)
```

