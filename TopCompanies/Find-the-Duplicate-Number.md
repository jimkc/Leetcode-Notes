## Question
Given an array of 2n integers, your task is to group these integers into n pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible. </br>

**Example :**   
<pre>
Input: [1,3,4,2,2]
Output: 2

Input: [3,1,3,4,2]
Output: 3
</pre>

Note:<br>

You must not modify the array (assume the array is read only).<br>
You must use only constant, O(1) extra space.<br>
Your runtime complexity should be less than O(n2).<br>
There is only one duplicate number in the array, but it could be repeated more than once.

## Thinking
https://leetcode.com/problems/find-the-duplicate-number/solution/<br>
Cycly linked list and proof.<br>
https://leetcode.com/problems/linked-list-cycle-ii/solution/

## Coding
Time: O(n); <br>
Space: O(1)
```python3
class Solution:
    def findDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        # Find the intersection point of the two runners.
        # Means they are both in cycle, because hare chased the tortoise again
        tortoise = nums[0]
        hare = nums[0]
        while True:
            tortoise = nums[tortoise]
            hare = nums[nums[hare]]
            if tortoise == hare:
                break

        # Find the "entrance" to the cycle.
        # One 
        ptr1 = nums[0]
        ptr2 = hare # or tortoise is fine
        while ptr1 != ptr2:
            ptr1 = nums[ptr1]
            ptr2 = nums[ptr2]

        return ptr1
```

