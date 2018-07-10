## Question
Initially, there is a Robot at position (0, 0). Given a sequence of its moves, judge if this robot makes a circle, which means it moves back to the original place.<br>

The move sequence is represented by a string. And each move is represent by a character. The valid robot moves are R (Right), L (Left), U (Up) and D (down). The output should be true or false representing whether the robot makes a circle.<br>

**Example 1:**
<pre>
Input: "UD"
Output: true
</pre>

**Example 2:**
<pre>
Input: "LL"
Output: false
</pre>


## Thinking
Find the number of chars is easier than tranforming them to nums and count them.

## Coding
Time: O(n); Go through the string 4 times. </br>
Space: O(1) 
```python3
class Solution:
    def judgeCircle(self, moves):
        """
        :type moves: str
        :rtype: bool
        """
        return moves.count('L') == moves.count('R') and moves.count('U') == moves.count('D')
```

