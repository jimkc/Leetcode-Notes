## Question
The set S originally contains numbers from 1 to n. But unfortunately, due to the data error, one of the numbers in the set got duplicated to another number in the set, which results in repetition of one number and loss of another number.<br>

Given an array nums representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.<br>

**Example :**
<pre>
Input: nums = [1,2,2,4]
Output: [2,3]
</pre>

Note:
The given array size will in the range [2, 10000].<br>
The given array's numbers won't have any order.<br>
Seen this question in a real interview before?  <br>

## Thinking
The num in the array is in the raange of 1 to n and most of the number appears once, so 
instead of sorting the whole array , using the set is smarter.

## Coding
Time: O(n); </br>
Space: O(n).
```python3
class Solution:
    def findErrorNums(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        n = len(nums)
        seen = set()
        ans = []
        for num in nums:
            if num not in seen:
                seen.add(num)
            else:
                ans.append(num)
        for k in range(1,n+1):
            if k not in seen:
                ans.append(k)
                return ans
        
```

