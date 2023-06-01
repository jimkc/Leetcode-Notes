## Question
Given an integer rowIndex, return the rowIndexth (0-indexed) row of the Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:
https://leetcode.com/problems/pascals-triangle-ii/description/

**Example 1:**
<pre>
Input: rowIndex = 3
Output: [1,3,3,1]
</pre>

**Example 2:**
<pre>
Input: rowIndex = 0
Output: [1]
</pre>

**Example 3:**
<pre>
Input: rowIndex = 1
Output: [1,1]
</pre>

## Thinking


## Coding
Time: O(n**2); </br>
Space: O(n)

### Dynamic Programming
```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> row  = new ArrayList<>();

        // add the most left index, also create for row 0
        row.add(1);

        // i for row index, j for column index
        for (int i = 1; i <= rowIndex; i++)
        {
            // from the column rightest-1 to the leftest+1
            for (int j = i-1; j > 0; j-- )
            {
                row.set(j, row.get(j) + row.get(j-1));
            }
            // add the rightest index
            row.add(1);
        }

        return row;
    }
}
```

**Another Solution that uses array**  
More efficient in time, also O(n) as well
```java
public class Solution {
    public List<Integer> getRow(int k) {
        Integer[] arr = new Integer[k + 1];
        Arrays.fill(arr, 0);
        arr[0] = 1;
        
        for (int i = 1; i <= k; i++) 
            for (int j = i; j > 0; j--) 
                arr[j] = arr[j] + arr[j - 1];
        
        return Arrays.asList(arr);
    }
}
```

