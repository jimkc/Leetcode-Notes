## Question
Given an unsorted array of integers, find the length of longest increasing subsequence.

**Example :**   
<pre>
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
</pre>

Note:<br>

There may be more than one LIS combination, it is only necessary for you to return the length.<br>
Your algorithm should run in O(n2) complexity.<br>
Follow up: Could you improve it to O(n log n) time complexity?<br>

## Thinking
Method 1 animation:https://leetcode.com/problems/longest-increasing-subsequence/solution/<br>


## Coding
Time: O(n^2); <br>
Space: O(n)
```python3
class Solution:
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        
        dp = [0]*len(nums) # Record from index 0 to index i the longest sequence length
        dp[0] = 1
        
        for i in range(1,len(nums)):
            length = 1
            
            for j in range(i+1):
                if nums[i] > nums[j] and dp[j]+1 > length:
                    length = dp[j]+1
            dp[i] = length
        
        return max(dp)
```

Time: O(nlogn);<br>
Space: O(n);
```python3
class Solution:
    # Time Complexity = O(nlogn)
    # Space Complexity = O(n)
    # 
    # Algorithm:
    # When traversing the nums, if the new element is larger than the maximum in dp, append the element.
    # If the new element is smaller than the maximum in dp, implementing binary search to find 
    # a position for the new element in dp and put the new element in that position and replace the old one
    # on that position.
    # 
    # Example: [3,5,6,2,5,4,19,5,6,7,12]
    # dp:
    # dp = []
    # insert 3 -> dp = [3]
    # insert 5 -> dp = [3, 5]
    # insert 6 -> dp = [3, 5, 6]
    # insert 2 -> 2 < 6 -> binary search find 2 should be on index 0 -> dp = [2, 5, 6]
    # insert 5 -> 5 < 6 -> binary search find 5 should be on index 1 -> dp = [2, 5, 6]
    # insert 4 -> 4 < 6 -> binary search find 4 should be on index 1 -> dp = [2, 4, 6]
    # insert 19 -> dp = [2, 4, 6, 19]
    # insert 5 -> 5 < 19 -> binary search find 5 should be on index 2 -> dp = [2, 4, 5, 19]
    # insert 6 -> 6 < 19 -> binary search find 6 should be on index 3 -> dp = [2, 4, 5, 6]
    # insert 7 -> dp = [2, 4, 5, 6, 7]
    # insert 12 -> dp = [2, 4, 5, 6, 7, 12]
    # return len(dp) -> 6
    # 
    # Why can this algorithm output the length of the longest increasing subsequence?
    # Explanation: 
    # It makes sense to append the new element to dp when the new element is larger than max(dp). 
    # Based on the example, upon the 3rd element in nums, we got dp = [3, 5, 6].
    # However, the coming element is 2 now which is smaller than max(dp). We insert 2 on dp[0] and 
    # get dp = [2, 5, 6]. It seems that we cannot do that, because in the order of nums 5, 6, 2 is not in
    # ascending order. However, we still keep the length of the ascending subsequence so far which is 3.
    # And the length of LIS is what we want not the LIS.
    # Keep going, assume we got dp = [2, 4, 5, 19] now. The coming element is 6 and we replace 19 to 6.
    # Now dp = [2, 4, 5, 6]. We can connect to the coming 7 and 12 to get the LIS. See the fantastic?
    # The element 6 kick off 19 and making the beautiful future. 
    # This algorithm cannot ensure to find the LIS, but it does give you the length of LIS. 
		
    def lengthOfLIS(self, nums):
        if not nums:
            return 0
        
        dp = [nums[0]]
        
        for i in range(1,len(nums)):
            if nums[i] > dp[-1]: dp.append(nums[i]) 
            else:
                pos = self.binarySearch(dp, 0, len(dp)-1, nums[i])
                dp[pos] = nums[i]
        return len(dp)
    
    def binarySearch(self, dp, low, high, toSearch):
        while low <= high:
            mid = (low + high) // 2
            if toSearch < dp[mid]:
                high = mid - 1
            elif toSearch > dp[mid]:
                low = mid + 1
            else:
                return mid
        # here we must return low, because we want to insert the new value on the right side of mid.
        # For example, insert 6 into [2,4,5,19], we need to get [2,4,5,6] rather than [2,4,6,19].
        return low
```
