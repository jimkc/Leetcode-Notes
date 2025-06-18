###Link
https://www.1point3acres.com/bbs/thread-1120151-1-1.html

###Question
An array of integers is almost sorted if at most one element can be deleted from it to make it perfectly sorted in ascending order.

For example:
```
[2, 1, 7], [13], [9, 2], and [1, 5, 6] are almost sorted because they have at most one element out of place.
[4, 2, 1], [1, 2, 6, 4, 3] are not because they have more than one element out of place.
```

Task:  
Given an array of n unique integers, determine the minimum number of elements to remove so it becomes almost sorted.  

Example:
```
arr = [3, 4, 2, 5, 1]
Remove 2 → [3, 4, 5, 1] (almost sorted)
Remove 1 → [3, 4, 2, 5] (almost sorted)
Answer: 1
```
Function Signature:
```java
int minDeletions(int[] arr)
```

Returns:  
The minimum number of items that must be deleted to make it almost sorted.

###Solution
Time: O(nlogn).

In this solution dp[i] does not replsent the LIS, it is the smallest tail element for the LIS for length i+1.  
dp[i] represents the smallest possible tail value of an increasing subsequence of length i + 1.

An example as below:
```
arr = [10, 9, 2, 5, 3, 7, 101, 18]

| Number | Action                                                            | dp\[]           |
| ------ | ----------------------------------------------------------------- | --------------- |
| 10     | `dp` is empty → insert at end                                     | \[10]           |
| 9      | Find first index `i` in dp where `dp[i] >= 9` → index 0 → replace | \[9]            |
| 2      | Replace index 0 (because 2 < 9)                                   | \[2]            |
| 5      | 5 > 2 → append                                                    | \[2, 5]         |
| 3      | 3 ≤ 5 → replace index 1                                           | \[2, 3]         |
| 7      | 7 > 3 → append                                                    | \[2, 3, 7]      |
| 101    | 101 > 7 → append                                                  | \[2, 3, 7, 101] |
| 18     | 18 < 101 → replace index 3                                        | \[2, 3, 7, 18]  |
```

dp can work to help calculate the LIS because if we continue update dp[i] with the smallest value from subarray then if dp[i+1] is larger then all previous dp values that means it is the current smallest tail element for the subarray + 1 length.  
And if we see a smaller one we updated to the next smaller elements index in dp to make that sub LIS smaller.

```java
public class AlmostSortedLIS {

    // Finds minimum deletions to make the array almost sorted
    public static int minDeletions(int[] arr) {
        int n = arr.length;
        int lis = lengthOfLIS(arr);
        return Math.max(0, n - (lis + 1));  // allow 1 bad element
    }

    // O(n log n) implementation of Longest Increasing Subsequence (LIS)
    private static int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        int length = 0; // length of current LIS

        for (int num : nums) {
            int pos = binarySearch(dp, 0, length - 1, num);

            dp[pos] = num;

            // if num is greater than any in dp, it extends LIS
            if (pos == length) {
                length++;
            }
        }

        return length;
    }

    // Finds the first index in dp[low..high] where dp[i] >= target
    private static int binarySearch(int[] dp, int low, int high, int target) {
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (dp[mid] < target) {
                low = mid + 1;
            } else {
            	// even if the num == target, the high pointer will move left
                high = mid - 1;
            }
        }
        return low;
    }

    // Test cases
    public static void main(String[] args) {
        int[] arr1 = {3, 4, 2, 5, 1};      // LIS = 3 → deletions = 5 - (3+1) = 1
        int[] arr2 = {4, 2, 1};            // LIS = 1 → deletions = 3 - (1+1) = 1
        int[] arr3 = {1, 3, 2, 4};         // LIS = 3 → deletions = 4 - (3+1) = 0
        int[] arr4 = {1, 2, 3, 4};         // LIS = 4 → deletions = 4 - 5 = 0
        int[] arr5 = {10, 1, 2, 3, 4};     // LIS = 4 → deletions = 5 - 5 = 0
        int[] arr6 = {10, 20, 5, 8, 25, 2}; // LIS = 3 → deletions = 6 - 4 = 2

        System.out.println(minDeletions(arr1)); // 1
        System.out.println(minDeletions(arr2)); // 1
        System.out.println(minDeletions(arr3)); // 0
        System.out.println(minDeletions(arr4)); // 0
        System.out.println(minDeletions(arr5)); // 0
        System.out.println(minDeletions(arr6)); // 2
    }
}

```