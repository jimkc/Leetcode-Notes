## Question
Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum number of conference rooms required.

**Example :**   
<pre>
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2

Input: [[7,10],[2,4]]
Output: 1
</pre>

## Thinking
 Considering the room as resources, if start[s] < end[e], it means we do not have available resource now. Otherwise, it means when you are starting a new conferences, some other ones have finished, i.e., we have available resource already. You do not really care about the meeting that the start and end time belong to, and no need to preserve their relationship.

## Coding
Time: O(nlogn); The sorting aldorithm of python is **Timsort** which is average O(nlogn), and the adding process goes through the whole array once. </br>
Space: O(n)
```python3
# Definition for an interval.
# class Interval:
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e

class Solution:
    def minMeetingRooms(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: int
        """
        # Very similar with what we do in real life. Whenever you want to start a meeting, 
        # you go and check if any empty room available (available > 0) and
        # if so take one of them ( available -=1 ). Otherwise,
        # you need to find a new room someplace else ( numRooms += 1 ).  
        # After you finish the meeting, the room becomes available again ( available += 1 ).
        
        num_of_rooms = 0 
        rooms_available = 0 
        
        starts = []
        ends = []
        s = 0 # Pointer of starts
        e = 0 # Pointer of ends
        
        for meeting in intervals:
            starts.append(meeting.start)
            ends.append(meeting.end)
        
        starts.sort()
        ends.sort()
        
        while s < len(starts):
            # In start[s] time, a meeting are goning to start but ongoing meetings are not finished, need to take a room
            if starts[s] < ends[e]: 
                if rooms_available == 0:
                    num_of_rooms += 1
                else:
                    rooms_available -= 1
                
                s+=1
            else:
                rooms_available += 1
                e += 1
        
        return num_of_rooms
```
