## Question
Given a sorted array, two integers k and x, find the k closest elements to x in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred.

**Example :**   
<pre>
Input: [1,2,3,4,5], k=4, x=3
Output: [1,2,3,4]

Input: [1,2,3,4,5], k=4, x=-1
Output: [1,2,3,4]
</pre>

Note:<br>
The value k is positive and will always be smaller than the length of the sorted array.<br>
Length of the given array is positive and will not exceed 104<br>
Absolute value of elements in the array and x will not exceed 104

## Thinking


## Coding
Time: O(n +logn); <br>
Space: O(1)
```python3
class Solution:
    def findClosestElements(self, arr, k, x):
        """
        :type arr: List[int]
        :type k: int
        :type x: int
        :rtype: List[int]
        """
        
        idx = self.binary_search(arr, x)
        
        # i to k is a k size window
        # window starts from k items at the left of x
        i = idx-k if (idx-k) >= 0 else 0 # Because smaller idx is prier
        j = i+k-1
    
        # mv window from left to right, if right is nearer than left than mv window
        # the window size is i-1 to j 
        while j+1 < len(arr) and abs(arr[j+1] - x) < abs(arr[i] - x): 
            i += 1
            j += 1
        return arr[i:j+1]
        
    # Return the index of the element nearest to the target
    def binary_search(self, arr, target):
        l = 0
        h = len(arr)-1
        
        if target < arr[l]:
            return l
        elif target > arr[h]:
            return h
        
        
        while l <= h:
            mid = (l+h)//2
            
            if arr[mid] == target:
                 return mid
            
            elif arr[mid] > target:
                h = mid-1
            else:
                l = mid+1
                
        return l if abs(arr[l]-target) <= abs(arr[h]-target) else h
```

