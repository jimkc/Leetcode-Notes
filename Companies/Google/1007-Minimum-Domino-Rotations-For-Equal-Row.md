## Question
In a row of dominoes, A[i] and B[i] represent the top and bottom halves of the i-th domino.  (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)<br>
<br>
We may rotate the i-th domino, so that A[i] and B[i] swap values.<br>
<br>
Return the minimum number of rotations so that all the values in A are the same, or all the values in B are the same.<br>
<br>
If it cannot be done, return -1.<br>
<br>
 
**Example :**   
<pre>
Picture: https://leetcode.com/problems/minimum-domino-rotations-for-equal-row/description/

Input: A = [2,1,2,4,2,2], B = [5,2,6,2,3,2]
Output: 2
Explanation: 
The first figure represents the dominoes as given by A and B: before we do any rotations.
If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2, as indicated by the second figure.

Input: A = [3,5,1,2,3], B = [3,6,3,3,4]
Output: -1
Explanation: 
In this case, it is not possible to rotate the dominoes to make one row of values equal.
</pre>

## Thinking
Count the frequency of each number in A and B, respectively;<br>
Count the frequency of A[i] if A[i] == B[i];<br>
If countA[i] + countB[i] - same[i] == A.length, we find a solution; otherwise, return -1;<br>
min(countA[i], countB[i]) - same[i] is the answer.

## Coding
Time: O(n); </br>
Space: O(n)

```python
from collections import Counter

class Solution:
    def minDominoRotations(self, A: List[int], B: List[int]) -> int:
        if len(A) != len(B): return -1
        same, countA, countB = Counter(), Counter(A), Counter(B)
        for a, b in zip(A, B):
            if a == b:
                same[a] += 1
        for i in range(1, 7):
            if countA[i] + countB[i] - same[i] == len(A):
                return min(countA[i], countB[i]) - same[i]        
        return -1
```

