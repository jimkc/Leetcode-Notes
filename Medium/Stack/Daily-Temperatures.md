## Question
Given a list of daily temperatures, produce a list that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead.<br>

For example, given the list temperatures = [73, 74, 75, 71, 69, 72, 76, 73], your output should be [1, 1, 4, 2, 1, 1, 0, 0].<br>

Note: The length of temperatures will be in the range [1, 30000]. Each temperature will be an integer in the range [30, 100].

## Thinking
1. Stack records the days "later" than the current day's temp, so whenever before we put the cur day into stack, we pop all temp smaller than cur temp until we meet the first day in stack that is warmer than cur temp.<br>


## Coding
Time: O(N), where N is the length of T and W is the number of allowed values for T[i]. Each index gets pushed and popped at most once from the stack. <br>
Space: O(W)
```python3
class Solution:
    def dailyTemperatures(self, temperatures):
        """
        :type temperatures: List[int]
        :rtype: List[int]
        """
        T = len(temperatures)
        ans = [0]*T
        stack = [] # Used to remember the index of the temperatures
        
        # Go from backwards
        for i in range(T-1,-1,-1):
            # We pop until we meet the temperature that is bigger than the current temperture
            # want to leave the temperature that is higher than this day
            while stack and temperatures[i] >= temperatures[stack[-1]] :
                stack.pop()
            
            # Count the days we need 
            # stack[-1] records the index that has higher temperature
            if stack:
                ans[i] = stack[-1] - i
            stack.append(i)
                
        return ans
```

