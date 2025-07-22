### Link
https://www.1point3acres.com/bbs/thread-1134035-1-1.html

### Question
You're designing an application consisting of multiple interconnected services, where each service may depend on one or more others, forming a directed acyclic graph (DAG). One service acts as the entrypoint, handling incoming user requests. 

Upon receiving a request, this entrypoint invokes its dependencies, which in turn call their own dependencies, and so forth.
The load factor of a service is defined as the total load it must handle when the entrypoint receives a single unit of load. 

All upstream services trigger their dependencies simultaneously, and if multiple dependency paths lead to the same service, the load on that service is cumulative.

You're given a list of service definitions in the format:
<service_name>=<dependency_1>,<dependency_2>,...
Services with no dependencies appear as <service_name>=. Dependencies not defined in the list should be disregarded. Each service appears exactly once.

Given a specified entrypoint (which is guaranteed to be listed), compute the worst-case load factor for every service that can be reached from it, including the entrypoint itself. Return the result as an array of strings formatted as
<service_name>*<load_factor>  

sorted lexicographically by service name. Only include services reachable from the entrypoint.  

Constraints:
- Each service definition is unique.
- Dependencies that are not explicitly defined in the list should be ignored.

### Solution 1 - BFS
The problem can be modeled as a directed acyclic graph (DAG), where services are nodes and dependencies are directed edges. The load factor of a service is the sum of the loads from all services that depend on it. This structure suggests that we should process the services in a topological order to ensure that a service's load is calculated only after all its predecessors' loads are finalized.

The overall approach is:

1. Parse and Build Graph: Construct a graph representation (an adjacency list) from the input strings. We'll also calculate the "in-degree" (number of incoming dependencies) for each service, which is essential for the topological sort.

2. Initialize Loads and Queue: Use a queue for the topological sort, initially populating it with all services that have an in-degree of 0 (i.e., no dependencies). We'll use a map to store the load factors, setting the entrypoint's initial load to 1 and all others to 0.

3. Propagate Loads: Process the services in topological order. For each service, add its load to all of its direct dependencies. When a dependency's in-degree becomes 0, add it to the queue to be processed.

4. Filter and Format: After computing the loads for all services, filter the results to include only those reachable from the entrypoint. Finally, format the output strings and sort them lexicographically.

```java
import java.util.*;
import java.util.stream.Collectors;

public class ServiceLoadCalculator {

    /**
     * Calculates the load factor for all services reachable from a given entrypoint.
     *
     * @param serviceDefinitions An array of strings defining services and their dependencies.
     * Format: "service_name=dep1,dep2,..."
     * @param entrypoint         The name of the entrypoint service.
     * @return A sorted array of strings with the format "service_name*load_factor".
     */
    public String[] calculateLoadFactors(String[] serviceDefinitions, String entrypoint) {
        // Step 1: Parse definitions and build the graph representation.
        Map<String, List<String>> adjList = new HashMap<>();
        // count how many pre-service it needs to finish first
        Map<String, Integer> inDegree = new HashMap<>();
        Set<String> allServices = new HashSet<>();

        // Initialize all potential services from definitions.
        for (String def : serviceDefinitions) {
            String serviceName = def.split("=", 2)[0];
            allServices.add(serviceName);
            adjList.putIfAbsent(serviceName, new ArrayList<>());
            inDegree.putIfAbsent(serviceName, 0);
        }

        // Populate the adjacency list and in-degree map.
        for (String def : serviceDefinitions) {
            String[] parts = def.split("=", 2);
            String serviceName = parts[0];
            if (parts.length > 1 && !parts[1].isEmpty()) {
                String[] dependencies = parts[1].split(",");
                for (String dep : dependencies) {
                    // Only consider dependencies that are defined services.
                    if (allServices.contains(dep)) {
                        adjList.get(serviceName).add(dep);
                        inDegree.put(dep, inDegree.get(dep) + 1);
                    }
                }
            }
        }

        // Step 2: Initialize loads and the queue for topological sort.
        Map<String, Integer> loadFactors = new HashMap<>();
        Queue<String> queue = new LinkedList<>();

        for (String service : allServices) {
            loadFactors.put(service, 0);
            if (inDegree.get(service) == 0) {
                queue.add(service);
            }
        }

        // The entrypoint receives one unit of load externally.
        if (allServices.contains(entrypoint)) {
            loadFactors.put(entrypoint, 1);
        } else {
            // If the entrypoint isn't a defined service, there's nothing to compute.
            return new String[0];
        }


        // Step 3: Propagate loads using topological sort (Kahn's algorithm).
        while (!queue.isEmpty()) {
            String currentService = queue.poll();
            int currentLoad = loadFactors.get(currentService);

            // If a service has no load, it can't propagate any.
            if (currentLoad == 0) {
                continue;
            }

            for (String dependentService : adjList.getOrDefault(currentService, new ArrayList<>())) {
                // Add the current service's load to its dependent service.
                loadFactors.put(dependentService, loadFactors.get(dependentService) + currentLoad);
                
                // Decrement the in-degree of the dependent service.
                inDegree.put(dependentService, inDegree.get(dependentService) - 1);

                // If in-degree becomes 0, it's ready to be processed.
                if (inDegree.get(dependentService) == 0) {
                    queue.add(dependentService);
                }
            }
        }

        // Step 4: Filter for services reachable from the entrypoint and format the output.
        Set<String> reachableServices = findReachableServices(entrypoint, adjList, allServices);
        
        List<String> resultList = new ArrayList<>();
        for (String service : reachableServices) {
            resultList.add(service + "*" + loadFactors.get(service));
        }

        // Sort the results lexicographically by service name.
        Collections.sort(resultList);

        return resultList.toArray(new String[0]);
    }

    /**
     * Performs a graph traversal (BFS) to find all nodes reachable from a start node.
     */
    private Set<String> findReachableServices(String startNode, Map<String, List<String>> adjList, Set<String> allServices) {
        if (!allServices.contains(startNode)) {
            return Collections.emptySet();
        }

        Set<String> reachable = new HashSet<>();
        Queue<String> queue = new LinkedList<>();
        
        queue.add(startNode);
        reachable.add(startNode);

        while (!queue.isEmpty()) {
            String current = queue.poll();
            for (String neighbor : adjList.getOrDefault(current, new ArrayList<>())) {
                if (reachable.add(neighbor)) { // .add() returns true if the element was new
                    queue.add(neighbor);
                }
            }
        }
        return reachable;
    }
}
```

### Solution - DFS
```java
import java.util.*;

public class ServiceLoadCalculatorDFS {

    private Map<String, List<String>> parentList;
    private String entrypoint;
    private Map<String, Integer> memo;

    /**
     * Calculates the load factor for all services reachable from a given entrypoint
     * using a DFS with memoization approach.
     *
     * @param serviceDefinitions An array of strings defining services and their dependencies.
     * @param entrypoint         The name of the entrypoint service.
     * @return A sorted array of strings with the format "service_name*load_factor".
     */
    public String[] calculateLoadFactors(String[] serviceDefinitions, String entrypoint) {
        // Step 1: Parse and build both forward and reversed (parent) graphs.
        Map<String, List<String>> adjList = new HashMap<>();
        this.parentList = new HashMap<>();
        Set<String> allServices = new HashSet<>();

        // Initialize all services to create nodes in our graphs.
        for (String def : serviceDefinitions) {
            String serviceName = def.split("=", 2)[0];
            allServices.add(serviceName);
            adjList.putIfAbsent(serviceName, new ArrayList<>());
            parentList.putIfAbsent(serviceName, new ArrayList<>());
        }

        // Populate the adjacency lists.
        for (String def : serviceDefinitions) {
            String[] parts = def.split("=", 2);
            String serviceName = parts[0];
            if (parts.length > 1 && !parts[1].isEmpty()) {
                String[] dependencies = parts[1].split(",");
                for (String dep : dependencies) {
                    if (allServices.contains(dep)) {
                        adjList.get(serviceName).add(dep); // Forward graph
                        parentList.get(dep).add(serviceName); // Reversed (parent) graph
                    }
                }
            }
        }

        // Step 2: Find all services reachable from the entrypoint.
        Set<String> reachableServices = findReachableServices(entrypoint, adjList, allServices);
        if (reachableServices.isEmpty()) {
            return new String[0];
        }

        // Step 3: Initialize state for DFS and calculate loads.
        this.entrypoint = entrypoint;
        this.memo = new HashMap<>(); // Memoization cache for our DFS

        for (String service : reachableServices) {
            getLoad(service); // This populates the memo cache for all reachable services
        }

        // Step 4: Format and sort the results from the cache.
        List<String> resultList = new ArrayList<>();
        for (String service : reachableServices) {
            resultList.add(service + "*" + memo.get(service));
        }

        Collections.sort(resultList);
        return resultList.toArray(new String[0]);
    }

    /**
     * Recursive DFS function to compute the load of a single service.
     */
    private int getLoad(String service) {
        // Base case: If already computed, return the cached value.
        if (memo.containsKey(service)) {
            return memo.get(service);
        }

        // Recursive step: Load is base load + sum of loads from all parents.
        // The entrypoint gets an external load of 1.
        int currentLoad = service.equals(this.entrypoint) ? 1 : 0;
        
        // Sum loads from all parents by making recursive calls.
        for (String parent : parentList.getOrDefault(service, new ArrayList<>())) {
            currentLoad += getLoad(parent);
        }

        // Cache the result before returning.
        memo.put(service, currentLoad);
        return currentLoad;
    }

    /**
     * Performs a graph traversal (BFS) to find all nodes reachable from a start node.
     */
    private Set<String> findReachableServices(String startNode, Map<String, List<String>> adjList, Set<String> allServices) {
        if (!allServices.contains(startNode)) {
            return Collections.emptySet();
        }
        Set<String> reachable = new HashSet<>();
        Queue<String> queue = new LinkedList<>();
        queue.add(startNode);
        reachable.add(startNode);
        while (!queue.isEmpty()) {
            String current = queue.poll();
            for (String neighbor : adjList.getOrDefault(current, new ArrayList<>())) {
                if (reachable.add(neighbor)) {
                    queue.add(neighbor);
                }
            }
        }
        return reachable;
    }
}
```

### Example go through
Of course. Let's trace the execution with your example using the **DFS with Memoization** approach.

### 1. Setup

* **Services & Dependencies**: `["a=b,c", "b=c,d", "c=d", "e=b"]`
* **Entrypoint**: `a`

First, we determine the graph structure and which services are reachable from the entrypoint `a`.

* **Forward Graph (Dependencies):**
    * `a` depends on `b`, `c`
    * `b` depends on `c`, `d`
    * `c` depends on `d`
    * `e` depends on `b`
* **Reversed Graph (Parents):**
    * `b` is a parent of `a` and `e`.
    * `c` is a parent of `a` and `b`.
    * `d` is a parent of `b` and `c`.
* **Reachable Services from `a`**:
    * From `a`, we can reach `b` and `c`.
    * From `b`, we can reach `c` and `d`.
    * The full set of reachable services is `{a, b, c, d}`. The service `e` is not reachable from `a` and will be excluded from the final output.

---
### 2. Load Calculation (DFS)

The system will recursively calculate the load for each reachable service. We'll use a cache (memo) to store results.

1.  **Calculate Load for `a`:**
    * `getLoad("a")` is called.
    * `a` is the **entrypoint**, so its base load is **1**.
    * `a` has no parents.
    * **Result**: `load(a) = 1`. This value is cached.

2.  **Calculate Load for `b`:**
    * `getLoad("b")` is called.
    * Base load is 0.
    * Parents of `b` are `a` and `e`. We only consider parents that can feed into the final calculation (which our algorithm does automatically).
    * It recursively calls `getLoad("a")`, which returns the cached value of `1`.
    * The load from parent `e` is not part of the subgraph reachable from `a`, so its contribution is effectively 0 in this context.
    * **Result**: `load(b) = load(a) = 1`. This is cached.

3.  **Calculate Load for `c`:**
    * `getLoad("c")` is called.
    * Base load is 0.
    * Parents of `c` are `a` and `b`.
    * It calls `getLoad("a")` (returns cached `1`) and `getLoad("b")` (returns cached `1`).
    * **Result**: `load(c) = load(a) + load(b) = 1 + 1 = 2`. This is cached.

4.  **Calculate Load for `d`:**
    * `getLoad("d")` is called.
    * Base load is 0.
    * Parents of `d` are `b` and `c`.
    * It calls `getLoad("b")` (returns cached `1`) and `getLoad("c")` (returns cached `2`).
    * **Result**: `load(d) = load(b) + load(c) = 1 + 2 = 3`. This is cached.

---
### 3. Final Output

The loads for all services reachable from `a` have been calculated. Now we format and sort them.

* `a` -> `a*1`
* `b` -> `b*1`
* `c` -> `c*2`
* `d` -> `d*3`

After sorting lexicographically, the final result is:
**`["a*1", "b*1", "c*2", "d*3"]`**
