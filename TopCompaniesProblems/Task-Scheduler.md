## Question
Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks.Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.<br>

However, there is a non-negative cooling interval n that means between two same tasks, there must be at least n intervals that CPU are doing different tasks or just be idle.<br>

You need to return the least number of intervals the CPU will take to finish all the given tasks.

**Example :**   
<pre>
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
</pre>

Note:<br>
The number of tasks is in the range [1, 10000].<br>
The integer n is in the range [0, 100].

## Thinking

## Coding
Time: O(n); 
Space: O(n);
```python3
class Solution(object):
    def leastInterval(self, tasks, n):
        """
        :type tasks: List[str]
        :type n: int
        :rtype: int
        """
        cnt = {}
        mf = 0 # max task  frequency
        mc = 0 # kinds of tasks in max freq
        
        for t in tasks:
            if t not in cnt:
                cnt[t] = 1
            else:
                cnt[t] += 1 
                
        for v in cnt.values():
            if v == mf:
                mc += 1
            elif v > mf:
                mf = v
                mc = 1
        
        base = mc*mf # AB_AB_AB_AB (A and B stands for all kinds of max freq we call bases)
        base += (mf-1)*(n-mc+1) # The spaces between AB, the tasks left in cnt will have freq <= num of bases, so we can put them                                   one by one in the spaces. Also we have guaranteed the distance between spaces will be at least                                   n, if tasks are not enough we put idle in it
        
        return max(len(tasks), base) # If the total spaces < num of tasks, there will be no idle
            
```
