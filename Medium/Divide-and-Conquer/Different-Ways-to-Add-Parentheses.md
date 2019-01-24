## Question
Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and \*.<br>

**Example :**   
<pre>
Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2

Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
</pre>

## Thinking
Divide and conquer的精髓就是把大問題拆成最小的單位來看他的本質。這題的最單純小問題就是只有一個符號，然後把兩邊加起來，而大問題其實也是如此，
我們要做的就是把每個符號的兩側加起來，至於兩側要多少就交給recursive去算就好了，但是記得罪trivial case要找出來，也就是只有一個數字的情況，這是我們的stop case。<br>

另外可以用一個詞典來記錄看過的省時間。

## Coding
Time: O();<br>
Space: O();
```python3
class Solution(object):
    def diffWaysToCompute(self, input, memo={}):
        """
        :type input: str
        :rtype: List[int]
        """
        if input.isdigit():
            return [int(input)]
        # used to remember computed values
        if input in memo:
            return memo[input]
        values = []
        
        # split every thing to left part and right part of an operater
        for i in range(len(input)):
            if input[i] in "+-*":
                # list of values of different calculate combinations
                left_vals = self.diffWaysToCompute(input[:i],memo)
                right_vals = self.diffWaysToCompute(input[i+1:],memo)
                # try all combinations
                for v1 in left_vals:
                    for v2 in right_vals:
                        values.append(self.compute(input[i],v1,v2))
        memo[input] = values
        return values
        
    def compute(self,oper,a,b):
        if oper == "+":
            return a+b
        if oper == "-":
            return a-b
        if oper == "*":
            return a*b
```

