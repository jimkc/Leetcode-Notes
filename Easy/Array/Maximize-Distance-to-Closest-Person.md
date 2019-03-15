## Question
In a row of seats, 1 represents a person sitting in that seat, and 0 represents that the seat is empty. <br>

There is at least one empty seat, and at least one person sitting.<br>

Alex wants to sit in the seat such that the distance between him and the closest person to him is maximized. <br>

Return that maximum distance to closest person.

**Example :**   
<pre>
Input: [1,0,0,0,1,0,1]
Output: 2
Explanation: 
If Alex sits in the second open seat (seats[2]), then the closest person has distance 2.
If Alex sits in any other open seat, the closest person has distance 1.
Thus, the maximum distance to the closest person is 2.


Input: [1,0,0,0]
Output: 3
Explanation: 
If Alex sits in the last seat, the closest person is 3 seats away.
This is the maximum distance possible, so the answer is 3.
</pre>

## Thinking
The max distance between position a and postition b is (b-a)//2

## Coding
Time: O(n);  </br>
Space: O(n)
```python
class Solution:
    def maxDistToClosest(self, seats: List[int]) -> int:
        
        
        positions = []
        
        for i in range(len(seats)):
            if seats[i] == 1:
                positions.append(i)
        
        last_seat = len(seats)-1-positions[-1]
        first_seat = positions[0] #-0
        max_dist = max(first_seat, last_seat)
        
        
        
        for i in range(len(positions)-1):
            max_dist = max((positions[i+1]-positions[i])//2, max_dist)
        return max_dist
```

