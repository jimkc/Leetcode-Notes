## Question
Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.<br>

The same repeated number may be chosen from candidates unlimited number of times.<br>

Note:<br>

All numbers (including target) will be positive integers.<br>
The solution set must not contain duplicate combinations.


**Example :**   
<pre>
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]


Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
</pre>

## Thinking


## Coding
Time: O(); <br>
Space: O()
```python3
class Solution:
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        
        q = collections.deque([[target]])
        ans = set()
        
        while q:
            target_combination = q.popleft() 
            target_remain = target_combination.pop() # Because the remainder is append at the bottom
            
            for n in candidates:
                t = target_remain - n # The next remain
                if t == 0:
                    c = target_combination + [target_remain]
                    c.sort()
                    ans.add(tuple(c))
                elif t > 0:
                    q.append(target_combination + [n, t])
        return [list(x) for x in ans]
```

DFS without duplicate combinations:
```python3
class Solution:
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        result = []
        candidates.sort()
        
        # Try to combine from 0 to target 
        def dfs(remain, stack):
            if remain == 0:
                result.append(stack[:])
                return 

            for item in candidates:
                if item > remain: 
                    break
                if stack and item < stack[-1]: # Prevent the duplicate combinations, because it is sorted
                    continue
                else:
                    dfs(remain - item, stack + [item])
            return

        dfs(target, [])
        return result
```
