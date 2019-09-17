## Question
The set [1,2,3,...,n] contains a total of n! unique permutations.<br>

By listing and labeling all of the permutations in order, we get the following sequence for n = 3:<br>

"123"<br>
"132"<br>
"213"<br>
"231"<br>
"312"<br>
"321"<br>
Given n and k, return the kth permutation sequence.<br>

Note:<br>

Given n will be between 1 and 9 inclusive.<br>
Given k will be between 1 and n! inclusive.

**Example :**   
<pre>
Input: n = 3, k = 3
Output: "213"

Input: n = 4, k = 9
Output: "2314"
</pre>

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
import math
class Solution:
    # @param {integer} n
    # @param {integer} k
    # @return {string}
    def getPermutation(self, n, k):
        numbers = list(range(1, n+1))
        permutation = ''
        k -= 1 # Let all possibility in range 0~k-1
        while n > 0:
            
            n -= 1 # Because we want to know how many sets of permutation for i-1~0 can permuatate for ith position
            
            # get the index of current digit
            index, k = divmod(k, math.factorial(n)) # The idxth biggest number

            permutation += str(numbers[index])
            # remove handled number
            numbers.pop(index) # Index number is used cant be select again

        return permutation
```

