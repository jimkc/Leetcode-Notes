## Link
https://www.1point3acres.com/bbs/thread-1134201-1-1.html

## Question
n Amazon's highly efficient logistics network, minimizing operational overhead and optimizing package routing is crucial to ensure smooth deliveries across various regions.  
  
The network consists of n warehouses, numbered from 1 to n, each strategically positioned at its corresponding index. Each warehouse has a specific storage capacity, given by warehouseCapacity[i] where warehouseCapacity[i] represents the capacity of the warehouse located at position i (assuming 1-based indexing).  
  
These warehouses are organized in a non-decreasing order of their storage capacities, meaning each warehouse's storage capacity is greater than or equal to the one before it.  
  
Each warehouse must establish a connection to a distribution hub positioned at a location greater than or equal to its own. This means that a warehouse at position i can only connect to a hub at position j, where j≥i.  
  
To optimize inventory routing, Amazon has placed a central high-capacity distribution hub at the last warehouse, located at position n. This hub serves as the main connection point for all warehouses if necessary. The cost of establishing a connection from warehouse i to a hub at position j is given by warehouseCapacity[j] - warehouseCapacity[i].  
  
Given q queries of the form (hubA,hubB), where two additional high-performance distribution hubs are deployed at warehouses hubA and hubB (1≤hubA < hubB < n), the goal is to calculate the minimum total connection cost for all warehouses, considering the nearest available distribution hub at or beyond each warehouse's position.  

Note:
* The problem statement assumes 1-based indexing for the warehouseCapacity array.
* Each query is independent, i.e., the changes made do not persist for subsequent queries.
* Each warehouse connects to the nearest hub at or beyond its position (either hubA, hubB, or the central hub at n) to minimize the overall connection cost.


### Example 1
```
n=5
warehouseCapacity=[3,6,10,15,20]

q=1
additionalHubs=[[2,4]]

In this case there is q=1 query with two additional high-performance distribution hubs at position hubA=2 and hubB=4.

This diagram shows the calculation of the total connection cost before additional distribution hubs are considered:
(Image showing diagram with warehouses and connections to hub at 20, costs 17, 14, 10, 5, 0)

After query 1: Additional distribution hubs installed at positions hubA=2 and hubB=4.
(Image showing diagram with warehouses and connections to hubs at 2, 4, 20)

Once additional distribution hubs are installed at positions hubA=2 and hubB=4:

1st warehouse will connect to the nearest available distribution hub at position 2, cost incurred = 6−3=3

2nd warehouse is itself a distribution hub, so cost incurred = 0

3rd warehouse will connect to the nearest available distribution hub at position 4, cost incurred = 15−10=5

4th and 5th warehouses are itself a distribution hub, so cost incurred = 0+0=0

Thus, the total connection cost is (6−3)+(0)+(15−10)+(0)+(0)=8.
Hence, return [8] as the answer.
```

### Function Description
Complete the function `getMinConnectionCost` in the editor below.

`getMinConnectionCost` has the following parameters:
`int warehouseCapacity[n]`: a non-decreasing array of integers representing the storage capacities of the warehouses
`int additionalHubs[q][2]`: an array where each element denotes the positions of two additional distribution hubs installed for a query

### Returns
`int[q]`: an array of integers, where each element i is the minimum total connection cost for the i-th query.

## Thinking
This problem requires careful consideration of the costs based on warehouse capacity and the dynamic placement of additional hubs for each query. Here's a Java solution:  
  
The core idea is that for each warehouse, we need to find the nearest available distribution hub at or beyond its position to minimize the connection cost. The available hubs are:  
  
1. The central hub at position n.  
2. The two additional hubs at hubA and hubB (if they are at or beyond the current warehouse's position).  

Since warehouseCapacity is non-decreasing, connecting to a hub further away will always result in a greater or equal capacity difference, thus a greater or equal cost. This means we can iterate through the warehouses and for each, efficiently find the best hub.  

**Algorithm for each query:**

1. Identify all potential hubs for the current query: This will be hubA, hubB, and n (the central hub). It's helpful to store these in a sorted list or array for efficient searching. Remember to adjust for 0-based vs 1-based indexing if necessary (the problem statement says 1-based for positions, but Java arrays are 0-based).

2. Iterate through each warehouse from 1 to n:
* For each warehouse i (0-indexed i-1 in Java array):
    * Find the smallest hub position j (from the potential hubs) such that j >= i.
    * The cost for this warehouse will be warehouseCapacity[j-1] - warehouseCapacity[i-1].
    * Add this cost to the total for the current query.

3. Store the total cost for the query.

**Optimization for finding the nearest hub:**

Since the potential hub positions are sorted (or can be easily sorted), we can use binary search (specifically Arrays.binarySearch or a manual binary search) or a two-pointer approach to quickly find the nearest suitable hub for each warehouse.

Let's use a TreeSet to store the active hub positions for each query. This automatically keeps them sorted and allows for efficient ceiling() queries (finding the smallest element greater than or equal to a given element).

## Coding

### Solution
```java
import java.util.TreeSet;
import java.util.Arrays; // Needed for Arrays.asList in main for testing convenience

public class Solution {

    /**
     * Completes the function getMinConnectionCost as per the function description.
     *
     * @param warehouseCapacity A non-decreasing array of integers representing
     * the storage capacities of the warehouses.
     * (1-based indexing conceptually, 0-based for array access in Java)
     * @param additionalHubs An array where each element denotes the positions of
     * two additional distribution hubs installed for a query.
     * (1-based indexing for hub positions)
     * @return An array of integers, where each element i is the minimum total
     * connection cost for the i-th query.
     */
    public int[] getMinConnectionCost(int[] warehouseCapacity, int[][] additionalHubs) {
        int n = warehouseCapacity.length;
        int q = additionalHubs.length;
        int[] result = new int[q];

        // The central hub is always at position n (1-based)
        int centralHubPosition = n;

        for (int i = 0; i < q; i++) {
            int hubA = additionalHubs[i][0];
            int hubB = additionalHubs[i][1];

            // Use a TreeSet to store current active hub positions, ensures sorted order
            // and efficient ceiling operations.
            // These positions are 1-based as per the problem.
            TreeSet<Integer> currentHubs = new TreeSet<>();
            currentHubs.add(centralHubPosition);
            currentHubs.add(hubA);
            currentHubs.add(hubB);

            long currentQueryTotalCost = 0; // Use long to prevent potential overflow during sum

            // Iterate through each warehouse (1-based index)
            for (int j = 1; j <= n; j++) {
                // Find the nearest available distribution hub at or beyond position 'j'.
                // The ceiling method of TreeSet finds the least element greater than or equal to the given element.
                Integer nearestHubPosition = currentHubs.ceiling(j);

                // Calculate cost for warehouse 'j'
                // Remember to convert 1-based positions to 0-based array indices for warehouseCapacity access.
                long cost = (long) warehouseCapacity[nearestHubPosition - 1] - warehouseCapacity[j - 1];
                currentQueryTotalCost += cost;
            }
            // Cast to int for the result array. This assumes the total cost will fit into an int.
            result[i] = (int) currentQueryTotalCost;
        }
        return result;
    }

    public static void main(String[] args) {
        Solution solver = new Solution();

        // Example from the problem description
        int[] warehouseCapacity = {3, 6, 10, 15, 20}; // Note: This should be {3, 6, 10, 15, 20} as per example
        int[][] additionalHubs = {{2, 4}};

        int[] costs = solver.getMinConnectionCost(warehouseCapacity, additionalHubs);
        System.out.println("Example Output: " + Arrays.toString(costs)); // Expected: [8]

        // Custom Test 1
        int[] wc2 = {1, 2, 3, 4, 5};
        int[][] ah2 = {{1, 3}, {2, 5}};
        int[] costs2 = solver.getMinConnectionCost(wc2, ah2);
        System.out.println("Custom Test 1 Output: " + Arrays.toString(costs2));
        // Expected for query [1,3]: Total cost = 2
        // Expected for query [2,5]: Total cost = 4
        // Overall Expected: [2, 4]
    }
}
```
