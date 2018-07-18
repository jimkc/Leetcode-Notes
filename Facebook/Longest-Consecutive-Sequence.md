## Question
Given an unsorted array of integers, find the length of the longest consecutive elements sequence.<br>

Your algorithm should run in O(n) complexity.

**Example :**   
<pre>
Input: [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
</pre>

## Thinking
Array and Union Find.

## Coding
Time: O(n); Go through all num once.<br>
Space: O(n)
```python3
class Solution:
    def longestConsecutive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        
        
        
        seen = set(nums)
        sequences = [] # A list that stores all possible consecutive combinations
        index = 0
        
        
        while seen:
            if len(sequences) < index+1:
                sequences.append(collections.deque([seen.pop()])) # Get a random num to start finding add minus 1 in seen
            
            has_upper = False 
            has_lower = False
            upper = sequences[index][-1]+1 # The biggest num in consecutive sequence add 1
            lower = sequences[index][0] -1 # The lowest num in consecutive sequence minus 1
            
            if upper in seen:
                has_upper = True
                seen.remove(upper)
                sequences[index].append(upper)
                
            if lower in seen:
                has_lower = True
                seen.remove(lower)
                sequences[index].appendleft(lower)
                
            if not (has_upper or has_lower): # If no upper and lower, it means the consecutive sequence is done
                index += 1 
            
                    
        maxl = 0
        for i in sequences:
            maxl = max(maxl,len(i))
        return maxl
```

