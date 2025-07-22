### Link
https://www.hack2hire.com/by-company/content/67e4438350b60b77022f381b?company=ROBINHOOD&type=ALGORITHM

### Question
(This question is a variation of the LeetCode question 1514. Path with Maximum Probability. If you haven't completed that question yet, it is recommended to solve it first.)  
  
Given a directed graph with n nodes labeled from 0 to n−1, where each edge represents an influence relationship between stocks and carries a multiplier value. Your task is to determine the maximum product of multipliers along any simple path (i.e., visiting each node at most once) from a given start node to a given end node.  
  
The graph is provided as a list of edges, where each edge is represented as [u, v, w], indicating a directed edge from node u to node v with a multiplier value w.  
  
Return the maximum product possible from the start node to the end node. If no such simple path exists from the start node to the end node, return -1.  
  
Constraints:  
* Each multiplier w is an integer in the range of [1, 105].  
* The start and end nodes are valid indices within the range [0, n - 1].  
* The graph may contain cycles, but each edge can be used at most once when calculating the product of multipliers.

Example 1:
```
Input: n = 5, edges = [[0, 1, 2], [1, 2, 3], [2, 1, 4], [1, 3, 5], [2, 4, 6], [4, 3, 10]], start = 0, end = 3
Output: 360
Explanation: There are two valid simple paths from node 0 to node 3:

* Path 0 → 1 → 3 gives a product of 2 × 5 = 10.
* Path 0 → 1 → 2 → 4 → 3 gives a product of 2 × 3 × 6 × 10 = 360.
The maximum product among these is 360.
```
Example 2:
```
Input: n = 4, edges = [[0, 1, 1], [1, 2, 2], [2, 1, 3], [1, 3, 4]], start = 0, end = 3
Output: 4
```
Example 3:
```
Input: n = 6, edges = [[0, 1, 2], [1, 2, 3], [2, 0, 4], [3, 4, 5], [4, 5, 6]], start = 0, end = 3
Output: -1
```

### Solution 1 - DFS
```java
import java.util.*;
import java.math.*;

class Edge {
    int next;
    int weight;

    Edge(int next, int weight) {
        this.next = next;
        this.weight = weight;
    }
}

class Solution {
    public int maxMultiplierProduct(int n, List<List<Integer>> edges, int start, int end) {
        // Build adjacency list: graph[u] = list of (v, w) edges
        List<Edge>[] graph = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
        }
        for (List<Integer> edge : edges) {
            int u = edge.get(0);
            int v = edge.get(1);
            int w = edge.get(2);
            graph[u].add(new Edge(v, w));
        }
        double maxProd = dfs(start, end, graph, new HashSet<>());
        return maxProd != -1 ? (int) maxProd : -1;
    }

    private double dfs(int curr, int end, List<Edge>[] graph, Set<Integer> visited) {
        // If we've reached the end node, return multiplicative identity (1)
        if (curr == end) {
            return 1.0;
        }
        visited.add(curr);
        double bestProd = -1.0;

        for (Edge edge : graph[curr]) {
            if (!visited.contains(edge.next)) {
                double productDownstream = dfs(edge.next, end, graph, visited);
                if (productDownstream != -1) {
                    // Multiply current edge weight by best downstream product
                    bestProd = Math.max(bestProd, edge.weight * productDownstream);
                }
            }
        }
        visited.remove(curr);  // Backtrack
        return bestProd;
    }
}

```


