## Question
Say you have an array for which the ith element is the price of a given stock on day i.<br>

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.<br>

Note that you cannot sell a stock before you buy one.<br>

**Example 1:**
<pre>
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
</pre>

**Example 2:**
<pre>
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
</pre>

## Thinking
While it only allows 1 transaction, we try to find the minimum price and the max profit.

## Coding
Time: O(n); Go through the array once. </br>
Space: O(1) 
```python3
class Solution:
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        
        max_profit, min_price = 0, float('inf')
        
        for price in prices:
            min_price = min(min_price, price) # The minimum price overall 
            profit = price - min_price # The current profit
            max_profit = max(max_profit, profit) # Compare the current profit with the overall max profit
        return max_profit    
```

