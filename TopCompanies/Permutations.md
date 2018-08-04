## Question
Given a collection of distinct integers, return all possible permutations.

**Example :**   
<pre>
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
</pre>

## Thinking


## Coding
Time: O(n);<br>
Space: O(n)
```python3
class Solution:
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        ans = []
        self.per([],nums,ans)
        return ans
    
    def per(self,tmp, remain, ans):
        
        if remain == []:
            ans.append(tmp)
            return
        
        for i in range(len(remain)):
            self.per(tmp+[remain[i]], remain[:i]+remain[i+1:], ans)
```

