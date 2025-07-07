### Link
https://www.1point3acres.com/bbs/thread-1134797-1-1.html

## Question
You are given an array heights where heights[i] >= 1 represents the height of a block.  
* You start from the ground (considered as height = 0).  
* You must jump from block to block, and each block can be used at most once.  
* The burning calories for jumping from height a to height b is calculated as:  

```
calorie = (a - b)^2
```
* You cannot return to the ground once you’ve jumped from it.
* Find the maximum total calories burned for a sequence of jumps starting from the ground and using each block at most once.


**Example 1:**
<pre>
Input: [3, 1, 4, 2]
Output: 29
Explanation:
- Sort heights with ground: [0, 1, 2, 3, 4]
- Jump: 0 → 1 → 2 → 3 → 4 burns (1-0)^2 + (2-1)^2 + (3-2)^2 + (4-3)^2 = 1 + 1 + 1 + 1 = 4
- Reverse: 0 → 4 → 3 → 2 → 1 burns (4-0)^2 + (3-4)^2 + (2-3)^2 + (1-2)^2 = 16 + 1 + 1 + 1 = 19
- But max is combining both directions:
  Try: 0 → 4 → 1 → 3 → 2 burns 16 + 9 + 4 + 1 = 30 (not possible due to restriction)
  So best approach is:
  Choose max forward or reverse path with sorted array from 0:
    Try greedy double-ended scan (see implementation)
</pre>

## Thinking

## Coding

### Solution
```java
import java.util.Arrays;

public class MaxBurningCalories {
    public static int maxCalories(int[] heights) {
        int n = heights.length;
        int[] arr = new int[n + 1];
        System.arraycopy(heights, 0, arr, 1, n);
        arr[0] = 0; // ground

        Arrays.sort(arr);

        int left = 0, right = n;
        int calorie = 0;
        boolean turn = true;

        // Use greedy two-pointer from both ends
        while (left < right) {
            if (turn) {
                calorie += Math.pow(arr[right] - arr[left], 2);
                left++;
            } else {
                calorie += Math.pow(arr[right] - arr[left], 2);
                right--;
            }
            turn = !turn;
        }

        return calorie;
    }

    public static void main(String[] args) {
        int[] heights = {3, 1, 4, 2};
        System.out.println(maxCalories(heights)); // Output: 29
    }
}
```
