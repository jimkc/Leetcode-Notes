## Question
X is a good number if after rotating each digit individually by 180 degrees, we get a valid number that is different from X.  Each digit must be rotated - we cannot choose to leave it alone.<br>

A number is valid if each digit remains a digit after rotation. 0, 1, and 8 rotate to themselves; 2 and 5 rotate to each other; 6 and 9 rotate to each other, and the rest of the numbers do not rotate to any other number and become invalid.<br>

Now given a positive number N, how many numbers X from 1 to N are good?<br>

**Example :**
<pre>
Input: 10
Output: 4
Explanation: 
There are four good numbers in the range [1, 10] : 2, 5, 6, 9.
Note that 1 and 10 are not good numbers, since they remain unchanged after rotating.
</pre>

Note:

Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.

## Thinking

## Coding
Time: O(N*D); D is the number of all digits. </br>
Space: O(1) 
```python3
class Solution:
    def rotatedDigits(self, N):
        """
        :type N: int
        :rtype: int
        """
        
        diff_rotate = {'2','5','6','9'}
        self_rotate = {'0','1','8'}
        cnt = 0
        for i in range(1,N+1):
            is_rotatable = True
            has_diff_rot = False
            for item in list(str(i)):
                if item not in diff_rotate:
                    if item not in self_rotate:
                        is_rotatable = False
                        break
                else:
                    has_diff_rot = True
            if is_rotatable and has_diff_rot:
                cnt += 1
        return cnt
```

