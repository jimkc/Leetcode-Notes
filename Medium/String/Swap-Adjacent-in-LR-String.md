## Question
In a string composed of 'L', 'R', and 'X' characters, like "RXXLRXRXL", a move consists of either replacing one occurrence of "XL" with "LX", or replacing one occurrence of "RX" with "XR". Given the starting string start and the ending string end, return True if and only if there exists a sequence of moves to transform one string to the other.


**Example :**   
<pre>
Input: start = "RXXLRXRXL", end = "XRLXXRRLX"
Output: True
Explanation:
We can transform start to end following these steps:
RXXLRXRXL ->
XRXLRXRXL ->
XRLXRXRXL ->
XRLXXRRXL ->
XRLXXRRLX
</pre>

## Thinking
1. The main idea is L swqp simply means that L can move any number of index left as long as the left is an "X". It's the same for "R" too.<br>
2. First, only record the index and char that is "L" or "R" for comparison.<br>
3. Second make sure it is possible for the char in start to move to the index of the same char in end.

## Coding
Time: O(n)</br>
Space: O(n)
```python
class Solution:
    def canTransform(self, start: str, end: str) -> bool:
        
        
        # ignore all X 
        s = [(c, i) for i,c in enumerate(start) if c in ("L", "R")]
        e = [(c, i) for i,c in enumerate(end) if c in ("L", "R")]
        
        if len(s) != len(e):
            return False
        
        for i in range(len(s)):
            start_c = s[i][0]
            start_i = s[i][1]
            end_c = e[i][0]
            end_i = e[i][1]
            
            # L or R doesn't match
            if start_c != end_c:
                return False
            else:
                # RXX..->XX..R this swap is the same as R moves form left to right
                if start_c == "R":
                    if start_i > end_i:
                        return False
                # XX..L->LXX.. this swap is the same as L moves form right to left
                else:
                    if start_i < end_i:
                        return False
        return True
            
        
                
        
```

