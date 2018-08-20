## Question
Evaluate the value of an arithmetic expression in Reverse Polish Notation.<br>

Valid operators are +, -, *, /. Each operand may be an integer or another expression.<br>

Note:<br>

Division between two integers should truncate toward zero.<br>
The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example :**   
<pre>
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9


Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6


Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def evalRPN(self, tokens):
        """
        :type tokens: List[str]
        :rtype: int
        """
        stack = []
        symbols = {'+','-','*','/'}
        for tok in tokens:
            if tok in symbols:
                n2 = stack.pop()
                n1 = stack.pop()
                
                if tok == '+':
                    stack.append(n1+n2)
                elif tok == '-':
                    stack.append(n1-n2)
                elif tok == '*':
                    stack.append(n1*n2)
                else:
                    stack.append(int(n1/n2)) # Dont use n1//n2 because // cant handle negative nums
            else:
                stack.append(int(tok))
                
            
        return stack[0]
```

