## Question
Amazon operates numerous warehouses, with each warehouse i holding inventory[i] units of a particular product. You and your co-worker are responsible for dispatching these items to fulfill customer orders, following a specific process:  
  
1. When dispatching from warehouse i, you begin by reducing the inventory of the ith warehouse by dispatch1 units.  
2. After your dispatch, your co-worker reduces the inventory by dispatch2 units.  
3. This process repeats until the inventory of the ith warehouse reaches zero or becomes negative (i.e., inventory[i] <= 0).  
4. For every warehouse that is emptied during your dispatch, you and your co-worker collectively earn 1 credit.  

Your co-worker has the option to skip their turn, but they can only do this a limited number of times, defined by skips.  
Your task is to determine the best strategy to maximize the total credits that both you and your co-worker can earn together. 

### Example 1
```
n = 6
inventory = [10, 6, 12, 8, 15, 1]
dispatch1 = 2
dispatch2 = 3
skips = 3

An optimal dispatch strategy is as follows:

1. Your co-worker skips 2 turns, allowing you to empty the inventory of the 1st warehouse.
2. Your co-worker doesn't skip any turns, and you empty the inventory of the 2nd warehouse.
3. Your co-worker doesn't skip any turns, and you empty the inventory of the 3rd warehouse.
4. Your co-worker skips 1 turn, and you drain the inventory of the 4th warehouse.
5. Your co-worker doesn't skip any turns, and they empty the inventory of the 5th warehouse.
6. Your co-worker doesn't skip any turns, and you empty the inventory of the 6th warehouse.

As a result, the 1st, 2nd, 3rd, 4th, and 6th warehouses were completely dispatched by you, and the two of you collectively earned 5 credits, which is the maximum possible in this scenario.

Hence, the answer is 5.
```

### Function Description
Function Description
Complete the function `getMaximumCredits` in the editor below.

`getMaximumCredits` has the following parameters:
* int inventory[n]: An array of integers denoting the inventory level of each warehouse.
* int dispatch1: An integer indicating your dispatch level per turn.
* int dispatch2: An integer indicating your co-worker's dispatch level per turn.
* int skips: An integer specifying the maximum number of times your co-worker can skip their turn.

### Return
* int: the maximum number of credits both of you can achieve collectively.

Constraints  
* 1 <= n <= 10^5
* 1 <= inventory[i] <= 10^9
* 1 <= dispatch1, dispatch2, skips <= 10^9


## Thinking


## Coding

### Solution
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Result {

    /*
     * Complete the 'getMaximumCredits' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     * 1. INTEGER_ARRAY inventory
     * 2. INTEGER dispatch1
     * 3. INTEGER dispatch2
     * 4. INTEGER skips
     */

    public static int getMaximumCredits(List<Integer> inventory, int dispatch1, int dispatch2, int skips) {
        // Use long for calculations to prevent potential integer overflow, as inputs can be large.
        long d1 = dispatch1;
        long d2 = dispatch2;
        long totalDispatchPerRound = d1 + d2;
        long availableSkips = skips;

        // This list will store the "cost" (in skips) to earn a credit for each warehouse.
        List<Long> skipCosts = new ArrayList<>();

        // --- Step 1: Calculate the skip cost for each warehouse ---
        for (int currentInventoryInt : inventory) {
            long currentInventory = currentInventoryInt;

            // This is the inventory left after full rounds of (you + co-worker) are completed.
            long remainder = currentInventory % totalDispatchPerRound;
            
            long amountToClearYourself;
            
            if (remainder == 0) {
                // If the inventory is a perfect multiple of (d1+d2), the co-worker
                // will make the final dispatch by default. To win the credit, you must
                // use skips to clear the final (d1 + d2) amount all by yourself.
                amountToClearYourself = totalDispatchPerRound;
            } else {
                // Otherwise, you only need to clear the final remainder amount by yourself.
                amountToClearYourself = remainder;
            }

            // If the amount you're responsible for is less than or equal to your dispatch
            // amount, you'll win on your turn naturally. No skips needed.
            if (amountToClearYourself <= d1) {
                skipCosts.add(0L);
            } else {
                // If the amount is larger, you need the co-worker to skip.
                // Calculate how many of YOUR turns are needed to clear the amount.
                // This formula is a common way to calculate ceiling division for integers.
                long turnsNeeded = (amountToClearYourself + d1 - 1) / d1;
                
                // The number of skips required is always one less than your number of turns.
                long cost = turnsNeeded - 1;
                skipCosts.add(cost);
            }
        }

        // --- Step 2: Greedily "buy" credits with your skip budget ---
        
        // Sort the costs so we can acquire the cheapest credits first.
        Collections.sort(skipCosts);

        int creditsEarned = 0;
        // Iterate through the sorted costs and spend your skips.
        for (long cost : skipCosts) {
            if (availableSkips >= cost) {
                availableSkips -= cost; // "Pay" the cost in skips.
                creditsEarned++;      // Earn the credit.
            } else {
                // If you can't afford this credit, you can't afford any subsequent ones either.
                break;
            }
        }

        return creditsEarned;
    }

}
```
