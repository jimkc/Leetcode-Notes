## Question
Given m arrays, and each array is sorted in ascending order. Now you can pick up two integers from two different arrays (each array picks one) and calculate the distance. We define the distance between two integers a and b to be their absolute difference |a-b|. Your task is to find the maximum distance.<br>

**Example :**
<pre>
Input: 
[[1,2,3],
 [4,5],
 [1,2,3]]
Output: 4
Explanation: 
One way to reach the maximum distance 4 is to pick 1 in the first or third array and pick 5 in the second array.
</pre>

Note:<br>
Each given array will have at least 1 number. There will be at least two non-empty arrays.<br>
The total number of the integers in all the m arrays will be in the range of [2, 10000].<br>
The integers in the m arrays will be in the range of [-10000, 10000].<br>

## Thinking
We only need the largest one and the smallest one, so we dont need to sort the whole lst and fst.

## Coding
Time: O(n); </br>
Space: O(n).
```python3
class Solution:
    def maxDistance(self, arrays):
        """
        :type arrays: List[List[int]]
        :rtype: int
        """
        lst = []
        fst = []
        
        for i,v in enumerate(arrays):
            fst.append((v[0],i))
            lst.append((v[-1],i))

        max1, min1 = max(lst, key = lambda x: x[0]), min(fst, key = lambda x: x[0])
        
        if max1[1] != min1[1]:
            return abs(max1[0]-min1[0])
        else:
            del lst[max1[1]]
            del fst[min1[1]]
            max2, min2 = max(lst, key = lambda x: x[0]), min(fst, key = lambda x: x[0])
            return max(abs(max1[0]-min2[0]), abs(max2[0]-min1[0]))
```

