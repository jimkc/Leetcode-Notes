## Question
Give a string s, count the number of non-empty (contiguous) substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.<br>

Substrings that occur multiple times are counted the number of times they occur.<br>

**Example 1:**
<pre>
Input: "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".

Notice that some of these substrings repeat and are counted the number of times they occur.

Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.
</pre>

**Example 2:**
<pre>
Input: "10101"
Output: 4
Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
</pre>

Note:

s.length will be between 1 and 50,000.
s will only consist of "0" or "1" characters.

## Thinking
Get every continuous seq lengths and count the possiblities at the end.

## Coding
Time: O(n); Go through the array once. </br>
Space: O(1) 
```python3
class Solution:
    def countBinarySubstrings(self, s):
        """
        :type s: str
        :rtype: int
        """
        con_nums = []
        fnd = 0
        cur = s[0]
        cur_num = 0 #Continue numbers
        for item in s:
            if item == cur:
                cur_num += 1
            else:
                con_nums.append(cur_num)
                cur = item
                cur_num = 1
        con_nums.append(cur_num)
        
        for i in range(len(con_nums)-1):
            fnd += min(con_nums[i+1],con_nums[i])
        return fnd

```

