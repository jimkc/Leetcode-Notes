## Link
https://www.1point3acres.com/bbs/thread-1134201-1-1.html

## Question
Amazon Shopping is running a reward collection event for its customers. There are n customers and the i^th customer has collected initialRewards[i] points so far.

One final tournament is to take place where the winner will be awarded n points, the runner-up will be awarded n - 1 points, the customer in third place will get n - 2 points, and so on until the one in last place gets 1 point.

Given an integer array initialRewards of length n, representing the initial reward points of the customer initially before the final tournament. Find the number of customers i (1 <= i <= n) such that, if the i^th customer wins the final tournament, i.e., they would have the highest total points.

Note: The total points for a customer are calculated as the sum of their initialRewards points and the points they would receive from the final tournament (with the winner receiving n points).

### Example 1
```
n = 3
initialRewards = [1, 3, 4]

Let's analyze for each customer if they win the final tournament, their total points would be:

If the 1st customer wins the final tournament, their total points would be: 1 + 3 = 4, but this is not the highest possible points in this case.
For example, if the 3rd customer with an initial reward of 4 comes 2nd, then they would achieve a total of: 4 + 2 = 6 points which is the higher than 1st customer points.

If the 2nd customer wins the final tournament, their total points would be: 3 + 3 = 6, and this is the highest total points in this case.
Even if the 3rd customer with an initial reward of 4 comes 2nd, then they would achieve a total of: 4 + 2 = 6 points which is not greater than 2nd customer points.

If the 3rd customer wins the final tournament, their total points would be: 4 + 3 = 7, and this is the highest total points, as there are no other customers that can achieve the total point of 7 in this case.
```

## Thinking


## Coding

### Solution
```java
import java.util.List;
import java.util.ArrayList; // Not strictly needed for List.of but good for general use

public class RewardCollection {

    /**
     * Finds the number of customers who, if they win the final tournament,
     * would have the highest total points among all customers.
     *
     * @param initialRewards A list of integers representing the initial reward points for each customer.
     * @return The count of customers who meet the condition.
     */
    public static int findWinningCustomers(List<Integer> initialRewards) {
        int n = initialRewards.size();

        // Base case: If there's only one customer, they automatically have the highest total points if they win.
        if (n == 1) {
            return 1;
        }

        // Step 1: Find the largest (max1) and second largest (max2) initial rewards.
        // Initialize with minimum possible integer values to ensure any reward is greater.
        int max1 = Integer.MIN_VALUE; // Stores the largest initial reward found so far
        int max2 = Integer.MIN_VALUE; // Stores the second largest initial reward found so far

        for (int reward : initialRewards) {
            if (reward > max1) {
                // If the current reward is greater than max1, it becomes the new max1.
                // The old max1 then becomes the new max2.
                max2 = max1;
                max1 = reward;
            } else if (reward > max2) {
                // If the current reward is not greater than max1, but it is greater than max2,
                // it becomes the new max2. This correctly handles cases where max1 has duplicates.
                max2 = reward;
            }
        }

        // Step 2: Initialize a counter for customers who meet the condition.
        int count = 0;

        // Step 3: Iterate through each customer to check the condition.
        for (int i = 0; i < n; i++) {
            int currentCustomerInitialReward = initialRewards.get(i);

            // Determine the maximum initial reward among other customers.
            // This represents the initial reward of the strongest competitor
            // if the current customer (customer i) wins the tournament.
            int maxInitialRewardOfOthers;
            if (currentCustomerInitialReward == max1) {
                // If the current customer has the overall highest initial reward (max1),
                // then the strongest competitor among the *remaining* N-1 customers
                // would be the one with the second highest initial reward (max2).
                maxInitialRewardOfOthers = max2;
            } else {
                // If the current customer does NOT have the overall highest initial reward
                // (i.e., their reward is less than max1), then the strongest competitor
                // among the *remaining* N-1 customers would still be the one with max1.
                maxInitialRewardOfOthers = max1;
            }

            // Apply the derived condition:
            // currentCustomerInitialReward + n >= maxInitialRewardOfOthers + (n-1)
            // Simplified: currentCustomerInitialReward >= maxInitialRewardOfOthers - 1
            if (currentCustomerInitialReward + n >= maxInitialRewardOfOthers + n - 1) {
                count++;
            }
        }

        // Step 4: Return the total count of eligible customers.
        return count;
    }

    public static void main(String[] args) {
        // Example from problem description:
        // n = 3, initialRewards = [1, 3, 4]
        // Expected output: 2 (Customers with initial rewards 3 and 4)
        List<Integer> rewards1 = List.of(1, 3, 4);
        System.out.println("Example 1 Output: " + findWinningCustomers(rewards1));

        // Additional test cases:

        // Case 1: n = 1, initialRewards = [10]
        // Expected output: 1 (The only customer always wins)
        List<Integer> rewards2 = List.of(10);
        System.out.println("Example 2 Output: " + findWinningCustomers(rewards2));

        // Case 2: n = 4, initialRewards = [5, 5, 3, 1]
        // max1=5, max2=5
        // Customer 0 (5): 5 >= 5-1 (true) -> count=1
        // Customer 1 (5): 5 >= 5-1 (true) -> count=2
        // Customer 2 (3): 3 >= 5-1 (false)
        // Customer 3 (1): 1 >= 5-1 (false)
        // Expected output: 2
        List<Integer> rewards3 = List.of(5, 5, 3, 1);
        System.out.println("Example 3 Output: " + findWinningCustomers(rewards3));

        // Case 3: n = 5, initialRewards = [10, 20, 30, 40, 50]
        // max1=50, max2=40
        // Customer (10): 10 >= 50-1 (false)
        // Customer (20): 20 >= 50-1 (false)
        // Customer (30): 30 >= 50-1 (false)
        // Customer (40): 40 >= 50-1 (false)
        // Customer (50): 50 >= 40-1 (true) -> count=1
        // Expected output: 1
        List<Integer> rewards4 = List.of(10, 20, 30, 40, 50);
        System.out.println("Example 4 Output: " + findWinningCustomers(rewards4));

        // Case 4: All initial rewards are the same
        // n = 3, initialRewards = [7, 7, 7]
        // max1=7, max2=7
        // C0 (7): 7 >= 7-1 (true)
        // C1 (7): 7 >= 7-1 (true)
        // C2 (7): 7 >= 7-1 (true)
        // Expected output: 3
        List<Integer> rewards5 = List.of(7, 7, 7);
        System.out.println("Example 5 Output: " + findWinningCustomers(rewards5));
    }
}
```
