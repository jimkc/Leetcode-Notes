### Link
https://www.1point3acres.com/bbs/thread-1095586-1-1.html

### Question
Robinhood is famous for its referral program. It’s exciting to see our users spreading the word across their friends and family. One thing that is interesting about the program is the network effect it creates. We would like to build a dashboard to track the status of the program. Specifically, we would like to learn about how people refer others through the chain of referral.  
  
For the purpose of this question, we consider that a person refers all other people down the referral chain. For example, A refers B, C, and D in a referral chain of A -> B -> C -> D. Please build a leaderboard for the top 3 users who have the most referred users along with the referral count.  
  
Referral rules:  
A user can only be referred once.  
Once the user is on the RH platform, he/she cannot be referred by other users. For example: if A refers B, no other user can refer A or B since both of them are on the RH platform.  
Referrals in the input will appear in the order they were made.  
Leaderboard rules:  

1. The user must have at least 1 referral count to be on the leaderboard.
2. The leaderboard contains at most 3 users.
3. The list should be sorted by the referral count in descending order.
4. If there are users with the same referral count, break the ties by the alphabetical order of the user name.
```
Input
rh_users = ["A", "B", "C"]
| | |
v v v
new_users = ["B", "C", "D"]

Output
["A 3", "B 2", "C 1"]
[execution time limit] 3 seconds (java)
[memory limit] 1 GB

[input] array.string rh_users
A list of referring users.
[input] array.string new_users
A list of user that was referred by the users in the referrers array with the same order.
[output] array.string
An array of 3 users on the leaderboard. Each of the element here would have the "[user] [referral count]" format. For example, "A 4".
```

### Solution
```java
import java.util.*;
import java.util.stream.Collectors;

class Solution {
    /**
     * Calculates the top 3 referrers based on a list of referral events.
     * The solution builds a graph of referrals and uses a Depth-First Search (DFS)
     * with memoization to efficiently count all downstream referrals for each user.
     *
     * @param rh_users  An array of users who made a referral.
     * @param new_users An array of users who were referred. The i-th user in rh_users
     * referred the i-th user in new_users.
     * @return An array of strings representing the leaderboard, with up to 3 users.
     * Each string is in the format "[user] [referral count]".
     */
    public String[] findTop3Referrers(String[] rh_users, String[] new_users) {
        // Step 1: Build the referral graph as an adjacency list.
        // The keys of this map are all users who have referred at least one person.
        Map<String, List<String>> adjList = new HashMap<>();
        for (int i = 0; i < rh_users.length; i++) {
            adjList.computeIfAbsent(rh_users[i], k -> new ArrayList<>()).add(new_users[i]);
        }

        // Step 2: Calculate referral counts for each potential leaderboard candidate.
        // We use DFS with memoization to avoid re-calculating counts.
        // The memoization map stores the total downstream referral count for each user.
        Map<String, Integer> referralCounts = new HashMap<>();

        // Start DFS from each user who has made at least one referral.
        // This ensures all referral chains are processed.
        for (String user : adjList.keySet()) {
            if (!referralCounts.containsKey(user)) {
                dfsCount(user, adjList, referralCounts);
            }
        }

        // Step 3: Create a list of candidates from the calculated counts.
        // Per the rules, a user must have at least 1 referral to be on the leaderboard.
        List<Map.Entry<String, Integer>> leaderboardList = referralCounts.entrySet()
            .stream()
            .filter(entry -> entry.getValue() > 0)
            .collect(Collectors.toList());

        // Step 4: Sort the list based on the leaderboard rules.
        leaderboardList.sort((a, b) -> {
            // Primary sort: by referral count, descending.
            int countCompare = b.getValue().compareTo(a.getValue());
            if (countCompare != 0) {
                return countCompare;
            }
            // Secondary sort (tie-breaker): by user name, alphabetical ascending.
            return a.getKey().compareTo(b.getKey());
        });

        // Step 5: Format the top 3 (or fewer) results into the required string array.
        int resultSize = Math.min(3, leaderboardList.size());
        String[] result = new String[resultSize];
        for (int i = 0; i < resultSize; i++) {
            Map.Entry<String, Integer> entry = leaderboardList.get(i);
            result[i] = entry.getKey() + " " + entry.getValue();
        }

        return result;
    }

    /**
     * Recursively calculates the total number of downstream referrals for a user
     * using Depth-First Search and stores the result in a memoization map.
     *
     * @param user    The user for whom to calculate the referral count.
     * @param adjList The referral graph.
     * @param memo    The memoization map to store calculated counts.
     * @return The total downstream referral count for the user.
     */
    private int dfsCount(String user, Map<String, List<String>> adjList, Map<String, Integer> memo) {
        // If the count is already calculated, return it from the cache.
        if (memo.containsKey(user)) {
            return memo.get(user);
        }

        // Base case: A user not in the adjacency list's keys has no direct
        // referrals, so their downstream count is 0.
        if (!adjList.containsKey(user)) {
            memo.put(user, 0);
            return 0;
        }

        int count = 0;
        // Iterate over all users directly referred by the current user.
        for (String referredUser : adjList.get(user)) {
            // Add 1 for the direct referral.
            count++;
            // Add the entire downstream count of the referred user recursively.
            count += dfsCount(referredUser, adjList, memo);
        }

        // Cache the result before returning.
        memo.put(user, count);
        return count;
    }
}
```