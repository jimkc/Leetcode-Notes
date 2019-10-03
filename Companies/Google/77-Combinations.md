## Question
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n. </br>

**Example :**   
<pre>
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
</pre>

## Thinking
**Approach 1: Backtracking**

Backtracking is an algorithm for finding all solutions by exploring all potential candidates. If the solution candidate turns to be not a solution (or at least not the last one), backtracking algorithm discards it by making some changes on the previous step, i.e. backtracks and then try again.<br>
<br>
Here is a backtrack function which takes a first integer to add and a current combination as arguments backtrack(first, curr).<br>
<br>
If the current combination is done - add it to output.<br>
<br>
Iterate over the integers from first to n.<br>
<br>
Add integer i into the current combination curr.<br>
<br>
Proceed to add more integers into the combination : backtrack(i + 1, curr).<br>
<br>
Backtrack by removing i from curr.<br>

## Coding
Time: O(kC(k)(n)) C k 選 n; </br>
Space: O(C k 選 n)
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        
        def backtrack(first, curr, ans):
            if len(curr) == k:
                # use [:] to duplicate the list, so when we pop curr the answer doesn't disappear
                ans.append(curr[:])
            
            for i in range(first, n+1):
                curr.append(i)
                backtrack(i+1, curr, ans)
                # remove the last digit, so the next loop can replace it with the new i
                curr.pop() 
            
        ans = []
        backtrack(1, [], ans)
        return ans
```

	