## Link
https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=1132096&highlight=amazon%2Boa

## Question
A team of developers at Amazon is working on a feature that checks the strength of a password as it is set by a user.
Analysts found that people use their personal information in passwords, which is insecure. The feature searches for the presence of a reference string as a subsequence of any permutation of the password. If any permutation contains the reference string as a subsequence, then the feature determines the minimum cost to remove characters from password so that no permutation contains the string reference as a subsequence.  

The removal of any character has a cost given in an array, cost, with 26 elements that represent the cost to replace 'a' (cost[0]) through 'z' (cost[25]). Given two strings password and reference, determine the minimum total cost to remove characters from password so that no permutation contains the string reference as a subsequence.  

Note:  
* A permutation is a sequence of integers from 1 to n of length n containing each number exactly once, for example, [1, 3, 2] is a permutation while [1, 2, 1] is not.
* A subsequence is a sequence that can be derived from another sequence by deleting some or no elements, without changing the order of the remaining elements. For example, given the string "abcde", the following are subsequences: "a", "ace", "be", "bde" etc. while sequences like "dea", "abcde" are not subsequences.
].  

## Thinking
First, let me understand the problem:  

1. We have a password and a reference string  
2. We need to ensure no permutation of the password contains the reference string as a subsequence  
3. We can remove characters from the password at given costs  
4. We want to minimize the total removal cost  
  
The key insight is that if we have enough characters of each type needed for the reference string, we can form it as a subsequence in some permutation. So we need to reduce the count of at least one character type that appears in the reference string.  

## Coding

### Solution
```java
import java.util.*;

public class PasswordStrengthener {
    
    public static int minCostToStrengthen(String password, String reference, int[] cost) {
        // Count frequency of each character in password
        int[] passwordCount = new int[26];
        for (char c : password.toCharArray()) {
            passwordCount[c - 'a']++;
        }
        
        // Count frequency of each character in reference
        int[] referenceCount = new int[26];
        for (char c : reference.toCharArray()) {
            referenceCount[c - 'a']++;
        }
        
        // If we don't have enough characters to form the reference string,
        // no permutation can contain it as subsequence
        boolean canFormReference = true;
        for (int i = 0; i < 26; i++) {
            if (passwordCount[i] < referenceCount[i]) {
                canFormReference = false;
                break;
            }
        }
        
        if (!canFormReference) {
            return 0; // No cost needed
        }
        
        // We need to remove characters to make it impossible to form reference
        // For each character in reference, calculate cost to remove enough instances
        int minCost = Integer.MAX_VALUE;
        
        for (int i = 0; i < 26; i++) {
            if (referenceCount[i] > 0) {
                // Calculate cost to remove enough of character i so that
                // we can't form the reference string
                int needToRemove = passwordCount[i] - referenceCount[i] + 1;
                int costToRemove = needToRemove * cost[i];
                minCost = Math.min(minCost, costToRemove);
            }
        }
        
        return minCost;
    }
    
    public static void main(String[] args) {
        // Test case 1
        String password1 = "abcde";
        String reference1 = "ace";
        int[] cost1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26};
        
        System.out.println("Test case 1:");
        System.out.println("Password: " + password1);
        System.out.println("Reference: " + reference1);
        System.out.println("Min cost: " + minCostToStrengthen(password1, reference1, cost1));
        System.out.println();
        
        // Test case 2
        String password2 = "aabbcc";
        String reference2 = "abc";
        int[] cost2 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26};
        
        System.out.println("Test case 2:");
        System.out.println("Password: " + password2);
        System.out.println("Reference: " + reference2);
        System.out.println("Min cost: " + minCostToStrengthen(password2, reference2, cost2));
        System.out.println();
        
        // Test case 3 - Reference can't be formed
        String password3 = "abc";
        String reference3 = "abcd";
        int[] cost3 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26};
        
        System.out.println("Test case 3:");
        System.out.println("Password: " + password3);
        System.out.println("Reference: " + reference3);
        System.out.println("Min cost: " + minCostToStrengthen(password3, reference3, cost3));
    }
}
```
