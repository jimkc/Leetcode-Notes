## Question
Given an array of strings, group anagrams together.

**Example :**   
<pre>
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
</pre>

Note:<br>

All inputs will be in lowercase.<br>
The order of your output does not matter.

## Thinking
It seems to me that by pairing the neighbor numbers in the sorted list will result the maximum value.

## Coding
Time: O(wlogs); <br>
Space: O(w)
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
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        seen = {}
        dic = collections.defaultdict(list)
        
        for w in strs:
            dic[''.join(sorted(w))].append(w)
            
        return [x for x in dic.values()]
```

