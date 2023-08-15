## Question
A sequence of numbers is called an arithmetic progression if the difference between any two consecutive elements is the same.
Given an array of numbers arr, return true if the array can be rearranged to form an arithmetic progression. Otherwise, return false.

https://leetcode.com/problems/can-make-arithmetic-progression-from-sequence/description/

**Example 1:**
<pre>
Input: arr = [3,5,1]
Output: true
Explanation: We can reorder the elements as [1,3,5] or [5,3,1] with differences 2 and -2 respectively, between each consecutive elements.
</pre>

**Example 2:**
<pre>
Input: arr = [1,2,4]
Output: false
Explanation: There is no way to reorder the elements to obtain an arithmetic progression.
</pre>

## Thinking


## Coding
Time: O(n); </br>
Space: O(n)

### Solution
```java
class Solution {
    public boolean canMakeArithmeticProgression(int[] arr)
    {
        final int size = arr.length;

        int minNumber = Integer.MAX_VALUE;
        int maxNumber = Integer.MIN_VALUE;

        for (int num: arr)
        {
            if(num < minNumber)
            {
                minNumber = num;
            }
            if(num > maxNumber)
            {
                maxNumber = num;
            }
        }

        if ((maxNumber - minNumber) % (size-1) != 0)
        {
            return false;
        }

        // difference between the numbers
        int d = (maxNumber - minNumber)/(size - 1);

        int cursor = 0;
        while (cursor < size)
        {
            if (arr[cursor] == minNumber + d*cursor)
            {
                cursor++;
            }
            else if ((arr[cursor] - minNumber) % d != 0)
            {
                return false;
            }
            else
            {
                int correctPos = (arr[cursor] - minNumber) / d;
                // avoid deadlock
                if (correctPos < cursor || arr[cursor] == arr[correctPos])
                {
                    return false;
                }
                
                int tmp = arr[correctPos];
                arr[correctPos] = arr[cursor];
                arr[cursor] = tmp;
            }
        }
        return true;

    }
}
```


