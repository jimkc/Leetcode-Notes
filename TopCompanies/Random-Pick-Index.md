## Question
Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.<br>

Note:<br>
The array size can be very large. Solution that uses too much extra space will not pass the judge.<br>

**Example :**   
<pre>
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(3);

// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);
</pre>

## Thinking

## Coding
Time: O();<br>
Space: O()
```python3
class Solution:

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.indexes = collections.defaultdict(set)
        for i in range(len(nums)):
            self.indexes[nums[i]].add(i)

    def pick(self, target):
        """
        :type target: int
        :rtype: int
        """
        return random.sample(self.indexes[target],1)[0]


# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.pick(target)
```


Faster one:
```python3
class Solution:

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.nums = nums

    def pick(self, target):
        """
        :type target: int
        :rtype: int
        """
        n = self.nums.count(target)
        r = int(random.random() * n) + 1
        idx = -1
        for i in range(r):
            idx = self.nums.index(target, idx+1)
        return idx

```
