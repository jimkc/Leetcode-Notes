## Question
In an election, the i-th vote was cast for persons[i] at time times[i].<br>

Now, we would like to implement the following query function: TopVotedCandidate.q(int t) will return the number of the person that was leading the election at time t.  <br>

Votes cast at time t will count towards our query.  In the case of a tie, the most recent vote (among tied candidates) wins.

 
**Example :**   
<pre>
Input: ["TopVotedCandidate","q","q","q","q","q","q"], [[[0,1,1,0,0,1,0],[0,5,10,15,20,25,30]],[3],[12],[25],[15],[24],[8]]
Output: [null,0,1,1,0,0,1]
Explanation: 
At time 3, the votes are [0], and 0 is leading.
At time 12, the votes are [0,1,1], and 1 is leading.
At time 25, the votes are [0,1,1,0,0,1], and 1 is leading (as ties go to the most recent vote.)
This continues for 3 more queries at time 15, 24, and 8.
</pre>

## Thinking
The first array is the vote in the corresponding time.<br>
1. Record an array to know at the certain time, which candidate is in the lead.<br>
2. Use binary search on the time to check what range is the target time in.

## Coding
Time: O(); </br>
Space: O()
```python
from collections import Counter
class TopVotedCandidate:

    def __init__(self, persons: List[int], times: List[int]):
        self.persons = persons
        self.times = times
        self.wins = []
        
        # the current lead person
        lead = -1
        # record the votes for each person in the current time
        candidates = {}
        
        for t, p in zip(times, persons):
            # get() gives a default calue
            candidates[p] = candidates.get(p, 0)+1
            if candidates[p] >= candidates.get(lead,0):
                lead = p
            self.wins.append(lead)
            

            
                                 

    def q(self, t: int) -> int:
        

        
        l = 0
        h = len(self.times)-1
        mid = -1
        
        while l<=h:
            mid = (l+h)//2
            #print (mid)
            if self.times[mid] == t:
                return self.wins[mid]
            elif self.times[mid] > t:
                h = mid-1
            else:
                l = mid+1
        
        if self.times[mid] < t:
            return self.wins[mid]
        else:
            return self.wins[mid-1]
            


# Your TopVotedCandidate object will be instantiated and called as such:
# obj = TopVotedCandidate(persons, times)
# param_1 = obj.q(t)
```

