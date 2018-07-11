## Question
Given a set of distinct integers, nums, return all possible subsets (the power set).<br>

Note: The solution set must not contain duplicate subsets.

**Example :**   
<pre>
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
<pre>

## Thinking

## Coding
Time: O(n^3); <br>
Space: O()
```python3
class Solution:
            def arrayPairSum(self, nums):
                #:type nums: List[int]
                #:rtype: int

                nums.sort()
                max_sum_pair = 0
                for i in range(0,len(nums),2):
                    max_sum_pair += nums[i] 
                return max_sum_pairclass Solution:
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        
        self.ans = []
        
        def backtracking(nums,index,tmp):
            self.ans.append(tmp)
            
            while index < len(nums):
                tmp1 = tmp + [nums[index]]
                index += 1 # index is a pointer that tells where to start mving on to the buttom
                backtracking(nums, index, tmp1)
                
        backtracking(nums,0,[])
        return self.ans
```

