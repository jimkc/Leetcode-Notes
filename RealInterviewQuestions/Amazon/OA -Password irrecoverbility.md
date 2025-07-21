## Question
Database security and authentication have become vital due to the increasing number of cyberattacks every day. Amazon has created a team for the analysis of various types of cyberattacks. In one such analysis, the team finds a virus that attacks the user passwords. The virus designed has an attacking rule defined by attackOrder, which is a permutation of length n. In the ith second of the attack, the virus attacks at the attackOrder[i]th character of the password, replacing it with the malicious character '*', i.e., password[attackOrder[i]] = '*' after the ith second.

The password is said to be irrecoverable when the number of substrings of the password containing at least one malicious character '*' becomes greater than or equal to m. In order to estimate the threat posed by the virus, the team wishes to find the minimum time after which the password becomes irrecoverable.

Note:
* If the password is irrecoverable at the start, report 1 as the answer.
* A substring of a string s is a contiguous segment of that string.

### Example 1
```
Sample Case 0
password: "abc" (n = 3)
attackOrder: [1, 3, 2]
m: 1
Total Substrings: (3 * 4) / 2 = 6
Time t=1:
The 1st element of attackOrder is 1. The 1st character is attacked.
The password becomes "*bc".
The malicious substrings are "*" , "*b", and "*bc". There are 3.
Is the count (3) >= m (1)? Yes.
The process stops. The minimum time is 1.

Sample Case 1

password: "bcdb" (n = 4)
attackOrder: [2, 4, 1, 3]
m: 10
Total Substrings: (4 * 5) / 2 = 10
```

### Function Description
Complete the function `findMinimumTime` in the editor below.  
`findMinimumTime` has the following parameters:
* string password: the initial password
* int attackOrder[n]: permutation array of integers [1, 2, ..., n], the attack order of the virus
* int m: the recoverability parameter

### Return
* int: the minimum time after which the password becomes irrecoverable.

Constraints  
* 1 ≤ n ≤ 8 * 10^5
* String password consists of lowercase English characters.
* 1 ≤ attackOrder[i] ≤ n
* 0 ≤ m ≤ n * (n + 1) / 2

## Thinking


## Coding

### Solution
```java
import java.io.*;
import java.util.*;

class Result {

    /**
     * A helper class to represent a contiguous block of un-attacked characters.
     * It's Comparable to be used in a TreeSet for efficient, sorted storage.
     */
    static class Block implements Comparable<Block> {
        int start;
        int end;

        public Block(int start, int end) {
            this.start = start;
            this.end = end;
        }

        // The TreeSet will order and find blocks based on their start index.
        @Override
        public int compareTo(Block other) {
            return Integer.compare(this.start, other.start);
        }
    }


    /**
     * Calculates the number of substrings in a block of a given length.
     * @param length The length of the block.
     * @return The total number of contiguous substrings.
     */
    private static long countSubstrings(long length) {
        if (length <= 0) {
            return 0;
        }
        return length * (length + 1) / 2;
    }


    /*
     * Complete the 'findMinimumTime' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     * 1. STRING password
     * 2. INTEGER_ARRAY attackOrder
     * 3. LONG m
     */
    public static int findMinimumTime(String password, List<Integer> attackOrder, long m) {
        int n = password.length();

        if (m == 0) {
            return 1;
        }

        long totalSubstrings = countSubstrings(n);
        
        // A TreeSet keeps blocks sorted, allowing efficient O(log k) lookup.
        TreeSet<Block> cleanBlocks = new TreeSet<>();
        
        // Initially, the entire password is one large "clean" block.
        cleanBlocks.add(new Block(1, n));
        long currentCleanSubstrings = totalSubstrings;

        // Simulate each attack second by second.
        for (int i = 0; i < n; i++) {
            int attackPos = attackOrder.get(i);

            // Find the block that contains the attack position.
            // floor() finds the greatest element <= the given element.
            Block blockToSplit = cleanBlocks.floor(new Block(attackPos, attackPos));

            if (blockToSplit != null && attackPos <= blockToSplit.end) {
                
                // 1. Remove the old block and its contribution to the clean count.
                cleanBlocks.remove(blockToSplit);
                long originalLength = blockToSplit.end - blockToSplit.start + 1;
                currentCleanSubstrings -= countSubstrings(originalLength);

                // 2. Create the left part as a new block, if it's not empty.
                int leftEnd = attackPos - 1;
                if (leftEnd >= blockToSplit.start) {
                    Block leftBlock = new Block(blockToSplit.start, leftEnd);
                    cleanBlocks.add(leftBlock);
                    currentCleanSubstrings += countSubstrings(leftBlock.end - leftBlock.start + 1);
                }

                // 3. Create the right part as a new block, if it's not empty.
                int rightStart = attackPos + 1;
                if (rightStart <= blockToSplit.end) {
                    Block rightBlock = new Block(rightStart, blockToSplit.end);
                    cleanBlocks.add(rightBlock);
                    currentCleanSubstrings += countSubstrings(rightBlock.end - rightBlock.start + 1);
                }
            }

            // 4. Check if the irrecoverable condition is met.
            if (totalSubstrings - currentCleanSubstrings >= m) {
                return i + 1; // Return the current time (1-based index).
            }
        }

        return n;
    }
}
```
