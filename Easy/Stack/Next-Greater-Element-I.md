## Question
You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.<br>

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

**Example :**
<pre>
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.


Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
</pre>

Note:<br>
All elements in nums1 and nums2 are unique.<br>
The length of both nums1 and nums2 would not exceed 1000.<br>


## Thinking
Build the stack to find next greater number for every num in nums2 first.

## Coding
Time: O(m+n);n for nums2 m for nums1, the second loop in loop1 only pops n elements in total, so its O(n) also . </br>
Space: O(m+n).
```python3
class Solution:
    def nextGreaterElement(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        ans = []
        tmp = []
        next_greater = {}
        
        # Use stack to record all possible next greater
        for item in nums2:
            if len(tmp) == 0:
                tmp.append(item)
            else:
                for i in range(len(tmp)-1,-1,-1):
                    if item > tmp[-1]:
                        next_greater[tmp[-1]] = item
                        tmp.pop()
                    else: 
                        break 
                tmp.append(item) # If we didnt find any next greater to add in dic, we add it in the top of the stack
                
        for n in nums1:
            if n in next_greater:
                ans.append(next_greater[n])
            else:
                ans.append(-1)
        return ans
```

