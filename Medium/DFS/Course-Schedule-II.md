## Question
There are a total of n courses you have to take, labeled from 0 to n-1.<br>

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]<br>

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.<br>

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

**Example :**   
<pre>
Input: 2, [[1,0]] 
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished   
             course 0. So the correct course order is [0,1] .


Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both     
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. 
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .
</pre>

Note:<br>

The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.<br>
You may assume that there are no duplicate edges in the input prerequisites.

## Thinking
Same as Course-Schedule.md

## Coding
Time: O(n); 
Space: O(n)
```python3
class Solution:
    def findOrder(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        
        graph = [[] for _ in range(numCourses)]
        visited = [0 for _ in range(numCourses)] 
        ans = []
        
        # Record all prerequisites for lessons
        for x,y in prerequisites :
            graph[x].append(y)
        
        for i in range(numCourses):
            if self.hasCycle(i, visited, graph, ans):
                return []
        return ans
        
    def hasCycle(self, lesson, visited, graph, ans):
        
        if visited[lesson] == -1: # The node being visited in the walking path will be marked -1
            return True
        elif visited[lesson] == 1: # The node that has been walked through as the first node to others and has no  cycle
            return False
        else:
            visited[lesson] = -1 # Set as visited
        
        for pre in graph[lesson]:
            if self.hasCycle(pre, visited, graph, ans):
                return True
        
        visited[lesson] = 1 # When nothing happens, it means there is no cycle for this node
        ans.append(lesson)
        return False 
```

