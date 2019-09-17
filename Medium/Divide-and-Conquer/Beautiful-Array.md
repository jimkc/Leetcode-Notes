## Question
For some fixed N, an array A is beautiful if it is a permutation of the integers 1, 2, ..., N, such that:<br>

For every i < j, there is no k with i < k < j such that A[k] * 2 = A[i] + A[j]. 任何兩個element加起來都不能等於他們之間任何element的兩倍<br>

Given N, return any beautiful array A.  (It is guaranteed that one exists.)

**Example :**   
<pre>
Input: 4
Output: [2,1,4,3]

Input: 5
Output: [3,1,2,5,4]
</pre>

## Thinking
Saying that an array is beautiful,<br>
there is no i < k < j,<br>
such that A[k] * 2 = A[i] + A[j]<br>

Apply these 3 following changes a beautiful array,<br>
we can get a new beautiful array<br>


1. Deletion<br>
Easy to prove.<br>

2. Addition<br>
If we have A[k] * 2 != A[i] + A[j],<br>
(A[k] + x) * 2 = A[k] * 2 + 2x != A[i] + A[j] + 2x = (A[i] + x) + (A[j] + x)<br>

E.g: [1,3,2] + 1 = [2,4,3].<br>

3. Multiplication<br>
If we have A[k] * 2 != A[i] + A[j],<br>
(A[k] * x) * 2 = A[k] * 2 * x != (A[i] + A[j]) * x = (A[i] * x) + (A[j] * x)<br>

E.g: [1,3,2] * 2 = [2,6,4]<br>

## Coding
Time: O(nlogn); <br>
Space: O(1)
```python3
class Solution(object):
    def beautifulArray(self, N):
        res = [1]
        # 逐步往上加
        while len(res) < N:
        	# fit 2 and 3 in description
        	# 把所有小於res最大數字的奇數和偶數part拿出來
        	# 任何數＊2-1都會是奇數
            res = [i * 2 - 1 for i in res] + [i * 2 for i in res]
        return [i for i in res if i <= N]
            
```

