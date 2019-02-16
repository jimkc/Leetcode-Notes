## Question
Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

**Example :**   
<pre>
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
</pre>

Follow up:<br>
What if negative numbers are allowed in the given array?<br>
How does it change the problem?<br>
What limitation we need to add to the question to allow negative numbers?

## Thinking
1. 第一步驟先對dp要用的值做初始化，算各個數字出現的次數（最基本的組合就是自己），而這個dp表格的大小就是我們target的大小。
2. 每個表格裡的數字可以代表那個index值有幾種組合（ex:index k，加起來=k有幾種組合）。dp從最base case出發，
也就是我們nums裡出現最小的數字，然後用每個nums裡的數字加上這個index，若沒有超過就把這個index的值（累積到目前的組合數目）＋ nums裡這個數字在此前有多少組合了（nums[i]）
3. 因為從最小的dp開始，所以每次加完都會是比自己大的值累積在dp array，因此在iterate的時候就可以直接
## Coding
Time: O(target*n); <br>
Space: O(target)
```python3
class Solution:
    def combinationSum4(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        # Initialize: Count freq of each number
        dp = [0]*(target+1)
        for n in nums:
            if n <= target:
                dp[n] += 1 
        
        # Use dp from small sum to large sum
        for i in range(1,len(dp)):
            if dp[i] != 0:    # if it's 0 (combinations) then we cant use it as dp to combine other nums to a new num
                for n in nums:
                    if i+n <= target:
                        dp[i+n] += dp[i]
            
        return dp[-1]
```
