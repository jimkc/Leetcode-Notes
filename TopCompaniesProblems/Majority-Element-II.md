## Question
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.<br>

Note: The algorithm should run in linear time and in O(1) space.

**Example :**   
<pre>
Input: [3,2,3]
Output: [3]
Example 2:

Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
</pre>

## Thinking
For those who aren't familiar with Boyer-Moore Majority Vote algorithm,<br>
I found a great article (http://goo.gl/64Nams) that helps me to understand this fantastic algorithm!!<br>
Please check it out!<br>

The essential concepts is you keep a counter for the majority number X. If you find a number Y that is not X, the current counter should deduce 1. The reason is that if there is 5 X and 4 Y, there would be one (5-4) more X than Y. This could be explained as "4 X being paired out by 4 Y".<br>

And since the requirement is finding the majority for more than ceiling of [n/3], the answer would be less than or equal to two numbers.<br>
So we can modify the algorithm to maintain two counters for two majorities.

## Coding
Time: O(n);<br>
Space: O(1)
```python3
class Solution:
# @param {integer[]} nums
# @return {integer[]}
    def majorityElement(self, nums):
        if not nums:
            return []
        count1, count2, candidate1, candidate2 = 0, 0, 0, 1
        for n in nums:
            if n == candidate1:
                count1 += 1
            elif n == candidate2:
                count2 += 1
            elif count1 == 0:
                candidate1, count1 = n, 1
            elif count2 == 0:
                candidate2, count2 = n, 1
            else:
                count1, count2 = count1 - 1, count2 - 1
        # We can only limit the cadidates, and still have to go through the array to count
        return [n for n in (candidate1, candidate2)
                        if nums.count(n) > len(nums) // 3]
```

