### Link
https://www.1point3acres.com/bbs/thread-1134727-1-1.html

## Question
Amazon engineers are optimizing an array of n high-performance printers. You are given:
* pages[i]: number of pages printer i can print.
* threshold[i]: the suspension threshold for printer i.

Each printer starts idle and is activated one-by-one.  

**Suspension Rule:**
* At the moment when x printers are active, any printer such that threshold[i] ≤ x will print its pages, but then gets suspended immediately. And all other printers even not active yet but with threshold <= x needs to get suspended as well.
* This means the order of activation matters — once a threshold condition is met, some printers may not get a chance to be activated or will be suspended quickly.

Determine the maximum total number of pages that can be printed by carefully choosing the best activation order.

Note: Printers can be suspended before they get activate.
✅ You must activate small-threshold printers before active count reaches their threshold, or you lose them forever.

🧮 You can only activate printers in an order where their threshold > number of printers activated so far, or you must take them immediately before they get blocked.

❌ Once activeCount reaches a printer's threshold and it is not yet activated, it's dead — it cannot be used anymore.

**Example 1:**
<pre>
Input:
pages     = [10, 5, 8]
threshold = [2, 1, 3]

Output:
18

Explanation:
One optimal activation order is:
- Activate printer 1 (threshold=1, pages=5) → active count = 1 → no suspension
- Activate printer 0 (threshold=2, pages=10) → active count = 2 → no suspension
- Activate printer 2 (threshold=3, pages=8) → active count = 3
  → Now threshold[i] ≤ 3: printer 0, 1, 2 all satisfy threshold ≤ 3, so they all suspend after printing.

All three printed before suspension. Total = 10 + 5 + 8 = **23**

But a worse order may trigger suspension earlier.
</pre>

## Thinking
Sort the printers by threshold (ascending). Then, simulate activating printers one by one:
* If the number of currently active printers is x, then any printer whose threshold ≤ x will suspend.
* By sorting, we prioritize printers with high page count early, and delay those with low thresholds.

We active the ones with smaller threshold first to make sure that the low threshold printers get chances to activate and print before they are suspended.

## Coding

### Solution
```java
import java.util.*;

public class MaxPrintedPages {

    public static int maxPrintedPages(int[] pages, int[] threshold) {
        int n = pages.length;

        // Pair: [threshold, pages]
        List<int[]> printers = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            printers.add(new int[]{threshold[i], pages[i]});
        }

        // Sort by threshold ascending
        Collections.sort(printers, new Comparator<int[]>(){
            @Override
            public int compare(int[] a, int[] b) {
                if (a[0] != b[0]) {
                    return Integer.compare(a[0], b[0]);
                }
                return Integer.compare(b[1], a[1]);
            }
        })

        Queue<int[]> activeQueue = new LinkedList<>();
        int totalPages = 0;
        int activeCount = 0;
        int index = 0;

        while (index < n) {
            // Skip not-yet-activated printers who already missed their window
            while (index < n && printers.get(index)[0] <= activeCount) {
                index++; // They never get to activate or print
            }

            // Activate a new printer
            activeQueue.offer(printers.get(index));
            activeCount++;
            index++;

            // Suspension check happens AFTER activation
            // Suspend all printers in activeQueue whose threshold ≤ activeCount
            int suspendedThisRound = 0;
            while (!activeQueue.isEmpty() && activeQueue.peek()[0] <= activeCount) {
                totalPages += activeQueue.poll()[1];
                suspendedThisRound++;
            }
            activeCount -= suspendedThisRound; // Batch update
        }

        // Remaining printers that never suspended
        while (!activeQueue.isEmpty()) {
            totalPages += activeQueue.poll()[1];
        }

        return totalPages;
    }

    public static void main(String[] args) {
        int[] pages = {10, 5, 8, 20};
        int[] threshold = {2, 1, 3, 2};
        System.out.println(maxPrintedPages(pages, threshold)); // Output: 43
    }
}
```
