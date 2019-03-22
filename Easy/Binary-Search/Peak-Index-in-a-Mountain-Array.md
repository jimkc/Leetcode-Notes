## Question
Let's call an array A a mountain if the following properties hold:<br>

A.length >= 3<br>
There exists some 0 < i < A.length - 1 such that A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]<br>
Given an array that is definitely a mountain, return any i such that A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1].

**Example :**   
<pre>
Input: [0,1,0]
Output: 1

Input: [0,2,1,0]
Output: 1
</pre>

Note:<br>

3 <= A.length <= 10000<br>
0 <= A[i] <= 10^6<br>
A is a mountain, as defined above.


## Thinking


## Coding
Time: O(logn);  </br>
Space: O(1)
```python
class Solution:
    def peakIndexInMountainArray(self, A: List[int]) -> int:
        
        l = 0
        r = len(A)-1
        
        while l <= r:
            
            mid = (l+r)//2
            print(A[mid], A[l], A[r])
            if A[mid-1] < A[mid] and A[mid] > A[mid+1]:
                return mid
            elif A[mid] > A[mid-1]:
                l = mid+1
            else:
                r = mid -1
        
```

