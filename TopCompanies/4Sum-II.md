## Question
Given four lists A, B, C, D of integer values, compute how many tuples (i, j, k, l) there are such that A[i] + B[j] + C[k] + D[l] is zero.<br>

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228 to 228 - 1 and the result is guaranteed to be at most 231 - 1.

**Example :**   
<pre>
Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
</pre>

## Thinking


## Coding
Time: O(n^2); <br>
Space: O(n)
```python3
class Solution:
    def fourSumCount(self, A, B, C, D):
        """
        :type A: List[int]
        :type B: List[int]
        :type C: List[int]
        :type D: List[int]
        :rtype: int
        """
        
        ab = []
        
        for n1 in A:
            for n2 in B:
                ab.append(n1+n2)
                
        ABsum = collections.Counter(ab)
        cnt = 0
        
        for n1 in C:
            for n2 in D:
                cdsum = n1+n2
                if -cdsum in ABsum:
                    cnt += ABsum[-cdsum] 
        return cnt
```

Faster:
```python3
class Solution:
    def fourSumCount(self, A, B, C, D):
        """
        :type A: List[int]
        :type B: List[int]
        :type C: List[int]
        :type D: List[int]
        :rtype: int
        """
        count_A=collections.Counter(A)
        count_B=collections.Counter(B)
        count_C=collections.Counter(C)
        count_D=collections.Counter(D)
        count_AB={}
        result=0
        for a,a_count in count_A.items():
            for b,b_count in count_B.items():
                count_AB[a+b]=count_AB.get(a+b,0)+a_count*b_count
        for c,c_count in count_C.items():
            for d,d_count in count_D.items():
                count=count_AB.get(-c-d,0)
                if count:
                    result+=c_count*d_count*count
        return result
```

