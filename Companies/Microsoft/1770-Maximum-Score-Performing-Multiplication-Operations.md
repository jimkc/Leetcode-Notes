## Question
You are given two integer arrays nums and multipliers of size n and m respectively, where n >= m. The arrays are 1-indexed.  
  
You begin with a score of 0. You want to perform exactly m operations. On the ith operation (1-indexed), you will:  
  
* Choose one integer x from either the start or the end of the array nums.
* Add multipliers[i] * x to your score.
* Remove x from the array nums.  

Return the maximum score after performing m operations. 
  
**Example 1:**   
<pre>
Input: nums = [1,2,3], multipliers = [3,2,1]
Output: 14
Explanation: An optimal solution is as follows:
- Choose from the end, [1,2,3], adding 3 * 3 = 9 to the score.
- Choose from the end, [1,2], adding 2 * 2 = 4 to the score.
- Choose from the end, [1], adding 1 * 1 = 1 to the score.
The total score is 9 + 4 + 1 = 14.
</pre>
  
**Example 2:**   
<pre>
Input: nums = [-5,-3,-3,-2,7,1], multipliers = [-10,-5,3,4,6]
Output: 102
Explanation: An optimal solution is as follows:
- Choose from the start, [-5,-3,-3,-2,7,1], adding -5 * -10 = 50 to the score.
- Choose from the start, [-3,-3,-2,7,1], adding -3 * -5 = 15 to the score.
- Choose from the start, [-3,-2,7,1], adding -3 * 3 = -9 to the score.
- Choose from the end, [-2,7,1], adding 1 * 4 = 4 to the score.
- Choose from the end, [-2,7], adding 7 * 6 = 42 to the score. 
The total score is 50 + 15 - 9 + 4 + 42 = 102.
</pre>
  
**Constraints:**
* n == boxes.length
* 1 <= n <= 2000
* boxes[i] is either '0' or '1'.
  
## Thinking
* We can easy to realise that this is a Dynamic Programming problem. In operation ith, we can choose to pick the left or the right side of nums.  
* The naive dp has 3 params which is dp(l, r, i), time complexity = O(m^3), l and r can be picked up to m numbers.
* We can optimize to 2 params which is dp(l, i), time complexity = O(m^2) , we can compute params r base on l and i:  

* Where:  
 * l, r is the index of the left side and the right side corresponding.
 * i is the number of elements that we picked.
 * leftPicked: is the number of elements picked on the left side, leftPicked = l.
 * rightPicked: is the number of elements picked on the right side, rightPicked = i - leftPicked = i - l.
 * Finally: r = n - rightPicked - 1 = n - (i-l) - 1.

## Coding
Time: O(2 * m^2);  
Space: O(m^2)

```Java
class Solution {
    int n, m;
    int[] nums, muls;
    Integer[][] memo;
    public int maximumScore(int[] nums, int[] muls) {
        n = nums.length;
        m = muls.length;
        this.nums= nums;
        this.muls = muls;
        this.memo = new Integer[m][m];
        return dp(0, 0);
    }
    // l is count of numbers picked from the left of nums, also the leftest index we can get from nums
    // i is count of numbers picked from nums
    // i - l is count of numbers picked from right
    private int dp(int l, int i) {
        if (i == m) return 0; // Picked enough m elements
        if (memo[l][i] != null) return memo[l][i];
        // dp(l+1, i+1) is the next (leftest index, numbers left) after the pick from left
        int pickLeft = dp(l+1, i+1) + nums[l] * muls[i]; // Pick the left side
        // dp(l, i+1) is the next (leftest index, numbers left) after the pick from right
        int pickRight = dp(l, i+1) + nums[n-1-(i-l)] * muls[i]; // Pick the right side
        return memo[l][i] = Math.max(pickLeft, pickRight);
    }
}
```

