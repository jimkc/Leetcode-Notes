## Question
Given a triangle array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index i on the current row, you may move to either index i or index i + 1 on the next row.
https://leetcode.com/problems/triangle/description/

**Example 1:**
<pre>
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).
</pre>

**Example 2:**
<pre>
Input: triangle = [[-10]]
Output: -10
</pre>

## Thinking


## Coding
Time: O(n**2); </br>
Space: O(n)

### Dynamic Programming
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle)
    {
        if (triangle.size() == 1)
        {
            return triangle.get(0).get(0);
        }

        // Initialize the dp array with the values from the bottom row
        final int rows = triangle.size();
        int[] dp = new int[rows];
        List<Integer> lastRow = triangle.get(rows-1);
        for (int i=0; i<dp.length; i++)
        {
            dp[i] = lastRow.get(i);
        }

        // Traverse the triangle from bottom to top
        for (int j=rows-2; j>=0; j--)
        {
            List<Integer> currentRow = triangle.get(j);

            for (int i=0; i<currentRow.size(); i++)
            {
                // calculate the minimum path for each position
                dp[i] = currentRow.get(i) + Math.min(dp[i], dp[i+1]);
            }
        }

        return dp[0];
    }
}
```


