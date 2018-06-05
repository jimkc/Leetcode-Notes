## Question
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.<br>

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.<br>

You are given an API bool isBadVersion(version) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.<br>
**Example :**
<pre>
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 
</pre>

## Thinking

## Coding
Time: O(logn); </br>
Space: O(1) 
```python3
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        l = 0
        r = n
        
        while l <= r:
            mid = int((l+r)/2)
            
            if isBadVersion(mid) == False and isBadVersion(mid + 1) == False:
                l = mid + 1
            elif isBadVersion(mid) == True and isBadVersion(mid + 1) == True:
                r = mid -1
            else:
                return mid + 1
```

