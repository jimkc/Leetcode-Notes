### Link
https://www.1point3acres.com/bbs/thread-1134350-1-1.html

### Question
```java
// At DoorDash, menus are updated daily even hourly to keep them up-to-date. Each menu can be regarded as a tree.
// When the merchant sends us the latest menu, can we calculate how many nodes has changed?
// Assume each Node structure is as below:

class Node {
    String key;
    int value;
    boolean active;
    List children;
}
// Assume there are no duplicate nodes with the same key.

// Output: Return the number of changed nodes in the tree.

// Example 1
// Existing Menu in our system:

// Existing tree
//                           a(1, T)
//                           /     \
//                    b(2, T)     c(3, T)
//                      / \              \
//            d(4, T) e(5, T)        f(6, T)
//
// Legend: In "a(1, T)", a is the key, 1 is the value, T is True for active status 

// New Menu sent by the Merchant:

// New tree
//                  a(1, T)
//                     |
//                  c(3, F)
//                     |
//                 f(66, T)
//
// Expected Answer: 5 Explanation: Node b, Node d, Node e are automatically set to inactive.
// The active status of Node c and the value of Node f changed as well.

// Example 2
// Existing Menu in our system:

// Existing tree
//                           a(1, T)
//                            /    \
//                    b(2, T)      c(3, T)
//                       / \           \
//             d(4, T) e(5, T)     g(7, T)
// New Menu sent by the Merchant:

// New tree
//                         a(1, T)
//                       /          \
//                 b(2, T)         c(3, T)
//                /  |    \              \
//       d(4, T) e(5, T)  f(6, T)      g(7, F)
//
// Expected Answer: 2 Explanation: Node f is a newly-added node. Node g changed from Active to inactive
```


### Solution
```java
import java.util.*;

/**
 * The main class to calculate menu changes.
 */
public class MenuComparer {

    /**
     * The Node structure for the menu tree, as specified.
     */
    static class Node {
        String key;
        int value;
        boolean active;
        List<Node> children;

        // Constructor for creating nodes easily in test cases.
        public Node(String key, int value, boolean active, List<Node> children) {
            this.key = key;
            this.value = value;
            this.active = active;
            this.children = children;
        }
    }

    /**
     * Calculates the number of changed nodes between an old and a new menu tree.
     * A change is defined as:
     * 1. A node's value or active status is different.
     * 2. A node exists in the new menu but not in the old one (added).
     * 3. A node exists in the old menu but not in the new one (deleted/inactive).
     *
     * @param oldRoot The root node of the existing menu tree.
     * @param newRoot The root node of the new menu tree.
     * @return The total number of changed nodes.
     */
    public int calculateChanges(Node oldRoot, Node newRoot) {
        // If one tree is null and the other isn't, all nodes in the non-null tree are changes.
        if (oldRoot == null && newRoot == null) return 0;
        if (oldRoot == null) return countAllNodes(newRoot);
        if (newRoot == null) return countAllNodes(oldRoot);

        // 1. Flatten the old tree into a map for efficient O(1) average time lookups.
        Map<String, Node> oldNodesMap = new HashMap<>();
        flattenTreeToMap(oldRoot, oldNodesMap);

        int changedCount = 0;
        Queue<Node> queue = new LinkedList<>();
        queue.add(newRoot);

        // 2. Traverse the new tree (BFS) and compare each node with the old tree's map.
        while (!queue.isEmpty()) {
            Node newNode = queue.poll();
            Node oldNode = oldNodesMap.get(newNode.key);

            if (oldNode != null) {
                // Case A: Node exists in both trees. Check for modifications.
                if (newNode.value != oldNode.value || newNode.active != oldNode.active) {
                    changedCount++;
                }
                // Mark this node as seen by removing it from the map.
                oldNodesMap.remove(newNode.key);
            } else {
                // Case B: Node is new (not in old map), so it's a change.
                changedCount++;
            }

            // Add children to the queue for traversal.
            if (newNode.children != null) {
                for (Node child : newNode.children) {
                    queue.add(child);
                }
            }
        }

        // 3. Any nodes left in the map were deleted. This counts as a change for each.
        changedCount += oldNodesMap.size();

        return changedCount;
    }

    /**
     * Helper method to traverse a tree and populate a map with its nodes.
     * @param root The root of the tree to flatten.
     * @param map The map to populate with key-node pairs.
     */
    private void flattenTreeToMap(Node root, Map<String, Node> map) {
        if (root == null) return;
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            Node current = queue.poll();
            map.put(current.key, current);
            if (current.children != null) {
                for (Node child : current.children) {
                    queue.add(child);
                }
            }
        }
    }

    /**
     * Helper method to count the total number of nodes in a tree.
     * @param root The root of the tree.
     * @return The total node count.
     */
    private int countAllNodes(Node root) {
        if (root == null) return 0;
        int count = 0;
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            Node current = queue.poll();
            count++;
            if (current.children != null) {
                for (Node child : current.children) {
                    queue.add(child);
                }
            }
        }
        return count;
    }

    public static void main(String[] args) {
        MenuComparer comparer = new MenuComparer();

        // --- Example 1 ---
        Node old_d = new Node("d", 4, true, null);
        Node old_e = new Node("e", 5, true, null);
        Node old_f = new Node("f", 6, true, null);
        Node old_b = new Node("b", 2, true, Arrays.asList(old_d, old_e));
        Node old_c = new Node("c", 3, true, Arrays.asList(old_f));
        Node oldRoot1 = new Node("a", 1, true, Arrays.asList(old_b, old_c));

        Node new_f = new Node("f", 66, true, null);
        Node new_c = new Node("c", 3, false, Arrays.asList(new_f));
        Node newRoot1 = new Node("a", 1, true, Arrays.asList(new_c));

        int changes1 = comparer.calculateChanges(oldRoot1, newRoot1);
        System.out.println("Example 1 Changes: " + changes1); // Expected: 5

        // --- Example 2 ---
        Node old_d2 = new Node("d", 4, true, null);
        Node old_e2 = new Node("e", 5, true, null);
        Node old_g2 = new Node("g", 7, true, null);
        Node old_b2 = new Node("b", 2, true, Arrays.asList(old_d2, old_e2));
        Node old_c2 = new Node("c", 3, true, Arrays.asList(old_g2));
        Node oldRoot2 = new Node("a", 1, true, Arrays.asList(old_b2, old_c2));

        Node new_d2 = new Node("d", 4, true, null);
        Node new_e2 = new Node("e", 5, true, null);
        Node new_f2 = new Node("f", 6, true, null);
        Node new_g2 = new Node("g", 7, false, null);
        Node new_b2 = new Node("b", 2, true, Arrays.asList(new_d2, new_e2, new_f2));
        Node new_c2 = new Node("c", 3, true, Arrays.asList(new_g2));
        Node newRoot2 = new Node("a", 1, true, Arrays.asList(new_b2, new_c2));

        int changes2 = comparer.calculateChanges(oldRoot2, newRoot2);
        System.out.println("Example 2 Changes: " + changes2); // Expected: 2
    }
}
```