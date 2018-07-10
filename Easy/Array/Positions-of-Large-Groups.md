## Question
n a string S of lowercase letters, these letters form consecutive groups of the same character.

For example, a string like S = "abbxxxxzyy" has the groups "a", "bb", "xxxx", "z" and "yy".

Call a group large if it has 3 or more characters.  We would like the starting and ending positions of every large group.

The final answer should be in lexicographic order.

**Example 1:**
<pre>
Input: "abbxxxxzzy"
Output: [[3,6]]
Explanation: "xxxx" is the single large group with starting  3 and ending positions 6.
</pre>

**Example 2:**
<pre>
Input: "abc"
Output: []
Explanation: We have "a","b" and "c" but no large group.
</pre>

**Example 3:**
<pre>
Input: "abcdddeeeeaabbbcd"
Output: [[3,5],[6,9],[12,14]]
</pre>

Note:  1 <= S.length <= 1000

## Thinking
By adding a space character to avoid the case we can't find diff items between index i and index i-1.
## Coding
Time: O(n); </br>
Space: O(n) 
```python3
class Solution:
    def largeGroupPositions(self, S):
        """
        :type S: str
        :rtype: List[List[int]]
        """
        large_groups = []
        group_start = 0
        group_end = 0
        group_name = S[0]
        S+=" " #In case all chars are the same in the str
        
        for i in range(1,len(S)):
            if S[i] != S[i-1]:
                group_end = i-1
                length = group_end - group_start + 1
                if length >= 3:
                    large_groups.append([group_start,group_end])
                group_name = S[i]
                group_start = i
    
        return large_groups                
            
                    
```

