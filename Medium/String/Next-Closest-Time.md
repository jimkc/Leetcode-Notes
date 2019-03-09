## Question
Given a time represented in the format "HH:MM", form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.<br>

You may assume the given input string is always valid. For example, "01:34", "12:09" are all valid. "1:34", "12:9" are all invalid.

**Example :**   
<pre>
Input: "19:34"
Output: "19:39"
Explanation: The next closest time choosing from digits 1, 9, 3, 4, is 19:39, which occurs 5 minutes later.  It is not 19:33, because this occurs 23 hours and 59 minutes later.

Input: "23:59"
Output: "22:22"
Explanation: The next closest time choosing from digits 2, 3, 5, 9, is 22:22. It may be assumed that the returned time is next day's time since it is smaller than the input time numerically.
Seen this question in a real
</pre>

## Thinking


## Coding
Time: O(1); </br>
Space: O(1)
```python
class Solution:
    def nextClosestTime(self, time: str) -> str:
        t = time.split(':')
        h = t[0]
        m = t[1]
        cur_digits = "".join(t)
        
        
        # string of all posible times (hour)(min)
        possibles = set()
        self.find_possibles("", cur_digits, possibles)
        possibles.remove(cur_digits) # exclude the curent time
        
        # corner cases if 4 digits are all the same, return the original
        if len(possibles) == 0:
            return "{0}:{1}".format(cur_digits[:2], cur_digits[2:])
        
        # int of total minutes the time represent
        cur_total = self.time2min(cur_digits)
        nearest = ""
        min_distance = 24*60
        for time in possibles:
            total_minutes = self.time2min(time)
            distance = 0
            if total_minutes >= cur_total:
                distance = total_minutes - cur_total
            else:
                distance = 24*60 - cur_total + total_minutes  # the next day
            
            if distance < min_distance:
                min_distance = distance
                nearest = time
        return "{0}:{1}".format(nearest[:2],nearest[2:])
        
    # different from permutation, can reuse all bits
    def find_possibles(self, tmp, digits, ans):
        
        if len(tmp) == 4:
            if int(tmp[:2]) < 24 and int(tmp[2:])<60:
                ans.add(tmp)
            return 

        for i in range(len(digits)):
            self.find_possibles(tmp+digits[i], digits, ans)
        
    def time2min(self, time):
        h = int(time[:2])
        m = int(time[2:])
        
        total = h*60+m
        return total
        
    

            
        
```

