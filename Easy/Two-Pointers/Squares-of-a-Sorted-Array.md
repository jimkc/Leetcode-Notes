## Question
Given an array of integers A sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

**Example :**   
<pre>
Input: [-4,-1,0,3,10]
Output: [0,1,9,16,100]

Input: [-7,-3,2,3,11]
Output: [4,9,9,49,121]
</pre>

Note:<br>

1 <= A.length <= 10000<br>
-10000 <= A[i] <= 10000<br>
A is sorted in non-decreasing order.

## Thinking
1. The first one finds the start two pointers in the middle.<br>
2. The second one finds from the two sides of the array, faster.

## Coding
Time: O(n); </br>
Space: O(1) despite answer array
```python
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
        
        if len(A) <= 1:
            return [A[0]**2]
        elif A[0] >= 0:
            return [x**2 for x in A]
        
        
        
        # 1st positive index >=0
        # last negative index 
        first_pos = 0
        last_neg = 0
        ans = []
        
        for i in range(1,len(A)):
            if A[i] >= 0 and A[i-1] < 0:
                first_pos = i
                last_neg = i-1
                break
        
        
        while last_neg >=0 and first_pos < len(A):
            
            if A[first_pos] > abs(A[last_neg]):
                ans.append(A[last_neg]**2)
                last_neg -= 1
            else:
                ans.append(A[first_pos]**2)
                first_pos += 1
        
        if last_neg >= 0:
            for i in range(last_neg, -1, -1):
                ans.append(A[i]**2)
        elif first_pos < len(A):
            for i in range(first_pos, len(A), 1):
                ans.append(A[i]**2)
        
        return ans
                
```
faster, find the bigger elements first:
```python
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
        res = []
        l, r = 0, len(A)-1
        while l<=r:
            if abs(A[l])<abs(A[r]):
                res.append(A[r]*A[r])
                r-=1
            else:
                res.append(A[l]*A[l])
                l+=1
        res.reverse()
        return res
```




