## Question
Implement a basic calculator to evaluate a simple expression string.<br>

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

**Example :**   
<pre>
Input: "1 + 1"
Output: 2

Input: " 2-1 + 2 "
Output: 3

Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23
</pre>

Note:<br>
You may assume that the given expression is always valid.<br>
Do not use the eval built-in library function.


## Thinking
這題主要的概念是以下四點：<br>
1.每個數字都會把stack的最後一個+1 or -1 拿來相乘加到答案裡面，而那+-1就是前面各個括弧前面的＋-號相乘累積下來最後和這個數字前面的符號相承的結果。<br>
2.因為數字會消耗掉一個符號所以為了保留ex:-(a+(b+c))最前面的-號也可以乘到b和c所以a前面的括弧要複製-號乘stack[-1]的結果給a消耗掉<br>
3.+-好會和stack[-1]裡的+-1相乘產生新符號append到stack最後<br>
4.每個 ")"會close掉這個scope所以要pop掉current sign<br>

<pre>
Here's an example trace for input 3-(2+(9-4)).
	
remaining   sign stack      total
3-(2+(9-4))   [1, 1]            0
 -(2+(9-4))   [1]               3
  (2+(9-4))   [1, -1]           3
   2+(9-4))   [1, -1, -1]       3
    +(9-4))   [1, -1]           1
     (9-4))   [1, -1, -1]       1
      9-4))   [1, -1, -1, -1]   1
       -4))   [1, -1, -1]      -8
        4))   [1, -1, -1, 1]   -8
         ))   [1, -1, -1]      -4
          )   [1, -1]          -4
              [1]              -4	

</pre>


## Coding
Time: O(n);  </br>
Space: O(n)
```python3
class Solution:
    def calculate(self, s):
        total = 0
        # in  case some a+(b+c) has no sign and "()" in front of a, we assumw 1(a+(b+c))
        i, signs = 0, [1, 1]
        while i < len(s):
            c = s[i]
            if c.isdigit():
                start = i
                # incase the number has multiple digits
                while i < len(s) and s[i].isdigit():
                    i += 1
                total += signs.pop() * int(s[start:i])
                continue
            if c in '+-(':
                # if c is not -, signs+=signs[-1]*1 else +=signs[-1]*-1
                # while meating "(", we put the same sign inside the stack again,because if the first number of the "()" will consume a sign in the stack, but there might still have digits in the "()" so we need to keep the sign outside the "()" untill we meet a ")"
                signs += signs[-1] * (1, -1)[c == '-'],
            # last digit is the sign of the "()" where the current cal is in
            elif c == ')':
                signs.pop()
            i += 1
        return total
```

