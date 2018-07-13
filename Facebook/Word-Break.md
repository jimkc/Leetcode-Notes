## Question
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.<br>

Note:<br>

The same word in the dictionary may be reused multiple times in the segmentation.<br>
You may assume the dictionary does not contain duplicate words.

**Example :**   
<pre>
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".


Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.


Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
</pre>

## Thinking
To do DP we go through each index of the string. While going through, we used two pointers
i,j. i for 0 to buttom  and j for i ->0, j is used to find the index that 0~j is breakable 
also j~i is in the wordDict, and each time we record rather 0~i is breakable in the table[i].


## Coding
Time: O(n^2); 
Space: O(n)
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
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        
        table = [False]*len(s)
        dic = set()
        
        for w in wordDict:
            dic.add(w)
        
        
        for i in range(len(s)):
            for j in range(i,-1,-1):
                
                right_word = s[j:i+1] 
                left_can_break = table[j-1] if j > 0 else True # If left word can be break
                
                if right_word in dic and left_can_break:
                    table[i] = True
                    break
                    
        return table[-1]
```

