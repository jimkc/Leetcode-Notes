## Question
There are a total of n courses you have to take, labeled from 0 to n-1.<br>

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]<br>

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

**Example :**   
<pre>
Input: 2, [[1,0]] 
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
             

Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
</pre>

Note:<br>

The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.<br>
You may assume that there are no duplicate edges in the input prerequisites.

## Thinking
1. This problem can be viewed as a graph problem, while prerequisites are the next class this class points to, if we walk through every path and find no cycles, then it is possible to complete all courses.<br>
2. The reason we dont use dictionary is because vertex in the graph might point to more than one classes, in such case dfs is better.<br>
3. While we start to walk from a vertex, every time we step on a vertex that is zero (never be visited) we set it to -1, and if we didnt end dfs(still on the path) and we met -1 again, it means we encoutered a cycle.<br>
3. If we finish traversing all prerequisites vertex paths (dfs function calls are finished) we set vertex to one as a safe node (no cycles guaranteed to start from this node), so next time other nodes came we can just return and no need to walk again.

## Coding
Time: O(n);<br>
Space: O(n)
```python3
class Solution:
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        
        graph = [[] for _ in range(numCourses)]
        visited = [0 for _ in range(numCourses)] 

        # Record all prerequisites for lessons
        for x,y in prerequisites :
            graph[x].append(y)
        
        # go through all vertexes
        for i in range(numCourses):
            if self.hasCycle(i, visited, graph):
                return False
        return True
        
    def hasCycle(self, lesson, visited, graph):
        
        if visited[lesson] == -1: # The node is visited in the current walking path 
            return True
        elif visited[lesson] == 1: # The node that has been walked through as the first node to others and has no cycle (already a safe path)
            return False
        else:
            visited[lesson] = -1 # Set as visited
        
        # keep walking the pre nodes until 
        for pre in graph[lesson]:
            if self.hasCycle(pre, visited, graph):
                return True
        
        visited[lesson] = 1 # When nothing happens, it means there is no cycle for this node
        return False 
```

