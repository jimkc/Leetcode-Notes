## Question
There are N rooms and you start in room 0.  Each room has a distinct number in 0, 1, 2, ..., N-1, and each room may have some keys to access the next room. <br>

Formally, each room i has a list of keys rooms[i], and each key rooms[i][j] is an integer in [0, 1, ..., N-1] where N = rooms.length.  A key rooms[i][j] = v opens the room with number v.<br>

Initially, all the rooms start locked (except for room 0). <br>

You can walk back and forth between rooms freely.<br>

Return true if and only if you can enter every room.

**Example :**   
<pre>
Input: [[1],[2],[3],[]]
Output: true
Explanation:  
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3.  Since we were able to go to every room, we return true.

Input: [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can't enter the room with number 2.
</pre>

Note:<br>

1 <= rooms.length <= 1000<br>
0 <= rooms[i].length <= 1000<br>
The number of keys in all rooms combined is at most 3000.

## Thinking


## Coding
Time: O(n) n for nums in list;<br>
Space: O(n)
```python
class Solution:
    def canVisitAllRooms(self, rooms):
        """
        :type rooms: List[List[int]]
        :rtype: bool
        """
        key = rooms[0]
        seen = set() # Path we had already seen
        open_rooms = [0]*len(rooms) # 0 for rooms not visited, 1 for visited
        open_rooms[0] = 1 
        cur_room = 0 # The room we are now in
        
        self.walk(rooms, key, open_rooms, cur_room, seen)
        return all(open_rooms) # Check if all idx is 1
        
    def walk(self, rooms, keys, open_rooms, cur_room, seen):
        
        for key in keys:
            nxt_room = key
            # Check if loop happens
            if (cur_room, nxt_room) in seen:
                continue 
            else:
                seen.add((cur_room, nxt_room))
                
            open_rooms[key] = 1
            new_keys = rooms[key] 
            self.walk(rooms, new_keys, open_rooms, key, seen)
    
```

Clean code:<br>
Time: O(k) go through all keys;<br>
Space: O(k)
```python
def canVisitAllRooms(self, rooms):
        dfs = [0]
        seen = set(dfs)
        while dfs:
            i = dfs.pop()
            for j in rooms[i]:
                if j not in seen:
                    dfs.append(j)
                    seen.add(j)
                    if len(seen) == len(rooms): return True # this line can be add or remove, depends
        return len(seen) == len(rooms)
```
