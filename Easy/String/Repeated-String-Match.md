## Question
Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.<br>

For example, with A = "abcd" and B = "cdabcdab".<br>

Return 3, because by repeating A three times (“abcdabcdabcd”), B is a substring of it; and B is not a substring of A repeated two times ("abcdabcd").<br>

Note:<br>
The length of A and B will be between 1 and 10000.<br>

## Thinking
Go through each index of A as the head of comparison, if the length of the rest of the words
isn't enough we append enough times of word A.

## Coding
Time: O(N); N is the length of word A. </br>
Space: O(1) 
```python3
class Solution:
    def repeatedStringMatch(self, A, B):
        """
        :type A: str
        :type B: str
        :rtype: int
        """
        la = len(A)
        lb = len(B)
        for i in range(la):
            if A[i] == B[0]:
                istart, iend = i, i+lb # iend is index after the last index being compared
                repeat = iend//la + 1 if iend % la !=0 else iend//la
                
                tmp = A*repeat
                if tmp[istart: iend] == B:
                    return repeat
        return -1        
```

