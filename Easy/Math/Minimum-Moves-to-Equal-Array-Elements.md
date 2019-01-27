## Question
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

**Example :**   
<pre>
Input:
[1,2,3]

Output:
3

Explanation:
Only three moves are needed (remember each move increments two elements):

[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
</pre>

## Thinking
Let's define sum as the sum of all the numbers, before any moves; minNum as the min number int the list; n is the length of the list<br>

After, say m moves, we get all the numbers as x（最後所有的數字都是x） , and we will get the following equation<br>

sum + m * (n - 1) = x * n <br>
and actually,<br>

x = minNum + m<br>
This part may be a little confusing, but @shijungg explained very well. let me explain a little again. it comes from two observations:<br>

the minum number will always be minum until it reachs the final number, because every move, other numbers (besides the max) will be increamented too;<br>
from above, we can get, the minum number will be incremented in every move. So, if the final number is x, it would be minNum + moves;<br>
and finally, we will get<br>

sum - minNum * n = m<br>
This is just a math calculation.


## Coding
Time: O(); </br>
Space: O(1)
```python3
class Solution:
    def minMoves(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        min_num = min(nums)
        sum_all = sum(nums)
        n = len(nums)
        m = sum_all - n*min_num
        return m
```

