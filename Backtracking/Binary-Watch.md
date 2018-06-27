## Question
A binary watch has 4 LEDs on the top which represent the hours (0-11), and the 6 LEDs on the bottom represent the minutes (0-59).<br>

Each LED represents a zero or one, with the least significant bit on the right.<br>

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.<br>

**Example :**
<pre>
Input: n = 1
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
</pre>

Note:<br>
The order of output does not matter.<br>
The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".<br>
The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".<br>

## Thinking


## Coding
Time: O(10^n); Not sure  </br>
Space: O().
```python3
class Solution:
    def readBinaryWatch(self, num):
        """
        :type num: int
        :rtype: List[str]
        """
        def backtrack(positions,remaining, outputs, start): 
            if remaining == 0: #if the number can't be decreased the dfs should be stopped ans saved
                positions = [str(x) for x in positions]
                outputs.append(''.join(positions))
            else:
                for i in range(start, len(positions)):
                    positions[i] = 1 #Set the index to one
                    backtrack(positions, remaining -1, outputs, i + 1) # We set an index to one, so num -1, and the index +1
                    positions[i] = 0  # While backing tracking ends, it means a permutation is saved, so we mv the last 1 one                                             index forward e.g: 1101000000 -> 1100100000
                    
        outputs = []
        leds = [0]*10
        backtrack(leds, num, outputs, 0)
        ans = []
        
        for b in outputs:
            hr = int(b[:4],2) # Turn string to int with base 2
            minute = int(b[4:],2)
            if hr < 12 and minute < 60:
                ans.append("{}:{:02}".format(hr,minute)) # Format {:02} means that the str will be at least 2 digits
        return ans
            
```
