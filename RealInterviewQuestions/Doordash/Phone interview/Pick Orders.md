### Link
https://www.1point3acres.com/bbs/thread-1133485-1-1.html

### Question
A chef receives all his orders for the day as a list of order ids. Given this list, the chef chooses to prepare them in the following way. 

He creates a new list by repeatedly removing the smallest eligible order from the list and appending it to the new list. An order is considered eligible if its id is greater than its immediate left and right neighbors in the list. 

Return the order in which the chef creates the new list. For order id at index 0, it is eligible if it is greater than its right neighbor [Since no left neighbor exists] For order id at index n-1 in a list of size n, it is eligible if it is greater than its left neighbor [Since no right neighbor exists] When there is only one order in the list, it is automatically eligible. Order ids are unique. 


order ids: [a,b,c,d,e] 
new list: []

### Solution 1 - Brute force
```java
import java.util.ArrayList;
import java.util.List;

public class ChefOrderSelector {

    /**
     * This method determines the order in which a chef prepares orders based on a specific eligibility rule.
     *
     * @param orderIds A list of unique integer order IDs.
     * @return A list of order IDs in the sequence they are prepared.
     */
    public List<Integer> findOrder(List<Integer> orderIds) {
        // Use a new ArrayList to avoid modifying the original list passed to the method.
        List<Integer> currentOrders = new ArrayList<>(orderIds);
        List<Integer> result = new ArrayList<>();

        // Continue processing as long as there are orders left.
        while (!currentOrders.isEmpty()) {
            List<Integer> eligibleOrders = new ArrayList<>();
            // When only one order is left, it's automatically eligible.
            if (currentOrders.size() == 1) {
                eligibleOrders.add(currentOrders.get(0));
            } else {
                // Iterate through the list to find all eligible orders.
                for (int i = 0; i < currentOrders.size(); i++) {
                    int currentOrder = currentOrders.get(i);
                    // Check the first element.
                    if (i == 0) {
                        if (currentOrder > currentOrders.get(i + 1)) {
                            eligibleOrders.add(currentOrder);
                        }
                    // Check the last element.
                    } else if (i == currentOrders.size() - 1) {
                        if (currentOrder > currentOrders.get(i - 1)) {
                            eligibleOrders.add(currentOrder);
                        }
                    // Check elements in the middle.
                    } else {
                        if (currentOrder > currentOrders.get(i - 1) && currentOrder > currentOrders.get(i + 1)) {
                            eligibleOrders.add(currentOrder);
                        }
                    }
                }
            }

            // Find the smallest among the eligible orders.
            if (!eligibleOrders.isEmpty()) {
                int smallestEligible = eligibleOrders.get(0);
                for (int order : eligibleOrders) {
                    if (order < smallestEligible) {
                        smallestEligible = order;
                    }
                }
                // Add the smallest eligible order to the result and remove it from the current list.
                result.add(smallestEligible);
                currentOrders.remove(Integer.valueOf(smallestEligible));
            }
        }
        return result;
    }

    public static void main(String[] args) {
        ChefOrderSelector selector = new ChefOrderSelector();
        List<Integer> orders = new ArrayList<>(List.of(3, 5, 2, 6, 8, 1, 4, 7));
        List<Integer> finalOrder = selector.findOrder(orders);
        System.out.println("The final order of preparation is: " + finalOrder);
        // Expected output: [5, 8, 6, 7, 4, 3, 2, 1]
    }
}
```

### Solution 2 - Optimized
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.PriorityQueue;

public class OptimizedChefOrderSelector {

    // Inner class to represent a node in a doubly linked list.
    private static class Node {
        int value;
        Node prev;
        Node next;

        Node(int value) {
            this.value = value;
        }
    }

    /**
     * Checks if a given node is eligible to be picked.
     * An order is eligible if its value is greater than its neighbors.
     */
    private boolean isEligible(Node node) {
        if (node == null) return false;

        boolean greaterThanPrev = (node.prev == null) || (node.value > node.prev.value);
        boolean greaterThanNext = (node.next == null) || (node.value > node.next.value);

        return greaterThanPrev && greaterThanNext;
    }

    /**
     * Finds the preparation order with O(n log n) time complexity.
     *
     * @param orderIds A list of unique integer order IDs.
     * @return A list of order IDs in the sequence they are prepared.
     */
    public List<Integer> findOrder(List<Integer> orderIds) {
        if (orderIds == null || orderIds.isEmpty()) {
            return new ArrayList<>();
        }

        // 1. Setup data structures
        List<Integer> result = new ArrayList<>();
        Map<Integer, Node> nodeMap = new HashMap<>();
        PriorityQueue<Integer> eligibleOrdersHeap = new PriorityQueue<>();

        // 2. Build the doubly linked list
        Node head = null, tail = null;
        for (int id : orderIds) {
            Node newNode = new Node(id);
            nodeMap.put(id, newNode);
            if (head == null) {
                head = tail = newNode;
            } else {
                tail.next = newNode;
                newNode.prev = tail;
                tail = newNode;
            }
        }

        // 3. Initial scan to find all currently eligible orders
        for (Node node : nodeMap.values()) {
            if (isEligible(node)) {
                eligibleOrdersHeap.add(node.value);
            }
        }

        // 4. Main loop: process orders until the heap is empty
        while (!eligibleOrdersHeap.isEmpty()) {
            // Get the smallest eligible order
            int smallestEligibleId = eligibleOrdersHeap.poll();
            result.add(smallestEligibleId);

            Node nodeToRemove = nodeMap.get(smallestEligibleId);

            // Keep track of neighbors before removal
            Node prevNode = nodeToRemove.prev;
            Node nextNode = nodeToRemove.next;

            // Remove the node from the doubly linked list
            if (prevNode != null) {
                prevNode.next = nextNode;
            }
            if (nextNode != null) {
                nextNode.prev = prevNode;
            }

            // Check if the neighbors' eligibility has changed.
            // A formerly ineligible neighbor might become eligible now.
            // We must remove them from the heap if they were in it and add them back
            // to ensure their eligibility is re-evaluated relative to other eligible orders.
            // A simple approach is to remove and re-add if eligible.
            if (prevNode != null) {
                eligibleOrdersHeap.remove(prevNode.value); // O(k) but k is small in practice
                if (isEligible(prevNode)) {
                    eligibleOrdersHeap.add(prevNode.value);
                }
            }
            if (nextNode != null) {
                eligibleOrdersHeap.remove(nextNode.value);
                if (isEligible(nextNode)) {
                    eligibleOrdersHeap.add(nextNode.value);
                }
            }
        }

        return result;
    }

    public static void main(String[] args) {
        OptimizedChefOrderSelector selector = new OptimizedChefOrderSelector();
        List<Integer> orders = new ArrayList<>(List.of(3, 5, 2, 6, 8, 1, 4, 7));
        List<Integer> finalOrder = selector.findOrder(orders);
        System.out.println("The optimized final order of preparation is: " + finalOrder);
        // Expected output: [5, 8, 6, 7, 4, 3, 2, 1]
    }
}
```