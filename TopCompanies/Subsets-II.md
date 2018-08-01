## Question
Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).<br>

Note: The solution set must not contain duplicate subsets.

**Example :**   
<pre>
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
</pre>

## Thinking
First method similar to Subset.md.

## Coding
Time: O(); <br>
Space: O()
```python3
class Solution:
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums.sort() 
        classify_array = [[nums[0], 1]] # Val and cnt
        ans = []
        
        # Record val and cnts
        for i in range(1,len(nums)):
            if nums[i] == classify_array[-1][0]:
                classify_array[-1][1] += 1
            else:
                classify_array.append([nums[i], 1])
                
        
        def dfs(index, array, tmp, ans):
            ans.append(tmp)
            
            # Get every possible combination of classify_array
            while index < len(array): 
                val = array[index][0]
                cnt = array[index][1]
                
                # Get every possible combination of different cnts of val 
                for i in range(1,cnt+1):
                    tmp1 = tmp + [val]*i
                    dfs(index+1, array, tmp1, ans)
                index += 1               
                
        dfs(0,classify_array, [], ans)
        return ans
```

Method 2:
```python3
class Solution:
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = set()
        def dfs(nums, i, curr):
          res.add(tuple(curr))
          for j in range(i, len(nums)):
            dfs(nums, j+1, curr + [nums[j]])
        
        dfs(sorted(nums), 0, [])
        return [list(s) for s in res]
```
