## Question
Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

**Example :**   
<pre>
Input:nums = [1,1,1], k = 2
Output: 2
</pre>

Note:<br>
The length of the array is in range [1, 20,000].<br>
The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].

## Thinking
Let's remember count[V], the number of previous prefix sums with value V. If our newest prefix sum has value W, and W-V == K, then we add count[V] to our answer.<br>

This is because at time t, A[0] + A[1] + ... + A[t-1] = W, and there are count[V] indices j with j < t-1 and A[0] + A[1] + ... + A[j] = V. Thus, there are count[V] subarrays A[j+1] + A[j+2] + ... + A[t-1] = K.<br>

## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def subarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        c = collections.Counter()
        c[0] = 1
        ans, cur_val = 0, 0
        
        for num in nums:
            cur_val += num
            ans += c[cur_val-k] #Count of appearance of V= W-K
            c[cur_val]+=1
        
        return ans
```

