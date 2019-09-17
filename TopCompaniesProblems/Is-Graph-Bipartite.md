## Question
Given an undirected graph, return true if and only if it is bipartite.<br>

Recall that a graph is bipartite if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.<br>

The graph is given in the following form: graph[i] is a list of indexes j for which the edge between nodes i and j exists.  Each node is an integer between 0 and graph.length - 1.  There are no self edges or parallel edges: graph[i] does not contain i, and it doesn't contain any element twice.

**Example :**   
<pre>
Input: [[1,3], [0,2], [1,3], [0,2]]
Output: true
Explanation: 
The graph looks like this:
0----1
|    |
|    |
3----2
We can divide the vertices into two groups: {0, 2} and {1, 3}.


Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
Output: false
Explanation: 
The graph looks like this:
0----1
| \  |
|  \ |
3----2
We cannot find a way to divide the set of nodes into two independent subsets.
</pre>

Note:<br>

graph will have length in range [1, 100].<br>
graph[i] will contain integers in range [0, graph.length - 1].<br>
graph[i] will not contain i or duplicate values.<br>
The graph is undirected: if any element j is in graph[i], then i will be in graph[j].

## Thinking


## Coding
Time: O(E);edges<br> 
Space: O(E)
```python3
class Solution:
    def isBipartite(self, graph):
        """
        :type graph: List[List[int]]
        :rtype: bool
        """
        seen = {} # key: node, value: color
        
        for nodes in graph:
            for n in nodes: # Must go through each node in case there are nodes with out edges
                if n not in seen: 
                    dfs = []
                    dfs.append(n)
                    seen[n] = 1 
                    
                    while dfs:
                        cur_node = dfs.pop()
                        color = seen[cur_node]*-1 # change color, because each edge has to connect to 2 diff color nodes

                        for node in graph[cur_node]:
                            if node not in seen:
                                dfs.append(node)
                                seen[node] = color
                            else:
                                if seen[node] != color: # Found color not the same 
                                    return False

        return True
```

