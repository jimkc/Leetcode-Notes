## Link
https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=1132096&highlight=amazon%2Boa

## Question
One of the products listed on Amazon Ecommerce is available in n sizes as indicated in the array size. The category manager recognizes that some of the sizes are repetitive and do not provide a good user experience. To make the best use of inventory, the product should be available in distinct sizes. The size of the i-th product, size[i], can be increased by one unit for an amount in the cost array, cost[i].  

Given the arrays size and cost for the product, find the minimal total cost in order to make all the sizes distinct.

### Example
```
Given n = 4, size = [2, 3, 3, 2] and cost = [2, 4, 5, 1].
-- It costs 7 units to make the prices distinct.  
We can also adjust size[1] to 5, which costs cost[1] * (5 - size[1]) = 8, and size[3] to 4, which costs cost[3] * (4 - size[3]) = 2. Therefore, the total cost is 8 + 2 = 10.
However, 7 is the minimum cost required to make all sizes distinct.
```

## Thinking
Allocate the larger costs first, and then go to the smaller cost ones later.

## Coding

### Solution
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class MinimalCostIncreasePackageSize {

    // Helper class to store size and cost together
    static class Product {
        int size;
        int cost;

        public Product(int size, int cost) {
            this.size = size;
            this.cost = cost;
        }
    }

    public static long getMinimalCost(int n, List<Integer> sizes, List<Integer> costs) {
        // 1. Create a list of Product objects
        List<Product> products = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            products.add(new Product(sizes.get(i), costs.get(i)));
        }

        // 2. Sort the products
        // Primary sort key: cost (descending)
        // Secondary sort key: size (ascending)
        Collections.sort(products, new Comparator<Product>() {
            @Override
            public int compare(Product p1, Product p2) {
                if (p1.cost != p2.cost) {
                    return Integer.compare(p2.cost, p1.cost); // Descending cost
                }
                return Integer.compare(p1.size, p2.size); // Ascending size
            }
        });

        long totalCost = 0;
        Set<Integer> seenSizes = new HashSet<>();

        // 3. Process and calculate cost
        for (Product p : products) {
            int currentSize = p.size;
            int currentCost = p.cost;

            // While the current size is already seen, increment it
            // and add the cost for each increment
            while (seenSizes.contains(currentSize)) {
                currentSize++;
                totalCost += currentCost;
            }
            // Once a unique size is found, add it to the set of seen sizes
            seenSizes.add(currentSize);
        }

        return totalCost;
    }

    public static void main(String[] args) {
        // Example from problem description
        int n1 = 4;
        List<Integer> size1 = List.of(2, 3, 3, 2);
        List<Integer> cost1 = List.of(2, 4, 5, 1);
        System.out.println("Example 1 Output: " + getMinimalCost(n1, size1, cost1)); // Expected: 7

        // Additional test case
        int n2 = 3;
        List<Integer> size2 = List.of(1, 1, 1);
        List<Integer> cost2 = List.of(10, 1, 5);
        // Sorted: (1,10), (1,5), (1,1)
        // Process (1,10): size=1, cost=0. seen={1}
        // Process (1,5): size=1 (conflict). size=2, cost+=5. seen={1,2}. total=5
        // Process (1,1): size=1 (conflict). size=2 (conflict). size=3, cost+=1. seen={1,2,3}. total=5+1=6
        // Total cost: 6
        System.out.println("Example 2 Output: " + getMinimalCost(n2, size2, cost2)); // Expected: 6
    }
}

```
