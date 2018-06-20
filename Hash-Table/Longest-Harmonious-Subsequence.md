## Question
We define a harmonious array is an array where the difference between its maximum value and its minimum value is exactly 1.<br>

Now, given an integer array, you need to find the length of its longest harmonious subsequence among all its possible subsequences.

**Example :**
<pre>
Input: [1,3,2,2,5,2,3,7]
Output: 5
Explanation: The longest harmonious subsequence is [3,2,2,2,3].
</pre>

Note:
You may assume that both strings contain only lowercase letters.

## Thinking


## Coding
Time: O(n);. </br>
Space: O(n).
```python3
class Solution:
    def findLHS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        seen = {}
        max_v = 0
        for n in nums:
            if n not in seen:
                seen[n] = 1
            else:
                seen[n] += 1
        for n,t in seen.items():
            if n+1 in seen and t + seen[n+1] > max_v:
                max_v = t + seen[n+1]
        return max_v
```

