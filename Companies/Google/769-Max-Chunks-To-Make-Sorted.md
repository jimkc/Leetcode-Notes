## Question
Given an array arr that is a permutation of [0, 1, ..., arr.length - 1], we split the array into some number of "chunks" (partitions), and individually sort each chunk.  After concatenating them, the result equals the sorted array.<br>
<br>
What is the most number of chunks we could have made? </br>

**Example :**   
<pre>
Input: arr = [4,3,2,1,0]
Output: 1
Explanation:
Splitting into two or more chunks will not return the required result.
For example, splitting into [4, 3], [2, 1, 0] will result in [3, 4, 0, 1, 2], which isn't sorted.

Input: arr = [1,0,2,3,4]
Output: 4
Explanation:
We can split into two chunks, such as [1, 0], [2, 3, 4].
However, splitting into [1, 0], [2], [3], [4] is the highest number of chunks possible.
</pre>

Note:<br>
<br>
arr will have length in range [1, 10].<br>
arr[i] will be a permutation of [0, 1, ..., arr.length - 1].

## Thinking
**Brute Force**
**Intuition and Algorithm**
<br>
Let's try to find the smallest left-most chunk. If the first k elements are [0, 1, ..., k-1], then it can be broken into a chunk, and we have a smaller instance of the same problem.<br>
<br>
We can check whether k+1 elements chosen from [0, 1, ..., n-1] are [0, 1, ..., k] by checking whether the maximum of that choice is k.<br>

## Coding
Time: O(n); </br>
Space: O(1)
```python
class Solution:
    def maxChunksToSorted(self, arr: List[int]) -> int:
        ans = 0
        range_max = 0
        
        for i, v in enumerate(arr):
            # max number we met in the current range
            range_max = max(range_max, v)
            
            # the previous values are 0~range_max
            if range_max == i: ans+=1
        return ans
            
```

