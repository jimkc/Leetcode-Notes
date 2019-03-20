## Question
Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.<br>

You may return any answer array that satisfies this condition.

**Example :**   
<pre>
Input: [3,1,2,4]
Output: [2,4,3,1]
The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
</pre>

## Thinking
Modify using two pointers is a better way as it saves space.

## Coding
Time: O(n); one pass </br>
Space: O(1)
```python
class Solution:
    def sortArrayByParity(self, A: List[int]) -> List[int]:
        
        i, j = 0, 0
        
        # left of A is original all odd, right even
        # i is the cursor points to odd numbers, j to identify even numbers
        for j in range(len(A)):
            if A[j]%2 == 0:
                A[j], A[i] = A[i], A[j]
                i+=1
        return A
```

