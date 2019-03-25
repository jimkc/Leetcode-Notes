## Question
You are given an integer array sorted in ascending order (may contain duplicates), you need to split them into several subsequences, where each subsequences consist of at least 3 consecutive integers. Return whether you can make such a split.<br>

**Example :**   
<pre>
Input: [1,2,3,3,4,5]
Output: True
Explanation:
You can split them into two consecutive subsequences : 
1, 2, 3
3, 4, 5


Input: [1,2,3,3,4,4,5,5]
Output: True
Explanation:
You can split them into two consecutive subsequences : 
1, 2, 3, 4, 5
3, 4, 5


Input: [1,2,3,4,4,5]
Output: False
</pre>

Note:<br>
The length of the input is in range of [1, 10000]<br>


## Thinking
I used a greedy algorithm.<br>
"remain" is a hashmap, remain[i] counts the number of i that I haven't placed yet.<br>
end is a hashmap, end[i] counts the number of consecutive subsequences that ends at number i<br>
Then I tried to split the nums one by one.<br>
If I could neither add a number to the end of a existing consecutive subsequence nor find two following number in the left, I returned False

## Coding
Time: O(n);  </br>
Space: O(n)
```python
class Solution:
    def isPossible(self, nums: List[int]) -> bool:
        # remain counts of numbers we havent use
        remain = collections.Counter(nums)
        # nums of sequence that ends with i
        end = collections.Counter()
        
        
        # greedy, either add to a seq or get new num to create seq
        # if cant return False
        for i in nums:
            if remain[i] == 0:
                continue
                
            remain[i] -= 1
            if end[i-1] > 0:
                end[i]+=1
                end[i-1]-=1
            elif remain[i+1] and remain[i+2]:
                remain[i+1]-=1
                remain[i+2]-=1
                end[i+2] += 1
            else:
                return False
        return True
```

