## Question
Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.<br>

Note:<br>
The length of num is less than 10002 and will be ≥ k.<br>
The given num does not contain any leading zero.

**Example :**   
<pre>
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.


Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.


Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
</pre>

## Thinking
This question is very similar to 316. Remove Duplicate Letters with little bit extra work<br>

the way to make number as small as possible is that make right most digit as small as possible<br>

then come back to question, we know we can remove at most k digit to generate new digit<br>
so we can add/remove until k == 0<br>

as example of "1432219"<br>
[]<br>
['1'] => since stack is empty , just add into stack<br>
['1', '4'] => since 1 < 4, so we do not need to repalce<br>
['1', '3'] => since 3 is less than 4, andwe have k(3)-- times able to remove,so we can<br>
replace 4 with 3<br>
['1', '2'] => 2 < 3, replace 3 with 2 k(2) --<br>
['1', '2', '2'] => 2 == 2, continue<br>
['1', '2', '1'] => since 1 < 2, replace it k(1)--<br>
['1', '2', '1', '9'] => does not matter the next number is less or greater, since k is zero, we<br>
can not remove any more digit, so this is the end of program<br>
<br>
some case need to handle is avoid leading zero, which seems like the only different to 316. Remove Duplicate Letters

## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def removeKdigits(self, num, k):
        """
        :type num: str
        :type k: int
        :rtype: str
        """
        
        stack = []
        anslen = len(num)-k
        
        for i in range(len(num)):
            
            while stack and k > 0:
                if num[i] < stack[-1]:
                    stack.pop()
                    k-=1
                else:
                    break
            
            stack.append(num[i])
            
            if k == 0:
                ans = ''.join(stack)+num[i+1:]
                return str(int(ans))
        
        ans = ''.join(stack[:anslen])
        if ans == '':
            return '0'
        else:
            return str(int(ans))
```

