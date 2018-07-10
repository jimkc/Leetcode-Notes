## Question
Winter is coming! Your first job during the contest is to design a standard heater with fixed warm radius to warm all the houses.</br>

Now, you are given positions of houses and heaters on a horizontal line, find out minimum radius of heaters so that all houses could be covered by those heaters.</br>

So, your input will be the positions of houses and heaters seperately, and your expected output will be the minimum radius standard of heaters.</br>
**Example 1:**
<pre>
Input: [1,2,3],[2]
Output: 1
Explanation: The only heater was placed in the position 2, and if we use the radius 1 standard, then all the houses can be warmed.
</pre>

**Example 2:**
<pre>
Input: [1,2,3,4],[1,4]
Output: 1
Explanation: The two heater was placed in the position 1 and 4. We need to use radius 1 standard, then all the houses can be warmed.
</pre>

Note:
Numbers of houses and heaters you are given are non-negative and will not exceed 25000.
Positions of houses and heaters you are given are non-negative and will not exceed 10^9.
As long as a house is in the heaters' warm radius range, it can be warmed.
All the heaters follow your radius standard and the warm radius will the same.

## Thinking


## Coding
Time: Not sure.;  </br>
Space: O(1) 
```python3
class Solution:
    def findRadius(self, houses, heaters):
        """
        :type houses: List[int]
        :type heaters: List[int]
        :rtype: int
        """
        
        houses.sort()
        heaters.sort()
        heaters=[float('-inf')]+heaters+[float('inf')] # add 2 fake heaters
        ans,i = 0,0
        for house in houses:
            while house > heaters[i+1]:  # search to put house between heaters
                i +=1
            dis = min (house - heaters[i], heaters[i+1]- house) #For every house, find the nearest heater
            ans = max(ans, dis)
        return ans
```

