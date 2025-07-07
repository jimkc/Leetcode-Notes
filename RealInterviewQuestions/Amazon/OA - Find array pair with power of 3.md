### Question
Given an array of integer array. Return the number of pairs which is i and j (i < j) and the sum of the pair array[i] + array[j] is the power of 3.

```java
import java.util.HashSet;
import java.util.Set;

public class PowerOfThreePairs {

    public static int countPowerOfThreePairs(int[] arr) {
        int count = 0;
        // Change the Set to store Long objects
        Set<Long> powersOfThree = new HashSet<>();

        long power = 1;
        // Generate powers of 3 up to 2 * Integer.MAX_VALUE
        // the max of two number adding together is 2 num are all Integer.MAX_VALUE
        while (power <= 2L * Integer.MAX_VALUE) {
            // Add the long directly to the Set<Long>
            powersOfThree.add(power); 
            // Uses divsion to prevent overflow, its the same as power*3 <= Long.MAX_VALUE
            if (Long.MAX_VALUE / 3 < power) {
                break; // Prevent overflow before next multiplication
            }
            power *= 3;
        }

        for (int i = 0; i < arr.length; i++) {
            for (int j = i + 1; j < arr.length; j++) {
                long sum = (long) arr[i] + arr[j]; // Sum is already a long

                // Check if the long sum is positive and present in the Set<Long>
                if (sum > 0 && powersOfThree.contains(sum)) {
                    count++;
                }
            }
        }

        return count;
    }

    public static void main(String[] args) {
        // Test cases remain the same.
        int[] arr1 = {1, 2, 3, 4, 5, 6, 7, 8};
        System.out.println("Array: " + java.util.Arrays.toString(arr1));
        System.out.println("Number of pairs with sum as power of 3: " + countPowerOfThreePairs(arr1));

        int[] arr2 = {1, 3, 5, 7, 9, 27};
        System.out.println("\nArray: " + java.util.Arrays.toString(arr2));
        System.out.println("Number of pairs with sum as power of 3: " + countPowerOfThreePairs(arr2));

        int[] arr3 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27};
        System.out.println("\nArray: " + java.util.Arrays.toString(arr3));
        System.out.println("Number of pairs with sum as power of 3: " + countPowerOfThreePairs(arr3));

        int[] arr4 = {-1, 4};
        System.out.println("\nArray: " + java.util.Arrays.toString(arr4));
        System.out.println("Number of pairs with sum as power of 3: " + countPowerOfThreePairs(arr4));

        int[] arr5 = {-10, 19};
        System.out.println("\nArray: " + java.util.Arrays.toString(arr5));
        System.out.println("Number of pairs with sum as power of 3: " + countPowerOfThreePairs(arr5));

        // Additional test case to demonstrate the need for Set<Long>
        int[] arr6 = {Integer.MAX_VALUE, 1339483648}; // Integer.MAX_VALUE + 1339483648 = 3486967295 (approx 3^20)
        System.out.println("\nArray: " + java.util.Arrays.toString(arr6));
        System.out.println("Number of pairs with sum as power of 3: " + countPowerOfThreePairs(arr6)); // Expected: 1 (sum is ~3^20)
                                                                                                        // Note: 1339483648 is a bit large,
                                                                                                        // Actual sum Integer.MAX_VALUE + (3^20 - Integer.MAX_VALUE)
                                                                                                        // Let's find an exact match for 3^20
        int neededValueFor3_20 = (int)(3486784401L - Integer.MAX_VALUE); // 3^20 - Integer.MAX_VALUE
        System.out.println("Needed value for 3^20: " + neededValueFor3_20); // 1339297539
        int[] arr7 = {Integer.MAX_VALUE, neededValueFor3_20};
        System.out.println("\nArray: " + java.util.Arrays.toString(arr7));
        System.out.println("Number of pairs with sum as power of 3: " + countPowerOfThreePairs(arr7)); // Expected: 1 (sum = 3^20)
    }
}
```

### Similar question
https://leetcode.com/discuss/post/872882/quora-coding-challenge-number-of-pairs-w-7hw9/



