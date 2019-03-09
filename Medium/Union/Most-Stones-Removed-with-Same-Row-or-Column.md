## Question
On a 2D plane, we place stones at some integer coordinate points.  Each coordinate point may have at most one stone.<br>

Now, a move consists of removing a stone that shares a column or row with another stone on the grid.<br>

What is the largest possible number of moves we can make?


**Example :**   
<pre>
Input: stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
Output: 5

Input: stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
Output: 3

Input: stones = [[0,0]]
Output: 0
</pre>

## Thinking
If you got the idea, question is basically asking for number of islands<br>
The key point is here, we define an island as number of points that are connected by row or column. Every point does not have to be next to each other.<br>
Rest is donkey work.

## Coding
Time: O(n);all points once </br>
Space: O(n)
```python
class Solution:
    def removeStones(self, stones):
        
        # remove every node in points that is the same row and col of the enter node
        def dfs(i, j):
            points.discard((i, j))
            for y in rows[i]:
                if (i, y) in points:
                    dfs(i, y)
            for x in cols[j]:
                if (x, j) in points:
                    dfs(x, j)
        
        # points is the set used to remember what node we have walked through
        points, island, rows, cols = {(i, j) for i, j in stones}, 0, collections.defaultdict(list), collections.defaultdict(list)
        
        for i, j in stones:
            rows[i].append(j)
            cols[j].append(i)
        for i, j in stones:
            if (i, j) in points:
                dfs(i, j)
                island += 1 # we deleted the node in points so we add1
        return len(stones) - island
                        
```

