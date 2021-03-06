## Question
Given a list of sorted characters letters containing only lowercase letters, and given a target letter target, find the smallest element in the list that is larger than the given target.

Letters also wrap around. For example, if the target is target = 'z' and letters = ['a', 'b'], the answer is 'a'.

**Example :**
<pre>
Input:
letters = ["c", "f", "j"]
target = "a"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "c"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "d"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "g"
Output: "j"

Input:
letters = ["c", "f", "j"]
target = "j"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "k"
Output: "c"
</pre>

Note:

**1.** letters has a length in range [2, 10000]. </br>
**2.** letters consists of lowercase letters, and contains at least 2 unique letters.</br>
**3.** target is a lowercase letter.</br>

## Thinking
Think about the situation while the two pointers left and right are meeting with each other.</br>
We will find out that the left pointer will be the less larger one of the target.</br>
The only exception is while the target >= the biggest one of the list. Considering wrapping, the head of the list will be the answer in such case.</br>

## Coding
Time: O(logn); Binary search. </br>
Space: O(1) 
```python3
class Solution:
    def nextGreatestLetter(self, letters, target):
        """
        :type letters: List[str]
        :type target: str
        :rtype: str
        """
        left = 0
        right = len(letters)-1
        middle = int((right+left)/2)
        while left <= right:
            if letters[middle] > target:
                right = middle-1
            else:
                left = middle+1
            middle = int((right+left)/2)
    
        if target >= letters[-1]:
            return letters[0]
        else:
            return letters[left]
```

