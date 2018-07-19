## Question
Given an array, rotate the array to the right by k steps, where k is non-negative.

**Example :**   
<pre>
Input: [1,2,3,4,5,6,7] and k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

Input: [-1,-100,3,99] and k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
</pre>

Note:<br>

Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.<br>
Could you do it in-place with O(1) extra space?

## Thinking

## Coding
Time: O(); 
Space: O()
```python3
class Solution:
    def rotate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        
        for i in range(k):
            tmp = nums[-1]
            nums[1:] = nums[:-1] 
            nums[0] = tmp
```

Method 2
```python3
class Solution:
    def rotate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        l = len(nums)
        k = k % l
        nums[:] = nums[-k:] + nums[:-k]
```

Method 3
```python3
class Solution:
    def rotate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        length = len(nums)
        k_copy = k
        new_array = []
        for i in range(k):
            val = nums.pop()
            nums.insert(0, val)
        
```

