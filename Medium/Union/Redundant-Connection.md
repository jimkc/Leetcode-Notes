## Question
In this problem, a tree is an undirected graph that is connected and has no cycles.<br>

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.<br>

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [u, v] with u < v, that represents an undirected edge connecting nodes u and v.<br>

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge [u, v] should be in the same format, with u < v.<br>

**Example :**   
<pre>
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given undirected graph will be like this:
  1
 / \
2 - 3


Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]
Explanation: The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
</pre>

Note:<br>
The size of the input 2D-array will be between 3 and 1000.<br>
Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.<br>

Update (2017-09-26):<br>
We have overhauled the problem description + test cases and specified clearly the graph is an undirected graph. For the directed graph follow up please see Redundant Connection II). We apologize for any inconvenience caused.<br>

## Thinking
1. Use string of "index of array" to stand for the union, which we can view as connection "0","1","2" ...<br>
2. When every we came to see a connection, we first check what connection does the two points belong to (parent of connection), if we found a connection then we recursively check what does this connection belongs, until we find a connection that is not in the parent dicitonary (the most outer union of other connections最近其包含大家的union). Then that connection now belongs to the connection we are now in (merge connection as a new union)<br>
3. If we can't find parent for node then it belongs to the current index connection (current union)<br>
4. The latest met connection, will be the parent connection of the previous connected all connections (root connection)


## Coding
Time: O(nlogn); </br>
Space: O(n)
```python

class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        def find(v):                         # find the root of the tree
            return find(parent[v]) if v in parent else v

        parent = {}                          # record the child: parent relation in the forest, it can be node:parent_connection or connection: parent_connection 
        for i,p in enumerate(edges):
            r1, r2 = map(find, p)
            print(parent)
            print("r1:",r1,",r2:",r2)
            if r1 == r2:
                return p
            else:
                parent[r1] = parent[r2] = str(i) # use str(i) as the tree label, "1" doesn't mean node 1, it is connection '1'
```

