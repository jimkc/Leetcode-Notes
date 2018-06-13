## Question
The count-and-say sequence is the sequence of integers with the first five terms as following:
<pre>
1.     1
2.     11
3.     21
4.     1211
5.     111221
</pre>

1 is read off as "one 1" or 11.<br>
11 is read off as "two 1s" or 21.<br>
21 is read off as "one 2, then one 1" or 1211.<br>
Given an integer n, generate the nth term of the count-and-say sequence.<br>

Note: Each term of the sequence of integers will be represented as a string.

**Example :**
<pre>
Input: 1
Output: "1"
</pre>

**Example :**
<pre>
Input: 1
Output: "1"
</pre>


Note:

Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.

## Thinking
Do it step by step.

## Coding
Time: O(n^2); . </br>
Space: O(1) 
```python3
class Solution:
    def countAndSay(self, n):
        """
        :type n: int
        :rtype: str
        """
        
        if n == 1:
            return '1'
        
        say = '11'
        
        for _ in range(3,n+1):
            new_say = ''
            cnt = 1
            for i in range(1,len(say)):
                if say[i] == say[i-1]:
                    cnt += 1
                else:
                    new_say += (str(cnt) + say[i-1])
                    cnt = 1
            new_say += (str(cnt) + say[-1])
            #print (new_say)
            say = new_say
        return say
```
