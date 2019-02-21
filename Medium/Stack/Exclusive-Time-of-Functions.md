## Question
Given the running logs of n functions that are executed in a nonpreemptive single threaded CPU, find the exclusive time of these functions.<br>

Each function has a unique id, start from 0 to n-1. A function may be called recursively or by another function.<br>

A log is a string has this format : function_id:start_or_end:timestamp. For example, "0:start:0" means function 0 starts from the very beginning of time 0. "0:end:0" means function 0 ends to the very end of time 0.<br>

Exclusive time of a function is defined as the time spent within this function, the time spent by calling other functions should not be considered as this function's exclusive time. You should return the exclusive time of each function sorted by their function id.

**Example :**   
<pre>
Input:
n = 2
logs = 
["0:start:0",
 "1:start:2",
 "1:end:5",
 "0:end:6"]
Output:[3, 4]
Explanation:
Function 0 starts at time 0, then it executes 2 units of time and reaches the end of time 1. 
Now function 0 calls function 1, function 1 starts at time 2, executes 4 units of time and end at time 5.
Function 0 is running again at time 6, and also end at the time 6, thus executes 1 unit of time. 
So function 0 totally execute 2 + 1 = 3 units of time, and function 1 totally execute 4 units of time.
</pre>

Note:<br>
Input logs will be sorted by timestamp, NOT log id.<br>
Your output should be sorted by function id, which means the 0th element of your output corresponds to the exclusive time of function 0.<br>
Two functions won't start or end at the same time.<br>
Functions could be called recursively, and will always end.<br>
1 <= n <= 100

## Thinking
1. Each time we pop out a question we get the time from start to end, and we delete the time that the recursive functions called inside this function.

## Coding
Time: O(logs);<br>
Space: O(logs)
```python3
class Solution:
    def exclusiveTime(self, n, logs):
        """
        :type n: int
        :type logs: List[str]
        :rtype: List[int]
        """
        
        stack = []
        ans = [0]*n
        prev_time = 0
        
        for log in logs:
            data = log.split(':')
            func = int(data[0])
            progress = data[1]
            time = int(data[2])
            
            if progress ==  'start':
                stack.append([func, time, 0]) 
            else:
                start_func, start_time, not_contain = stack.pop() # Previous func must be the same as end
                duration = time - start_time + 1 - not_contain # not contain means the time not belong to this func
                ans[func] += duration
               
                # the below process adds a the time that the pop function runs to not_contain
                # so that we dont add the inner recursive function to the exclusive time of the 
                # outer function
                if stack:
                    stack[-1][-1] += time - start_time + 1 # These time belongs to other func
                
        return ans
```

