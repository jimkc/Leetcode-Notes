## Question
On a campus represented as a 2D grid, there are N workers and M bikes, with N <= M. Each worker and bike is a 2D coordinate on this grid.<br>
<br>
Our goal is to assign a bike to each worker. Among the available bikes and workers, we choose the (worker, bike) pair with the shortest Manhattan distance between each other, and assign the bike to that worker. (If there are multiple (worker, bike) pairs with the same shortest Manhattan distance, we choose the pair with the smallest worker index; if there are multiple ways to do that, we choose the pair with the smallest bike index). We repeat this process until there are no available workers.<br>
<br>
The Manhattan distance between two points p1 and p2 is Manhattan(p1, p2) = |p1.x - p2.x| + |p1.y - p2.y|.<br>
<br>
Return a vector ans of length N, where ans[i] is the index (0-indexed) of the bike that the i-th worker is assigned to.<br>

**Example :**   
<pre>
Check pic: https://leetcode.com/problems/campus-bikes/description/

Input: workers = [[0,0],[2,1]], bikes = [[1,2],[3,3]]
Output: [1,0]
Explanation: 
Worker 1 grabs Bike 0 as they are closest (without ties), and Worker 0 is assigned Bike 1. So the output is [1, 0].

Input: workers = [[0,0],[1,1],[2,0]], bikes = [[1,0],[2,2],[2,1]]
Output: [0,2,1]
Explanation: 
Worker 0 grabs Bike 0 at first. Worker 1 and Worker 2 share the same distance to Bike 2, thus Worker 1 is assigned to Bike 2, and Worker 2 will take Bike 1. So the output is [0,2,1].
</pre>

Note:<br>
<br>
0 <= workers[i][j], bikes[i][j] < 1000<br>
All worker and bike locations are distinct.<br>
1 <= workers.length <= bikes.length <= 1000

## Thinking

## Coding
Time: O(mn); m is the number of workers and n is the nuber of bikes </br>
Space: O(mn)
```python
class Solution:
    def assignBikes(self, workers: List[List[int]], bikes: List[List[int]]) -> List[int]:
        
        worker_bike_distances = []
        
        # create a list of distances for each worker-bike combination, 
		# put distance in the first tuple element and worker index in second tuple element 
		# and bike index in third. we will sort this list of tuples later.
        for i, w_pos in enumerate(workers):
            for j, b_pos in enumerate(bikes):
                distance = abs(w_pos[0]-b_pos[0]) + abs(w_pos[1]-b_pos[1])
                worker_bike_distances.append((distance, i, j))
        
        # Sort the tuples
        worker_bike_distances.sort()
        
        ans = [-1]*len(workers)
        bike_given = set() # Mark a bike as taken by putting it in this set as we traverse the tuples from shortest distance onwards.
        for distance, i, j in worker_bike_distances:
            if ans[i] == -1 and j not in bike_given:
                ans[i] = j
                bike_given.add(j)
        
        return ans
```

